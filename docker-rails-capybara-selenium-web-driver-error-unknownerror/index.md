```metadata
{
    "title": "docker + rails + capybaraでSelenium::WebDriver::Error::UnknownError",
    "date": "2022-04-28T23:10:16",
    "categories": "Docker, Ruby on Rails",
}
```

dockerでRailsのcapybaraテストをjs trueで実装したところ、タイトルのエラーが発生しました

とりあえずこちらが実行した手順

```docker
 # まずはdockerのプロセス出す
docker ps

# 該当のコンテナを見つけて下記のようにweb_1の場所に記述
docker exec -it web_1 /bin/bash

# 入ったら下記のコマンド(google-chrome)を打ち込み、chromeの状態を調べる
root@cb7713727f8b:/app# google-chrome
[27:27:0428/113240.201687:ERROR:zygote_host_impl_linux.cc(90)] Running as root without --no-sandbox is not supported. See https://crbug.com/638180.
```

実行したところ–no-sandboxがつけてください、とのことなので下記のようにchrome driverに設定つけて実行してみます

すると下記のようにエラーが出てきました

```docker
 root@4cde53c2b35e:/app# google-chrome --version
Google Chrome 101.0.4951.41
root@4cde53c2b35e:/app# google-chrome --headless --no-sandbox
qemu: uncaught target signal 5 (Trace/breakpoint trap) - core dumped
qemu: uncaught target signal 5 (Trace/breakpoint trap) - core dumped
[0428/122431.553682:ERROR:bus.cc(398)] Failed to connect to the bus: Failed to connect to socket /run/dbus/system_bus_socket: No such file or directory
[0428/122431.566936:ERROR:file_path_watcher_linux.cc(321)] inotify_init() failed: Function not implemented (38)
[0428/122431.570218:ERROR:bus.cc(398)] Failed to connect to the bus: Failed to connect to socket /run/dbus/system_bus_socket: No such file or directory
qemu: unknown option 'type=utility'
[
```

下記の記事を調べてみたところ、

[wsl上でheadless chrome を起動すると Failed to connect to the bus: Failed to connect to socket /var/run/dbus/system_bus_socket: No such file or directory](https://qiita.com/yugo-yamamoto/items/4b3286ab9c67c5f40080)

こちらのコマンドを実行すると良い、とのことなので実行

```docker
 /etc/init.d/dbus start
```

 そしてもう一度chrome driverを実行したところ、上記のエラーは解決できて新たにエラーが出現

```docker
 root@4cde53c2b35e:/app# google-chrome --headless --no-sandbox
qemu: uncaught target signal 5 (Trace/breakpoint trap) - core dumped
qemu: uncaught target signal 5 (Trace/breakpoint trap) - core dumped
[0428/122811.053535:ERROR:file_path_watcher_linux.cc(321)] inotify_init() failed: Function not implemented (38)
qemu: unknown option 'type=utility'
[0428/122811.199162:WARNING:bluez_dbus_manager.cc(248)] Floss manager not present, cannot set Floss enable/disable.
[0428/122811.200983:ERROR:gpu_process_host.cc(968)] GPU process launch failed: error_code=1002
[0428/122811.201179:WARNING:gpu_process_host.cc(1279)] The GPU process has crashed 1 time(s)
[0428/122811.245122:ERROR:network_service_instance_impl.cc(978)] Network service crashed, restarting service.
qemu: unknown option 'type=utility'
[0428/122811.384215:ERROR:gpu_process_host.cc(968)] GPU process launch failed: error_code=1002
[0428/122811.384293:WARNING:gpu_process_host.cc(1279)] The GPU process has crashed 2 time(s)
[0428/122811.392197:ERROR:network_service_instance_impl.cc(978)] Network service crashed, restarting service.
qemu: unknown option 'type=utility'
[
```

上記のエラーを調べている途中で問題判明

M1ではchrome driverのbuildがないっぽいので、railsが格納しているコンテナのDockerfile自体がダメぽいです

[https://zenn.dev/kenkenlysh/articles/78c1797b830053](https://zenn.dev/kenkenlysh/articles/78c1797b830053)

[M1 mac上のDockerコンテナ内でChromiumを動かそうとしてやったこと＆やろうとしてること](https://blog.savanna.io/entry/2021/12/06/182102)

ただ、M1用のimageもあるので、こちらを別でコンテナ作って、繋げてあげればいけるとのこと

なので、別コンテナ作成

```ruby
 # Dockerfile
chrome:
    // M1 dockerでも使えるやつ
    image: seleniarm/standalone-chromium
    ports:
      - '4444:4444'
web: &web
    // 省略
    environment:
      //こちら追加
      SELENIUM_DRIVER_URL: http://chrome:4444/wd/hub

# spec/supports/capybara.rb
Capybara.register_driver :remote_chrome do |app|
  url = ENV['SELENIUM_DRIVER_URL']
  caps = ::Selenium::WebDriver::Remote::Capabilities.chrome(
    'goog:chromeOptions' => {
      'args' => [
        'no-sandbox',
        'headless',
        'disable-gpu',
        'window-size=1680,1050'
      ]
    }
  )
  Capybara::Selenium::Driver.new(app, browser: :remote, url: url, desired_capabilities: caps)
end

RSpec.configure do |config|
  config.before(:each) do |example|
    if %i[system feature].include?(example.metadata[:type])
      if example.metadata[:js]
        Capybara.server_host = IPSocket.getaddress(Socket.gethostname)
        Capybara.app_host = "http://#{Capybara.server_host}"
        driven_by :remote_chrome
      else
        driven_by :rack_test
      end
    end
  end
end

```

これでM1 macでもchrome driver動かせました！
