```metadata
{
    "title": "imported module &#8216;test_room&#8217; has this __file__ attributes which is not the same as the test file we want to collect:",
    "date": "2022-01-01T14:36:47",
    "categories": "FastAPI, Flask, Python",
}
```

Flaskのプロジェクトでテストを書いていたら、なぜかタイトルのエラーが発生

```
 問題点は各ディレクトリに __init__.py ファイルがないことで、各ディレクトリに追加することで解消できました！
```

こちら参考画像

![](./Screen-Shot-2022-01-01-at-14.35.35.png)

## 参考記事

なし、思い出した
