```metadata
{
    "title": "jestで全てpassなのにexit status1が出る",
    "date": "2022-03-30T11:52:00",
    "categories": "jest, React, React Testing Library",
}
```

jestで全てのtestをpassしているにもかかわらず、なぜかexit status1が出る、という問題が発生しました

```vim
 Test Suites: 55 passed, 55 total
Tests:       402 passed, 402 total
Snapshots:   3 obsolete, 20 passed, 20 total
Time:        71.287 s
Ran all test suites.
error Command failed with exit code 1.
```

どうやらobsoleteが悪さをしているらしく、こちらのコマンドでobsoleteを解消することでpassさせることができます

※ -u を追加

```vim
 yarn test --coverage -w 1 -u
```

-u コマンドは失敗したsnapshotテストを再生成するための指定で、残っているsnapshotなどに悪ささせないことが可能となっています

ちなみにですが、CI上では-uを付けると問題になることがあるみたいなので、あまりよろしくないそうです

[https://github.com/facebook/jest/issues/9324#issuecomment-623713377](https://github.com/facebook/jest/issues/9324#issuecomment-623713377)

## 参考記事

[【Jest】テストは全てsuccessなのにexit statusが 1](https://uga-box.hatenablog.com/entry/2020/07/23/000000)

[JEST tests complete successfully but returns exit status 1 #9324](https://github.com/facebook/jest/issues/9324)
