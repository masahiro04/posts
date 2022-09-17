```metadata
{
    "title": "coc + neovimでkotlinを使う方法",
    "date": "2022-03-05T01:13:53",
    "categories": "Android, kotlin",
}```

Android開発をAndroid studioではなくvimで行いたくて、色々と詰まった箇所あるので記事にしてみました

## 手順

<strong>Kotlinのinstall</strong>

まずはbrew経由でinstall

```vim
 brew install kotlin

kotlin --version
Kotlin version 1.6.10-release-923 (JRE 17.0.2+0)
```

<strong>Syntax系</strong>

```vim
 [[plugins]]
repo = 'udalov/kotlin-vim'
on_ft = ['kotlin']

# .vimrcにはこちらも追加。kotlinファイルをうまく認識してくれなかったので入れました
autocmd BufReadPost *.kt setlocal filetype=kotlin
```

<strong>lspの設定</strong>

まずはkotlin lspをclone

[fwcd/kotlin-language-server](https://github.com/fwcd)

```vim
 https://github.com/fwcd/kotlin-language-server.git

cd ../kotlin-language-server
./gradlew build
```

cloneが完了したら、coc-settingにこちらを追加

```vim
 "kotlin": {
    // LSPをdownloadしたpathを入れる
    "command": "/Users/masahirookubo/development/kotlin-language-server/server/build/install/server/bin/kotlin-language-server",
    "filetypes": ["kotlin"]
}
```

![](./Screen-Shot-2022-03-05-at-1.04.47-644x480.png)

若干動作が遅い気もしますが、上記の対応でneovimでAndroid開発をサクサク(一応)行えるようになりました！

## 参考記事

[Building](https://github.com/fwcd/kotlin-language-server/blob/main/BUILDING.md)

[Editor Integration](https://github.com/fwcd/kotlin-language-server/blob/main/EDITORS.md)
