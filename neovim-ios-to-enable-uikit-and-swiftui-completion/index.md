```metadata
{
    "title": "[WIP]NeoVim + iOSでUIKitやSwiftUIの補完を可能にする",
    "date": "2022-07-10T00:25:16",
    "categories": "iOS, Swift",
}
```

sourcekit lspを追加するところまでは全く問題なかったのですが、

```swift
 import UIKit
import SwiftUI
```

などのiOSのUI frameworkが動かない問題が発生し、そのエラーを解消するための方法の記事になります

従って、最初からstep by stepで説明するわけではないので、初期設定を済ませていない方は下記の記事など参考に準備してください🙏

[導入がまだの場合はこちらなどを参考に：Vim(NeoVim)でSwift -プラグイン, LSPの導入まで](https://qiita.com/AK-10/items/975b2b2d036ef9126e9b)

## 具体的な方法

とりあえず結論

※注意!!!!M1だとx86_64アーキテクチャで動かない場合があり、arm64に変更が必要かもです

```swift
 swift build -c release -Xswiftc -sdk -Xswiftc /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk -Xswiftc -target -Xswiftc x86_64-apple-ios15.2-simulator
```





## 参考記事

[UIKitに依存するSwift PackageをVSCodeで開発する](https://qiita.com/niusounds/items/5a39b65b54939814a9f9)

[iOS Development on VSCode](https://medium.com/swlh/ios-development-on-vscode-27be37293fe1)

[How to use SourceKit-lsp with iOS projects](https://forums.swift.org/t/how-to-use-sourcekit-lsp-with-ios-projects/28273)

[no such module ‘UIKit’](https://haifengkao.medium.com/no-such-module-uikit-b51d2ce76e6)




