```metadata
{
    "title": "AWS EC2で古いnodeがinstallされる問題の解消",
    "date": "2022-01-13T02:05:43",
    "categories": "AWS",
}```

AWSのEC2でver 14のnode入れているはずなのに、6系が毎度installされる問題が発生しました

下記の通り実行することで解消できました！

```json
 sudo yum remove -y nodesource-release* nodejs
yum clean all
sudo rm -rf /var/cache/yum/*
sudo rm /etc/yum.repos.d/nodesource-el.repo

curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
sudo yum install -y nodejs
```

## 参考記事

[Amazon Linuxで古いNode.jsがインストールされる時の解決方法](https://inaba.hatenablog.com/entry/2018/11/13/023933)
