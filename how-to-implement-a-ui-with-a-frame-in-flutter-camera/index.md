```metadata
{
    "title": "Flutter cameraで枠ありのUIを実装する方法",
    "date": "2022-06-29T17:56:59",
    "categories": "Dart, Flutter",
}```

Flutterでカメラ画面で特定の枠を切り抜いているようなUIの実装で少し時間をとってしまったので、記事にしておいてみました

Stackで重ねる方法など色々チャレンジしたのですが、最終的にclipするという方法に落ち着いたので、おそらくほとんどのケースで通用するのでは？と思ってます🙌

※cameraの導入やcontroller変数定義などは公式ドキュメントに従ってご自身で実装してからこちらの記事の内容を検証してください

```dart
 // widget部分
SizedBox(
  width: double.infinity,
  height: MediaQuery.of(context).size.height * 0.4,
  child: Stack(
    alignment: Alignment.center,
    children: [
      CustomPaint(
        foregroundPainter: Paint(),
        child: SizedBox(
          width: double.infinity,
          child: CameraPreview(controller.value!),
        ),
      ),
      ClipPath(
        clipper: Clip(),
        child: SizedBox(
          width: double.infinity,
          child: CameraPreview(controller.value!),
        ),
      ),
    ],
  ),
)

// CustomerPaint/ClipPathの引数に当てるclass
class Paint extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    canvas.drawColor(Colors.grey.withOpacity(0.7), BlendMode.dstOut);
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) {
    return true;
  }
}

class Clip extends CustomClipper<Path> {
  @override
  Path getClip(Size size) {
    const horizontalPadding = 40.0;
    const verticalPadding = 180.0;
    final path = Path()
      ..addRRect(
        RRect.fromRectAndRadius(
          Rect.fromLTWH(
            horizontalPadding / 2,
            size.height - (size.height - (verticalPadding / 2)),
            size.width - horizontalPadding,
            size.height - verticalPadding,
          ),
          const Radius.circular(10),
        ),
      );
    return path;
  }

  @override
  bool shouldReclip(CustomClipper<Path> oldClipper) {
    return true;
  }
}
```

![](./Screenshot-2022_06_29-17_40_29-644x1395.png)

## 参考記事

[Add Rectangle overlay in a camera application and Crop the portrait image in flutter](https://stackoverflow.com/questions/64451536/add-rectangle-overlay-in-a-camera-application-and-crop-the-portrait-image-in-flu)
