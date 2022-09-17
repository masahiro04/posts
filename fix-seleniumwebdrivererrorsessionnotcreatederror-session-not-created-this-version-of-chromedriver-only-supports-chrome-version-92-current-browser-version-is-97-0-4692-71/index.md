```metadata
{
    "title": "Selenium::WebDriver::Error::SessionNotCreatedError: session not created: This version of ChromeDriver only supports Chrome version 92 Current browser version is 97.0.4692.71の解決方法",
    "date": "2022-01-17T23:46:52",
    "categories": "Chromedriver, Rspec, Ruby on Rails",
}
```

定期的にタイトルのエラーが発生するのですが、いつも苦労して気がついたら治ってて。。。。<br>みたいなことの連続なので、今回はメモとして残してみました

## 根本の問題

そのままでchrome driverのversionがあっていない、が問題です

ただ、個人的に気になったのは、

```ruby
 brew uninstall chromedriver
```

しても、なぜか、タイトルのエラーが発生してどうやらbrew経由でinstallしたchrome driverとは関係なさそうな感じ。。。

## 解決策

ググってみたらこんな記事発見

[RSpecでchromedriverとChromeのバージョンが合わない](https://qiita.com/sakakinn/items/dc5d588df87c054554be)

chromedriver-helperと言うgemがサポート終了しても入っていると、それが原因でversion違いが起こってるのでは？と言うことが書いてありました

なのでこちらを実行

```ruby
 gem list

....
chromedriver-helper (2.1.1)
...
```

なんと入ってました！！！！

なので、こちらをdeleteします<br>※ちなみにGemfileにも入っていたのでコメントアウトしました🙌

```ruby
 gem uninstall chromedriver-helper
```

その後、rspec走らせたら動きました!!!!!!!!

```ruby
 Registrations
  User
    password correct
2022-01-17 23:44:44 WARN Selenium [DEPRECATION] [:browser_options] :options as a parameter for driver initialization is deprecated. Use :capabilities with an Array of value capabilities/options if necessary instead.
      succeeds

Top 1 slowest examples (32.77 seconds, 100.0% of total time):
  Registrations User password correct succeeds
    32.77 seconds ./spec/system/users/registrations_system_spec.rb:12

Finished in 32.78 seconds (files took 2.74 seconds to load)
1 example, 0 failures

Randomized with seed 30581

Coverage report generated for RSpec to /Users/masahirookubo/client/world_alive/driver_app/coverage. 210 / 867 LOC (24.22%) covered.


```
