```metadata
{
    "title": "nimのSPAフレームワークkaraxでhot reloadする方法",
    "date": "2022-04-06T16:49:53",
    "categories": "karax, nim",
}```

Goのairみたいにhot reloadする方法を探していたら、readmeに普通に書いてありました🙌

こちらが実行コマンド👇

```vim
 karun -w index.nim
```

色々な記事を見ていたら、

```c
 nim c --hotcodereloading:on -d:glfwDLL testgl.nim
```

などのhotreloadコマンドが多くて参考になりませんでしたが、karunのnimファイルがあったので、確認してみたところガッツリコマンドのオプションに関する記述があったのですぐにいけました

[こちら該当のコード(URL: karun.nim)](https://github.com/karaxnim/karax/blob/master/karax/tools/karun.nim#L32)

```vim
 case op.kind
    of cmdLongOption:
      case op.key
      of "run":
        run = true
        rest = rest.replace("--run ")
      of "css":
        if op.val != "":
          selectedCss = readFile(op.val)
        else:
          selectedCss = css
        rest = rest.substr(rest.find(" "))
      of "ssr":
        ssr = true
        rest = rest.replace("--ssr ")
      else: discard
    of cmdShortOption:
      if op.key == "r":
        run = true
        rest = rest.replace("-r ")
      if op.key == "w": #[ ここ！！！！！ ]#
        watch = true
        rest = rest.replace("-w ")
      if op.key == "s":
        ssr = true
        rest = rest.replace("-s ")
    of cmdArgument: file = op.key
    of cmdEnd: break
```

複数記事で`-r`オプション入れる、などと記載あったので、もしかしたら他にもコマンドあるかも？と思って調べたところ一発で見つかってよかったです

一応他のオプションも見たのですが、SSRも行けるそうなので、まだ調査はできていないのですが、SPAフレームワークとしてガッツリ使用することも悪くない選択肢と思えてきます

数日前からnimを触り始めたので魅力を引き出せきれてないですが、思いのほか書き方が好きなので継続して調査と実装進めていきたいです！

## 参考記事

[https://github.com/karaxnim/karax/blob/master/karax/tools/karun.nim#L32](https://github.com/karaxnim/karax/blob/master/karax/tools/karun.nim#L32)
