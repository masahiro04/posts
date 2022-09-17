```metadata
{
    "title": "AWS EC2でRailsの環境変数更新したのに反映されない問題の解消法",
    "date": "2022-01-18T17:05:24",
    "categories": "AWS, EC2, Ruby on Rails",
}```

EC2の環境変数を更新したにもかかわらずなぜか環境変数が更新されずに、S3に繋げれない、と言う問題が発生しました。。。。

Herokuだと変更するたびにbuildが走るのでAWSも勝手に完了するものかと思っていましたが、<br>どうやらunicorn等のサーバーを再起動しないとダメらしく、こちらのコマンド実行で対処しました

```ruby
 ps aux | grep unicorn

# pidを全部killする
kill 1111

# unicorn再起動
unicorn_rails -c config/unicorn.rb -E production -D
```

## 参考記事

[AWSのEC2で環境変数(ENV)を読み込めない](https://teratail.com/questions/226685)
