```metadata
{
    "title": "AWS EC2でRailsの/blogルートでWordPressを表示する",
    "date": "2022-01-16T17:07:46",
    "categories": "AWS, EC2, Nginx, Ruby on Rails",
}
```

```
 Railsの/blog ルートでwordpressを表示しようと思ったら、思いの他詰まってしまったので、後続の方もいらっしゃるかと思って、記事にまとめました🙏
```

そもそもですが、最近のトレンドでは

- システム -> http://example.com/
- ブログ -> http://blog.example.com/

で実装することが多いと思いますが、ちょっとした好奇心と、お客様でもしかしたらやりたいかも？<br>みたいなタイミングが一致したのでチャレンジした、という経緯です

最終的にはこのようなurl実装

<meta charset="utf-8">
<li>システム -&gt; http://example.com/</li>



<li>ブログ -&gt; http://example.com/blog</li>




一応見出しでわかりやすくまとめたつもりですが、<br>いきなり着手せずに、まずは下までスクロールして落ち着いて確認してください🙏

## 実現方法

結論、nginxのリバースプロキシで実現が可能です

参考にしたのはこちらの記事

[AWS上でRailsアプリとWordPressをnginxで共存させる](https://tori-kichi.com/blog/?p=134)

上記のリンクを参考にして実装しましたが、最終的にnginxのconfはこんな感じになりました

```nginx
 upstream unicorn {
        server unix:/var/www/your_app_name/shared/tmp/sockets/unicorn.sock;
}

server {
        listen       80;
        server_name  xx.xxx.xxx.xxx;
        root         /var/www/your_app_name/current/public;

        location /blog {
                index   index.html index.htm index.php;
                alias /var/www/html/blog;
                location ~ \.php$ {
                        fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
                        fastcgi_index  index.php;
                        fastcgi_param SCRIPT_FILENAME $request_filename;
                        include        fastcgi_params;
                }
        }

        location ^~ /assets/ {
                gzip_static on;
                expires max;
                add_header Cache-Control public;
        }

        location @unicorn {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;

                proxy_redirect off;
                 proxy_pass http://unicorn;
        }

        try_files $uri/index.html $uri @unicorn;
        error_page 500 502 503 504 /500.html;
}
```

## 要点の説明

要点を説明するにあたって、どのように実装したか？の参考記事がこちら

### Railsについて

[【Rails】 AWSのEC2にデプロイする方法~画像で丁寧に解説！](https://pikawaka.com/rails/ec2_deploy)

[Rails】 Capistranoを使ってデプロイを自動化しよう](https://pikawaka.com/rails/capistrano)

### WordPressについて

[Amazon EC2(Amazon Linux)でWordPressをインストールし、Nginxで表示させる手順2「MySQLサーバーの設定」という項目は下記の記事を参考にしています現在はMySQLではなくMariadbが主流のようです](https://owani.net/2015/12/16/wordpress-nginx/)

[【AWS】EC2にWordPressをインストール(構築)する](https://kacfg.com/ec2-wordpress/)

### どうやったか

それぞれの実装は上記を参考にすれば、手順通り進めていけば問題なく実装可能かと思います

おそらくこの記事を読んでいる方は、「Railsはできたけど、wordpressを埋め込めない」という状況かなと



私も同じ状況で、とった手順としては

- まずはwordpressを/var/www/htmlに展開 -> nginx経由で公開
- その後に/var/www/html/blogをmkdirして、そこに作成したwordpressファイルを移動
- nginxでlocation /blog/を追加して、まずはwordpressだけを公開
- その後にrailsをrootで使えるようにする

という流れです

この方法に変更したところ、詰まっていたことが嘘のようにとんとん拍子に進みました

やはりRailsを実装してからだと、Railsの影響範囲が大きいですし、問題の場所がわからなくなってしまうので、もし自分がもう一度実装するとしても、上記の流れで行うと思います

## チャレンジしたことと詰まったポイント

実際この作業は3 ~ 4日間かけて行っていたので、相当に詰まって、無駄なことをたくさんしました

自分が発生させた無駄な作業を少しでも回避してもらえればと思いますので、ぜひ参考にしてください🙏

### Herokuの中でDockerで実現させようとした

私はフルスタックを名乗っていますが、インフラは最低限のことしかできません。。。。

なので通常Herokuを使って実装していました

いつもHerokuを使っているため、Herokuであることは確定させ、Herokuで実装するためには、

- Dockerの中に複数のコンテナ作成して対応
- Herokuのapacheかnginxを直接修正して対応
- Railsのpublic dirにwordpress入れて、dbをmysqlのaddonで対応

等の案が思い浮かんでました。

結論から言うと、全部だめでした

Dockerに関してこちらのリポジトリで実装したのですが、<br>どうやらHeorkuのDockerではEXPOSE等のいくつかの設定が利用できなかったり、<br>docker-compose.ymlでpushできると思っていたら、Dockerfileごとだったりと、<br>インフラ知識が弱い故に相当な時間を注ぎ込み、実現が難しい、EC2の方が簡単、<br>と言うことがわかりました

その他二つに関しても調べれば調べるほどHerokuではなくEC2の方が簡単、という結論に至り、<br>Herokuは捨てて、EC2で実装する、と言う方針へ転換

[DockerでRails + WordPress + Nginx reverse proxyの参考リポ: masahiro04/docker-sample ](https://github.com/masahiro04/docker-sample)

### Rails入れてからwordpress入れたがphp-fpm入れてなかった

EC2にRailsを実装できて、reverse proxyでwordpressに飛ばすわけですが、<br>インフラわからなさすぎて、php-fpm入れていないくて、phpが動かず<br>結構ハマりました。。。。

インフラ初心者であれば、記事を内容を飛ばしたり、カスタムせずにとりあえず右に倣えが良いかと思います

### SSL入れていないのにhttpsにアクセスして5時間飛ばした

RailsをEC2にデプロイすることができたのですが、なぜか表示されない。。。。と言う現象が発生しました

configファイルでforce sslをtrueにしていたため、SSLは設定されていない、だけどhttpsに飛ばされていて、<br>画面が表示されない、となっていました

[こちらの記事を参考にしてください: AWS + Rails + Capistranoで何もエラーないのに表示されない問題](https://www.masahiro.me/posts/aws-rails-capistrano%e3%81%a7%e4%bd%95%e3%82%82%e3%82%a8%e3%83%a9%e3%83%bc%e3%81%aa%e3%81%84%e3%81%ae%e3%81%ab%e8%a1%a8%e7%a4%ba%e3%81%95%e3%82%8c%e3%81%aa%e3%81%84%e5%95%8f%e9%a1%8c)

### 小さな問題を大きくしすぎた

上記のphpが動かない、という問題自体は簡単だと思うのですが、問題と影響範囲を分けなかったことは失敗でした

RailsとWordpressそれぞれを分けて作成して、その後どちらかに足りていない、RailsかWordpressを入れる、と言うこともできたので、まずは問題を切り分けて解決することが大切だと、改めて認識です🙇‍♂️

### EC2のインスタンスを複数作ることをしなかった

面倒なので全部一つのインスタンスで実装していましたが、間違いでした

金額も安いので、RailsとWordpress、さらに統合用のインスタンスを作成、とかでも全然問題ないと思いますので、今悩んでいるぐらいならサクッと作ってしまいましょう<br>※金額が増えたとしても責任は取れませんので、あくまでもご自身の判断で

## 最後に

私自身インフラ知識が希薄で詰まったおかげでかなり理解が進みました

ネット上の情報が少なく、結構苦労したので、もし悩んでいる方の参考となれば幸いです🙏

上記情報で足りなくて、どうしてもということでしたらtwitterのDMもらえればZoomで少し通話もできるので、<br>ご連絡ください🙌
