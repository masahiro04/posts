```metadata
{
    "title": "Cloud Formationでcreate stackするとTemplate format error: unsupported structureが出る",
    "date": "2022-04-30T00:46:44",
    "categories": "Cloud Formation",
}```

Cloud Formationをcreate stackしたところタイトルのエラーが発生しました

```yaml
 % aws cloudformation create-stack --capabilities CAPABILITY_IAM --stack-name ecs-core-infrastructure --template-body ./ecs-setup/1_Core-infrastructure-setup/core-infrastructure-setup.yml

You must specify a region. You can also configure your region by running "aws configure".

```

![](./Screen-Shot-2022-04-30-at-0.37.50-644x902.png)

エラー発生してから確認したところ、上記画像のようにエラーが発生していたのでlspで設定を追加しようかなと思っていたのですが、よくよく考えたら、vim上のエラーはあくまでも設定上エラーとして表現されている、と思い諸々調べていたところ、下記のような有益が情報を得られました

[The --template-body argument must be specified as a file URI.https://stackoverflow.com/questions/41992569/template-format-error-unsupported-structure-seen-in-aws-cloudformation](https://en.wikipedia.org/wiki/File_URI_scheme)

簡単に説明すると、–template-bodyはfile URIとして呼び出さないといけません、ということみたいです

なのでこちらで実行

```yaml
 % aws cloudformation create-stack --capabilities CAPABILITY_IAM --stack-name ecs-core-infrastructure --template-body file://./ecs-setup/1_Core-infrastructure-setup/core-infrastructure-setup.yml


{
    "StackId": "arn:aws:cloudformation:ap-northeast-1:hogehoge:stack/ecs-core-infrastructure/hogehogehogehogehogehogehogehoge"
}
```

動きました！

## 参考記事

引用部分に記載してあります
