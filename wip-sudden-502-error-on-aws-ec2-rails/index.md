```metadata
{
    "title": "AWS EC2 + Railsで急に502エラーが発生した",
    "date": "2022-01-15T19:43:27",
    "categories": "AWS, EC2, Ruby on Rails",
}
```

何もしていないけど、急に502のエラー画面が出現してlogを漁ってみたところ

```json
 (13: Permission denied)
```

が表示されました、。。。

どうやら自分の場合はunicornのsocketにうまく接続できていないみたいで、プロセスをkillして

```json
 sudo chmod 777 /var/www
```

からの再デプロイ、で解決できました！

## 参考記事

[【ALB unicorn nginx】急に502 Bad Gatewayが出て直らないとき](https://qiita.com/ichihara-development/items/37ae6819e53d936fb03d)
