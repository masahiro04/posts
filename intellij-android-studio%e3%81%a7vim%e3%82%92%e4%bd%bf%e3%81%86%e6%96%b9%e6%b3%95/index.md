```metadata
{
    "title": "IntelliJ/Android Studioでvimを使う方法",
    "date": "2022-05-17T15:32:04",
    "categories": "Android Studio, IntelliJ, Vim",
}
```

普段NeoVimで開発を行なっているのですが、スマホアプリを開発するにあたってemulatorや解析等々のツールを使った方が良いと思い、Android Studioでもvimが使えるようにしてみました

## プラグイン追加

まずはvimを使えるようにするためにプラグインを追加します

IntelliJ/Android Studio -&gt; preferences -&gt; pluginsでIdeaVimを追加

![](./Screen-Shot-2022-05-17-at-15.00.12-644x477.png)

追加した後は再起動必須なので、再起動します

再起動するとvimでの操作が可能になっています

![](./Screen-Shot-2022-05-17-at-15.03.20-644x587.png)

## vim機能の有効化

```
 調べてみたところ、IntelliJやAndroid Studioでvimのプラグインを入れても  .ideavimrc というファイルが参照されるだけで、自動的に .vimrc が参照されるわけではないみたいです
```

```
 なので、既存のvim設定を反映させるために .ideavimrc の中身を下記のように変更します
```

まずはファイルに移動してファイル編集モードへ

```vim
 vi ~/.ideavimrc
```

開くことができたらこちらを貼り付けます

```vim
 source ~/.vimrc
```

```
 上記のコードを貼り付けることで、 .vimrc に定義しているvimの設定をimportすることが可能になり、intelliJやAndroid Studioでもいつも通りなvim操作を行うことができるようになります
```

## 余談

上記の設定で一応vim操作を行うことができるようになるのですが、あくまでも<strong>vimの操作だけ</strong>が可能になります

もしかしたら参照先をnvimに切り替えたらいけるかも？とも思いましたが結果変わらずで、

```vim
 # .ideavimrc
source ~/.config/nvim/init.vim
```

plugin等の設定は一切引き継がれないので、その辺りの注意は必要で、確認したところ下記のpluginだけ利用できるみたいです

- vim-easymotion
- NERDTree
- vim-surround
- vim-multiple-cursors
- argtextobj.vim
- vim-textobj-entire
- ReplaceWithRegister
- vim-exchange
- vim-highlightedyank
- vim-paragraph-motion
- vim-indent-object
- match.it

[利用可能なpluginsの参考url: https://github.com/JetBrains/ideavim/wiki/Emulated-plugins](https://github.com/JetBrains/ideavim/wiki/Emulated-plugins)

個人的には普段利用しているpluginを全て引き継ぎたかったので、最低限の確認はIntelliJ/Android Studioで行い、それ以外の一切の操作は引き続きvimを利用することにしました

とても便利、とうい領域には程遠いですが、マウス操作が減るので最終的には割と効率的な感じにはいけました！

## 参考記事

[Is there a way to get IdeaVIM to honor the mappings from my .vimrc file?](https://stackoverflow.com/questions/5585687/is-there-a-way-to-get-ideavim-to-honor-the-mappings-from-my-vimrc-file)
