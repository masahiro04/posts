```metadata
{
    "title": "AWS EC2 Linux でgem install mysql2 -v &#8216;0.5.3&#8217;するとエラーが出る",
    "date": "2022-01-16T13:21:07",
    "categories": "AWS, EC2, MySQL, Ruby on Rails",
}```

AWS EC2でrails環境を構築中にタイトルコマンド実行でエラーが発生しました

```ruby
 gem install mysql2 -v '0.5.3'

Building native extensions. This could take a while...
ERROR:  Error installing mysql2:
        ERROR: Failed to build gem native extension.

    current directory: /home/ec2-user/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/mysql2-0.5.3/ext/mysql2
/home/ec2-user/.rbenv/versions/2.7.2/bin/ruby -I /home/ec2-user/.rbenv/versions/2.7.2/lib/ruby/2.7.0 -r ./siteconf20220116-18846-rjobkk.rb extconf.rb
checking for rb_absint_size()... yes
checking for rb_absint_singlebit_p()... yes
checking for rb_wait_for_single_fd()... yes
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.
```



どうやらyumこちらを追加しないといけないようなので追加して再度実行

```ruby
 sudo yum install mysql-devel
gem install mysql2 -v '0.5.3'

Building native extensions. This could take a while...
Successfully installed mysql2-0.5.3
Parsing documentation for mysql2-0.5.3
Installing ri documentation for mysql2-0.5.3
Done installing documentation for mysql2 after 0 seconds
1 gem installed

```

上記で問題なくinstallできました！！

## 参考記事

[【AmazonLinux2】MySQL2のGemをインストールする際にエラー](https://qiita.com/akk_ayy/items/f43a1af5368d1570c6f3)
