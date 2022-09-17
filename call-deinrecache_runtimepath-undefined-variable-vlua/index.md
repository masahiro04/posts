```metadata
{
    "title": "call dein#recache_runtimepath()でUndefined variable: v:luaエラー",
    "date": "2022-03-26T03:56:21",
    "categories": "Vim",
}
```

deinのcacheを削除してもう一度再生成を試みていたところタイトルのエラーが発生しました

cacheを再生成したにも関わらず更新されない場合は、こちらのコマンドを実行することでマニュアル削除できる、とのことでしたが、そもそもそれが実行できなく、どうしようもない状態でした。

※こちらがキャッシュ強制削除コマンド

```vim
 :call dein#recache_runtimepath()
```

## 結論

nvimをupdateしたら直りました

update前は0.4.4?あたりのversionで、最新版は0.7.0ということで、もしかしたら？と思って

まずはupdateして

![](./Screen-Shot-2022-03-26-at-3.47.25.png)

こちらを実行してみたら

```vim
 :call dein#recache_runtimepath()
```

エラーが出ずに、キャッシュクリアして元通りとなりました!!!

## nvimの再インストール手順

```vim
 # 既存のnvimをuninstall
sudo rm /usr/local/bin/nvim
sudo rm -r /usr/local/share/nvim/

# nvimをclone
git clone https://github.com/neovim/neovim.git

# install. sudo make installでエラーが出る場合は下記を参考に￥
cd neovim
make CMAKE_BUILD_TYPE=Release
sudo make install
```

こんな感じのエラー出る場合は

```vim
 CMake Error at runtime/cmake_install.cmake:77 (file):
  file cannot create directory:
  /usr/local/share/nvim/runtime/pack/dist/opt/matchit/doc.  Maybe need
  administrative privileges.
Call Stack (most recent call first):
  cmake_install.cmake:86 (include)
```

こちらを実行して、ディレクトリを作成、そして実行

```vim
 mkdir /usr/local/bin/nvim
mkdir /usr/local/share/nvim/

# 作成したら再度実行
sudo make install
```

完了したら、vimを一旦落としてまた立ち上げると動いています！！！！

![](./Screen-Shot-2022-03-26-at-3.44.02-644x629.png)

動いていることと、便利な機能を提供してくれている作者さん等々に感謝です！！！！！

## 参考記事

[neovimのアップデート](https://qiita.com/r12tkmt/items/5c22c68c70f39f8d1168)
