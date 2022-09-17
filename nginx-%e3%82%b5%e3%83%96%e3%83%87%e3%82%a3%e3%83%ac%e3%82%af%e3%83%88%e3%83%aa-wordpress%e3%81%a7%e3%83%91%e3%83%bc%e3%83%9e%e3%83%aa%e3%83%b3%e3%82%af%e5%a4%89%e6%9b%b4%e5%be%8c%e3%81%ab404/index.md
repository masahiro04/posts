```metadata
{
    "title": "Nginx + サブディレクトリ wordpressでパーマリンク変更後に404が出る",
    "date": "2022-01-29T01:38:36",
    "categories": "AWS, EC2, Nginx",
}
```

```
 /magazine でwordpressを設置しているプロジェクトでパーマリンクを変更したところ、タイトルのようなエラーが発生しました
```

とりあえず結論

```nginx
 location /magazine {
  # alias /var/www/html/blog;
  root /var/www/html;
  try_files $uri $uri/ /magazine/index.php?q=$request_uri;
}
```

```
 元々は/blog で運用することを検討していたので、EC2の中のディレクトリは
```

- /var/www/app -> Rails
- /var/www/html/blog -> wordpress

で構成しておりました

```
 しかし途中で /blog ではなく/magazine で運用したいという要望が出てきたため、location部分を変更したのですが、タイトルのエラーに遭遇、という流れです
```

```
 で、やったこととしては、/blog をディレクトリ丸々コピーして /magazine として作成、そして上記のように振り向けることで解決、となります
```

## 参考記事

[nginx fails to load wordpress subfolder using different root location](https://serverfault.com/questions/845840/nginx-fails-to-load-wordpress-subfolder-using-different-root-location)
