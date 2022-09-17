```metadata
{
    "title": "Rails + Haml + Tailwindで Illegal nesting: nesting within a self-closing tag is illegal エラー",
    "date": "2021-12-07T17:06:27",
    "categories": "Ruby on Rails, Tailwind",
}
```

Rails + Haml + Tailwindで実装していたら、タイトルのエラーが発生しました

どうやらnestがおかしくなっていると認識しちゃっているらしかったので、<br>class要素として分けることで対処しました🙏

```ruby
 .bg-gray-300.w-25.mx-4.my-2.p-2.rounded-lg.clearfix{ class: 'w-1/3' }
```

## 参考記事

なし
