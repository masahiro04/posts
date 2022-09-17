```metadata
{
    "title": "AWS + Rails + Capistrano でGemfile not found (Bundler::GemfileNotFound)出る",
    "date": "2022-01-14T00:16:01",
    "categories": "AWS, EC2, Ruby on Rails",
}```

Capistranoでデプロイできているようですが、サーバー起動せずlog見たらタイトルのエラー出てました

調べてみるとどうやら、Gemfileの参照がうまくいっていないようなので、下記の設定を追加することで参照先を固定して問題を解決しました！

```json
 # config/unicorn.rb
before_exec do |server|
  ENV['BUNDLE_GEMFILE'] = @app_path + "/current/Gemfile"
end
```


