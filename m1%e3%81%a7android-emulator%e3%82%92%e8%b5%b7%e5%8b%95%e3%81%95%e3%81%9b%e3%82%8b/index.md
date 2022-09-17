```metadata
{
    "title": "M1でAndroid emulatorを起動させる",
    "date": "2022-05-18T21:04:10",
    "categories": "Android, Android Studio",
}```

M1でAndroid emulator起動させようとしたら下記のエラーが発生しました

```vim
 % ~/Library/Android/sdk/tools/emulator -list-avds
Pixel_3_API_30
Pixel_4_API_Tiramisu

% ~/Library/Android/sdk/tools/emulator -avd Pixel_4_API_Tiramisu
Could not launch '/Users/masahirookubo/Library/Android/sdk/emulator/qemu/darwin-x86_64/qemu-system-aarch64': No such file or directory
```

厳密に言うとエラーではなく、該当のファイル or ディレクトリがありません、の問題です

調べて見たところ、こちらの現象は参照しているディレクトリが異なるようで、シンボリックリンクを貼ることで問題解決ができるようです

こちらが該当のシンボリックリンクで、問題のディレクトリまでいきそこで実行することで起動が可能な状態になります

```vim
 % cd Library/Android/sdk/emulator/qemu
% ln -s darwin-aarch64 darwin-x86_64
```

上記対応完了後に冒頭のコマンドを実行すると

```vim
 %  ~/Library/Android/sdk/tools/emulator -avd Pixel_4_API_Tiramisu
```

emulatorが起動します！

![](./Screen-Shot-2022-05-18-at-20.58.02-644x569.png)

毎回上記コマンドを打ち込むのも面倒なので、私はaliasでこちらを設定することでandroidコマンドだけで起動できるようにしておきました🙌

```vim
 # .zshrc
alias android="~/Library/Android/sdk/tools/emulator -avd Pixel_4_API_Tiramisu"
```

## 参考記事

[Run AVD Emulator without Android Studio](https://stackoverflow.com/questions/42718973/run-avd-emulator-without-android-studio)

[How can I launch Android Emulator without android studio on Mac M1](https://stackoverflow.com/questions/71015608/how-can-i-launch-android-emulator-without-android-studio-on-mac-m1)
