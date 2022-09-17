```metadata
{
    "title": "nvim + coc + nim + macOSの環境開発",
    "date": "2022-04-06T01:49:14",
    "categories": "nim",
}```

仕事でGoやほんの少しRustを触る機会があって、近しいnimがだいぶ気になっていたので、環境設定してみました

まずはnimをinstallしていきます

```c
 
$ curl https://nim-lang.org/choosenim/init.sh -sSf | sh

Switched to Nim 1.6.4
choosenim-init: ChooseNim installed in /Users/masahirookubo/.nimble/bin
choosenim-init: You must now ensure that the Nimble bin dir is in your PATH.
choosenim-init: Place the following line in the ~/.profile or ~/.bashrc file.
choosenim-init:     export PATH=/Users/masahirookubo/.nimble/bin:$PATH
```

※brew install nimでもinstallできましたが、nimlspをinstallするタイミングで下記エラー出たので、上記でinstallを行っています

```c
 Error: cannot open file: /opt/homebrew/Cellar/nim/1.6.4/nim/nimsuggest/nimsuggest.nim
```

installが完了した環境変数をzshやbashに設置

```c
 # .zshrc
export PATH=/Users/masahirookubo/.nimble/bin:$PATH
```

設置完了したら反映して適用されているかを確認

```c
 $source ~/.zshrc
$ which nim
/Users/masahirookubo/.nimble/bin/nim
```

nimが問題なく設置できているので、次はlspを追加します

```c
 $ nimble install nimlsp
Downloading https://github.com/PMunch/nimlsp using git
  Verifying dependencies for nimlsp@0.4.0
      Info: Dependency on jsonschema@>= 0.2.1 already satisfied
  Verifying dependencies for jsonschema@0.2.1
      Info: Dependency on ast_pattern_matching@any version already satisfied
  Verifying dependencies for ast_pattern_matching@1.0.0
 Installing nimlsp@0.4.0
   Building nimlsp/nimlsp using c backend
/private/var/folders/bb/n3dq0s2963v_4c8dw8ygjlgm0000gn/T/nimble_45528/githubcom_PMunchnimlsp/src/nimlsppkg/baseprotocol.nim(26, 29) Warning: Deprecated since 1.5; TaintedString is deprecated [Deprecated]
   Success: nimlsp installed successfully.

```

lspが追加できたらcocの設定を行います

```vim
 "nim": {
  "command": "nimlsp",
  "filetypes": ["nim"],
  "trace.server": "verbose"
}
```

lspの設定を行ってもnimファイルを認識しないことがあるので、私はこちらをvimrcに追加して強制的に認識させました

```vim
 autocmd BufNewFile,BufRead *.nim, set filetype=nim
```

lsp入れ終わったので、一旦確認

型定義を表示できたので、一応これで開発はできそうです！

![](./Screen-Shot-2022-04-06-at-1.37.53-644x185.png)

ガッツリ開発もしてみたいので、シンタックスも入れてみます

```c
 [[plugins]]
repo = 'baabelfish/nvim-nim'
on_ft = ['nim']
```

入れ終わるとこんな感じでシンタックス適用できて、かなりみやすくなりました！

![](./Screen-Shot-2022-04-06-at-1.45.11-644x817.png)

## 参考記事

[VimでのNim開発環境構築](https://scrapbox.io/jiro4989/Vim%E3%81%A7%E3%81%AENim%E9%96%8B%E7%99%BA%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89)

[NeovimでNimを書く環境を整える](https://qiita.com/jiro4989/items/6cf94cec054e50f66dbf)
