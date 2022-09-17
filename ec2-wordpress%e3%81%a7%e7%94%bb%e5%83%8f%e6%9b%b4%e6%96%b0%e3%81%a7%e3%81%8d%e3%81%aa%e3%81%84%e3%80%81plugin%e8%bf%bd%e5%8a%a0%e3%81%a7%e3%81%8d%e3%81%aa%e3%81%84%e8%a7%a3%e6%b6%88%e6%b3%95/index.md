```metadata
{
    "title": "EC2 + wordpressで画像更新できない、plugin追加できない解消法",
    "date": "2022-01-29T19:05:46",
    "categories": "AWS, Nginx, WordPress",
}
```

EC2にwordpressを設定して、色々修正を加えているうちになぜか

- 画像が保存できない
- プラグインを追加できない

という問題が発生しました

結論、権限の問題だったらしく、下記の対応をすることによって解決できました🙏<br> ※バックアップは必須だと思うので、その点はご注意を

```nginx
 # まずはwordpressの権限をnginxに変更。nginxで配信してるので、ここは適宜変更する必要有りかもです
sudo chown -R nginx:nginx magazine

sudo chmod 755 plugins
sudo chmod 755 uploads
```

## 参考記事

[WordPress管理画面からプラグインがインストールできない問題](https://note.com/terumari/n/n1d61b4f20aff)

[https://caramelcase.com/wordpress-aws-ec2-nginx-php7-mysql/](https://caramelcase.com/wordpress-aws-ec2-nginx-php7-mysql/)
