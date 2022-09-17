```metadata
{
    "title": "Vimプラグイン自作の方法",
    "date": "2022-08-28T15:29:54",
    "categories": "Vim",
}
```

基本的にはGithubを漁って自分に合ったプラグインを探して利用する、という方針だったのですが、どうしても実現できない機能がいくつかあり、新しいチャレンジを行うという試みでプラグインの自作に踏み出してみました

難しいことはやらずに簡易的な機能を提供するだけのプラグインをとりあえず作ってみます🦀

## 事前準備

現在NeoVimとそれなりに多いプラグインで構成されているので、最小構成のVim環境を新しいブランチを作成しましょう

特にやる必要はないかもですが、今後作成していく自作プラグインは一応Githubで公開予定なので可能な限り依存を少なくする、という意味合いで準備します

※deinやmotion移動等だけ残してそれ以外は消しました

## 超最小構成*.vimの実行

```
 最小構成のvimに変更したら、任意のdirに sample.vim というファイルを作成します
```

```vim
 [masahirookubo@MacBook-Pro-2 (arm64):~/dotfiles][feature/plugins]
% touch sample.vim
```

そしてprint文を記載して実行

```vim
 echo "hogehgoe!!!"

:source sample.vim
```

![](./Screen-Shot-2022-08-28-at-13.06.43.png)

動きました！！

最小構成ではこちらでいけますので、一旦これで完了とします

## 超最小構成のvimプラグインの実装

最小構成のプログラムが稼働することを検証できたので、次はもう少しプログラムを追加してそれをremoteで公開してみます

まずはpluginを作っていくためのdirなどの準備コマンドの実行

```vim
 # 任意のdirへ移動してから実行
mkdir sample_plugin
mkdir plugin
touch plugin/sample_plugin.vim
vi plugin/sample_plugin.vim
```

作成したら、下記のプログラムを追加

```vim
 function! sample_plugin#SamplePlugin()
  echo 'It works!'
endfunction

command! SamplePlugin call sample_plugin#SamplePlugin()
```

プログラムの追加が完了したら、ファイルを読み込ませて実行してみます

```vim
 :source sample_plugin.vim
:SamplePlugin
```

問題なく読み込めています👍

![](./Screen-Shot-2022-08-28-at-13.22.12.png)

## GIthubから利用してみる

localでの稼働も検証できたので、Githubに上げてdeinで追加して検証を行います

※パッケージマネージャはお使いのものを利用してください

```vim
 [[plugins]]
repo = 'masahiro04/sample_plugin'
```

追加した後はvimを起動して下記を実行・検証してみます

「sam」と打ち込むと候補が出力され

![](./Screen-Shot-2022-08-28-at-13.27.26.png)

エンターを押すと、実装したプラグインが表示されました！

![](./Screen-Shot-2022-08-28-at-13.27.31.png)

もうちょっと難しいと思ってたんですけど、思いのほか簡単に実装できたので、自分が作りたいプラグインをこれから作ってみようと思います！！！

## 参考記事

[はじめてのvimscriptとvimプラグインの作成](http://qs.nndo.jp/2017/08/12/643/)

[簡単なVimプラグインを作って基礎を学ぶ](https://qiita.com/sakupa80/items/9ed267c0fb4bb3003744)
