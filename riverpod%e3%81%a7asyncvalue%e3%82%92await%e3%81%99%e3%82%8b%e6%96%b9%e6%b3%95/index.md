```metadata
{
    "title": "RiverpodでAsyncValueをawaitする方法",
    "date": "2022-05-24T11:00:29",
    "categories": "Flutter, Riverpod",
}
```

Riverpodを用いて特定のfunction発火の際にawaitする方法でかなり詰まってしまったので、記事に残してます

とりあえずこちらが結論

```
 Future<void> onClicked() async {
  final hoge = await ref.read(hogeProvider.future);
}
```

whenやwhenData等も検討しましたが、awaitしていないのでタイミング合わず調べていたところこちらの記事を発見しました

[AsyncValue data function callback never executed when used from onPressed #628](https://github.com/rrousselGit/riverpod/issues/628)

<br>

[This is not how AsyncValue works.when does not “listen” to data/loading/error. It’s a switch-case on the current state.AsyncValue data function callback never executed when used from onPressed #628](https://github.com/rrousselGit/riverpod/issues/628)


