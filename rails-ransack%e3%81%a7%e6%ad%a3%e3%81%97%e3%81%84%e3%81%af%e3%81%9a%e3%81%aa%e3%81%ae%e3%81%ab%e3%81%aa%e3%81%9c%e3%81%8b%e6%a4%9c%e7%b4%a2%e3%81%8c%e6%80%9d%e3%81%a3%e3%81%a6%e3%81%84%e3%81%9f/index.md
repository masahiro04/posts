```metadata
{
    "title": "Rails + ransackで正しいはずなのになぜか検索が思っていた通りに動かない時の参考点",
    "date": "2022-02-01T00:24:12",
    "categories": "Ruby on Rails",
}```

結論、enumでintではなくstringで検索していたから、です

通常のstringなら動いているのですが、enurmrizeを使ったenumのカラムのみなぜか検索が効かずに、どんな手法を通じませんでした。。。

色々と検索している際に、active adminの検索もintじゃないと動かないことを思い出し、実装してみたところ動きました🙌

こちらコード

```ruby
 # 動かないコード
= link_to jobs_path(q: { kind_eq: 'hogehoge' })

# 動くコード
= link_to jobs_path(q: { kind_eq: 1 })
```

## 参考記事

なし、思いつきました
