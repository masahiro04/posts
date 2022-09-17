```metadata
{
    "title": "Xcodeで実機デバッグ試すとThis operation can fail if the version of the OS on the device is incompatible&#8230;と出る",
    "date": "2022-07-10T18:35:19",
    "categories": "iOS, Swift, 未分類",
}```

Xcodeで実機デバッグを試みたところ、下記のエラーが発生しました

```swift
 Failed to prepare device for development.

This operation can fail if the version of the OS on the device is incompatible with the installed version of Xcode. You may also need to restart your mac and device in order to correctly detect compatibility.
```

こちらの問題ですが、どうやらXcodeでバージョンの対応があるかないか？で判断してるようで、対応していないとエラーが出るみたいです

ちなみに対応しているかどうかは下記のコマンドで件検証可能

```swift
 cd /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport

ls
10.0	10.2	11.0	11.2	11.4	12.1	12.3	13.0	13.2	13.4	13.6	14.0	14.2	14.4	15.0	9.0	9.2
10.1	10.3	11.1	11.3	12.0	12.2	12.4	13.1	13.3	13.5	13.7	14.1	14.3	14.5	15.2	9.1	9.3

```

私が検証したいのは15.5で一覧にないので、下記のプロジェクトからpullして上記dirに移動させます

[プロジェクトのURL](https://github.com/filsv/iOSDeviceSupport)

![](./Screen-Shot-2022-07-10-at-18.02.16-644x539.png)

ダウンロードが完了したらターミナルで下記を実行していきます

```swift
 # Downloads dirに移動
cd Downloads

# zipをunzip
unzip 15.5.zip
Archive:  15.5.zip
   creating: 15.5/
  inflating: 15.5/DeveloperDiskImage.dmg.signature
  inflating: 15.5/DeveloperDiskImage.dmg

# 解凍したdirをiOSバージョンのdirへ移動. 権限エラーが発生するのでsudoをつけてます
sudo mv 15.5 /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport

# 該当のdirへ移動して確認
cd /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport
ls
10.0	10.2	11.0	11.2	11.4	12.1	12.3	13.0	13.2	13.4	13.6	14.0	14.2	14.4	15.0	15.5	9.1	9.3
10.1	10.3	11.1	11.3	12.0	12.2	12.4	13.1	13.3	13.5	13.7	14.1	14.3	14.5	15.2	9.0	9.2
```

15.5を入れた後は問題なく起動しました！

## 参考記事

[filsv/iOSDeviceSupport](https://github.com/filsv/iOSDeviceSupport)

[Xcode: iOS 15.4 実機ビルドができない!?](https://www.fuwamaki.com/article/345)
