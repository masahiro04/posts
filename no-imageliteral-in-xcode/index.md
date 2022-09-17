```metadata
{
    "title": "XcodeでimageLiteralが出てこない",
    "date": "2022-07-10T02:15:46",
    "categories": "iOS, Swift, 未分類",
}```

Xcode 13.2.1でimageLiteralがサジェストで出てこなく、調べてみたら記述方法が変わったとのことです

```swift
 class ViewController: UIViewController {
    @IBOutlet weak var hoge: UIImageView!
    override func viewDidLoad() {
        super.viewDidLoad()
        hoge.image = #imageLiteral()
    }
}
```

#imageLiteralと記述し、その後に括弧を入れることで画像選択UIが発火します

![](./Screen-Shot-2022-07-10-at-2.14.25.png)

## 参考記事

[Color Literal behaves inconsistently in Xcode 13.2.1](https://developer.apple.com/forums/thread/697107)
