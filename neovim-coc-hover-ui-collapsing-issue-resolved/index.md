```metadata
{
    "title": "NeoVim + COCのhover UIが崩れる問題の解消",
    "date": "2022-07-20T03:45:23",
    "categories": "Vim",
}```

iOSのビルドやUI以外は原則vimで開発を行なっているのですが、COCのホバーで情報を確認する際にUIが崩れていて長い間じわじわと精神的苦痛が伴っていたのですが解決できたので情報を共有します

結論下記をコメントアウトしました

```vim
 " NG
set ambiwidth=double

" OK
" set ambiwidth=double
```

before

![](./Screen-Shot-2022-07-20-at-3.36.50.png)

after, 綺麗!!!感激です😭😭

![](./Screen-Shot-2022-07-20-at-3.37.38.png)

今までは定義元へジャンプしたりして対処していたのですが、本来は上記画像のように丁寧な見出し分的な情報があるみたいです😳

そもそも何故ambiwidthをセットしていたかというと、「vimで全角文字を表示するため」という理由でした

正直利用ケースは皆無なのですが、何故か入っていた、、、、その結果非常に大きな苦痛を強いられていたので、これからは定期的にvimrcを育てていこう、と思える痛い経験でした🙇‍♂️

## 参考記事

[[Vim] 文字崩れ, 文字が残ってしまうのを防ぐ](https://reona.dev/posts/20200823)