```metadata
{
    "title": "EC2のSSH接続でport 22: Operation timed outが出る問題",
    "date": "2022-01-21T09:15:00",
    "categories": "AWS, EC2",
}
```

直し方の結論、

Public IPの末尾1桁が抜けていました！！！

夜作業していて、途中で諦めて朝から作業していたのですが、IPがあっているか確認している途中に気がつきました。。。

皆さんも気をつけてください🙇‍♂️

## 参考記事

[【AWS EC2 エラー】ssh port 22 Operation timed out](https://qiita.com/yokoto/items/338bd80262d9eefb152e)

[AWS ssh access ‘port 22: Operation timed out’ issue](https://stackoverflow.com/questions/17698876/aws-ssh-access-port-22-operation-timed-out-issue)
