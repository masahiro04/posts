```metadata
{
    "title": "vimのnextが遅い場合の対処法",
    "date": "2022-03-23T12:07:14",
    "categories": "Vim",
}```

vimでnextが非常に遅く(nをクリックしてから1秒ぐらい経過)とてもじゃないけど、作業できない状態で作業してました

そんな時にvim仲間の作業を見ていたところ、非常に早い動作をしていたのでgoogleで検索してみたところ、timeを設定できるらしく、こちらを入力したところ問題を解消することができました

```vim
 :set timeoutlen=100 ttimeoutlen=100
```

## 注意点

timeoutを設定すると

- fzf
- tree

等のコマンド入力に対する時間(コマンドを打つ間隔)も短くなるので、素早く打つ必要が出てしまいます

また上記コマンドだと毎回入力する必要があるので、下記の設定を.vimrcに追加して、入力間隔を200に設定することでいい感じにしました🙌

```vim
 set timeoutlen=200 ttimeoutlen=0
```

## 参考記事

[my vim jump to next word is quite slow, what am I doing wrong?](https://stackoverflow.com/questions/9722234/my-vim-jump-to-next-word-is-quite-slow-what-am-i-doing-wrong)
