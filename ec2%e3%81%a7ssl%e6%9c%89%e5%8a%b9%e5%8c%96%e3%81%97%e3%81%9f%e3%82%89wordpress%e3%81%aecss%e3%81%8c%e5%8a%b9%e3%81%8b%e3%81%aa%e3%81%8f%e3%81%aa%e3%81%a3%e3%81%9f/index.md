```metadata
{
    "title": "EC2でSSL有効化したらWordPressのCSSが効かなくなった",
    "date": "2022-01-21T16:33:29",
    "categories": "EC2, WordPress",
}
```

ACMでSSLを有効化したら、EC2インスタンスで動いているWordPressのCSSが動かなくなってしましました

結論wordpressのwp-config.phpにこちらを追加することで直ります

```php
 $_SERVER['HTTPS'] = 'on';
$_ENV['HTTPS'] = 'on';
```

## 参考記事

[WordPressでSSL化したとき、CSSやJavaScriptがhttpのままで読み込めないときの対処法](https://frostmoon.net/?p=641)
