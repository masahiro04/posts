```metadata
{
    "title": "sudo yum -y install mysql56-server mysql56-devel mysql56 No package mysql56-server available",
    "date": "2022-01-12T15:30:41",
    "categories": "AWS, MySQL",
}
```

AWSのEC2でMySQLを構築しようとしたところ、タイトルのエラーが発生しました

どうやら、

こちらのコマンドに変更して実行したところ、問題なくinstallできました🙌

```json
 sudo yum -y install mariadb-server mariadb-devel mariadb
sudo service mariadb start
sudo service mariadb status

# ここから先は起動方法と初期password生成方法
sudo systemctl start mariadb
sudo systemctl status mariadb
sudo /usr/bin/mysql_secure_installation
mysql -u root -p
```

## 参考記事

[Yum: No package mysql-server available in Cent OS 7](https://serverfault.com/questions/662741/yum-no-package-mysql-server-available-in-cent-os-7)
