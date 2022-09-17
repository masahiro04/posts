```metadata
{
    "title": "Rails + WebpackerでERROR in chunk application [entry] js/[name]-[contenthash].jsが発生",
    "date": "2022-06-09T01:10:41",
    "categories": "Ruby on Rails",
}
```

Railsの各種packageのバージョンアップを行なっていたところタイトルのエラーが発生しました

とりあえず結論

```
 config/webpack/development.js を下記のように修正することでエラーを回避できます
```

```typescript
 process.env.NODE_ENV = process.env.NODE_ENV || 'development'

const environment = require('./environment')

const config = environment.toWebpackConfig();
config.output.filename = 'js/[name]-[hash].js'

module.exports = config;
```

## 参考記事

[ERROR in chunk application [entry] js/[name]-[contenthash].js Cannot use [chunkhash] or [contenthash] for chunk in .](https://gorails.com/forum/error-in-chunk-application-entry-js-name-contenthash-js-cannot-use-chunkhash-or-contenthash-for-chunk-in)
