```metadata
{
    "title": "AWS EC2 LinuxでNeovimを導入する方法",
    "date": "2022-01-14T17:14:04",
    "categories": "AWS, EC2, Vim",
}```

普段Neovimで開発しており、最低限のvim操作を行うために導入しました

まずはyumで関連のライブラリとかをinstall

```bash
 sudo yum groups install -y Development\ tools
sudo yum install -y cmake
sudo yum install -y python34-{devel,pip}
sudo pip-3.4 install neovim --upgrade
(
cd "$(mktemp -d)"
git clone https://github.com/neovim/neovim.git
cd neovim
make CMAKE_BUILD_TYPE=Release
sudo make install
)
```

上記完了したら、ターミナルでこちらを打ち込んでください

```bash
 /usr/local/bin/nvim
```

![](./Screen-Shot-2022-01-14-at-17.10.35-644x565.png)

nvimをinstallしただけでは、毎回上記のようにpath指定しないと起動しないので<br>エイリアスを下記のように追加しておきます

```bash
 vim .bash_profile

alias vi='/bin/nvim'
alias vim='/bin/nvim'
```

これでvimコマンドでneovimが発火するようになりました！

## 参考記事

[kawaz/install_neovim_to_amazonlinux.sh](https://gist.github.com/kawaz/393c7f62fe6e857cc3d9)

[CentOS8 に neovim をインストールする](https://sig9.hatenablog.com/entry/2020/04/20/000000)
