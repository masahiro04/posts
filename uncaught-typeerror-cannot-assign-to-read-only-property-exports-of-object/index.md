```metadata
{
    "title": "Uncaught TypeError: Cannot assign to read only property &#8216;exports&#8217; of object &#8216;#<Object>&#8216;",
    "date": "2022-04-14T21:16:35",
    "categories": "React, Ruby on Rails",
}
```

Rails + Reactでタイトルのエラーが発生しました

どうやら、commonJSをES modulesで利用しようとしていることが問題らしく、<br>babel.config.jsに下記のコードを入力したら解決できました

```vim
 return {
    "sourceType": "unambiguous", // この部分！
    presets: []
}
```

## 参考記事

[webpack: import + module.exports in the same module caused error](https://stackoverflow.com/questions/42449999/webpack-import-module-exports-in-the-same-module-caused-error)
