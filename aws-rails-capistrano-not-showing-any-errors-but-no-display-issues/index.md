```metadata
{
    "title": "AWS + Rails + Capistranoで何もエラーないのに表示されない問題",
    "date": "2022-01-14T01:50:36",
    "categories": "AWS, EC2, Ruby on Rails",
}
```

タイトルの通りで、

- Rails
- AWS EC2
- capistrano

で自動デプロイを実現し、Elastic IPで固定したIPにアクセスしているにもかかわらず、全く何も表示されない<br>という謎現象が発生しました。。。。

で、リンクを確認してcurlしようとしたら、リンクが！！！！

```json
 curl -IXGET https://xx.xxx.xxx.xx/
```

httpではなく<strong>https</strong>になってました!!!!!

どうやらenvironment系のところで

```ruby
 # condig/environments/production.rb
  config.force_ssl = true
```

SSL強制化されてるみたいで、URLをコピるとなぜかhttpsにリダイレクトされてました

なので、上記をfalseに変更し、デプロイしてみました



こちらが、SSL強制化されてた時のcurl

```json
 $ (x86_64)curl -IXGET http://xx.xxx.xxx.xx/
HTTP/1.1 301 Moved Permanently
Server: nginx/1.20.0
Date: Thu, 13 Jan 2022 16:19:41 GMT
Content-Type: text/html
Content-Length: 0
Connection: keep-alive
Location: https://xx.xxx.xxx.xx/
```

こちらがSSL強制化せずにhttp許容した時のcurl

status 200で帰ってきています！！！

```json
 $ (x86_64)curl -IXGET http://xx.xxx.xxx.xx/
HTTP/1.1 200 OK
Server: nginx/1.20.0
Date: Thu, 13 Jan 2022 16:35:23 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 44617
Connection: keep-alive
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Permitted-Cross-Domain-Policies: none
Referrer-Policy: strict-origin-when-cross-origin
Link: </packs/css/application-b171b713.css>; rel=preload; as=style; nopush,</packs/js/application-7afb58b9a494bcaea598.js>; rel=preload; as=script; nopush
Vary: Accept
ETag: W/"6fa88ded29571f01eeb7abda0a4163d6"
Cache-Control: max-age=0, private, must-revalidate
Set-Cookie: _driver_app_session=TveHf8x1%2BTOIoa99u9JSKQwfW0t%2FcMGAXfoiQnVSnvSc5hYdsazCzMpGAP35yNq7TetZsRYS8JJ24WiLzQ1tCzmcSC5fldSHA%2BqA8Upp1o%2FaOO51WmU9rcwBV3lKhdzIHM%2Fxq0aYYGX8Jk9QlZ1Sc3TGcYyHwCLrPG5nzmk0aXM5d785RHnLW9nh8W9MI2NXbrkDl1%2FIOkjU3jUTlidW6mEs3ngEN10cd6majXuzIackgOXcJ4BimYy7PkSAtWsmr%2FRu1hqSahpS%2BOtD6K72sRgyouyD3YhqewYc--RUwMmoZJgeSmRZ8b--6ijMtvVGaI8HGUBqLZPoTw%3D%3D; path=/; HttpOnly; SameSite=Lax
X-Request-Id: 8939aab1-4a98-4060-8669-053a804fca84
X-Runtime: 0.261474
```

200で返ってきたことを確認したら、ブラウザで

```json
 http://xx.xxx.xxx.xx/
```

にアクセスするとRailsアプリが表示されています!!!!!!!

httpsではなく、<strong>http</strong>なのでここは必ず確認したください！！！！

もしうまく動かなければ、unicornのプロセスを削除した上で再度デプロイなどしてみると動くこともあるかもです！！！！！

## 参考記事

[EC2にデプロイするも、エラーログに何も表示されない時の可能性](https://qiita.com/hiroki_404_/items/025e681c22faf5f76736)
