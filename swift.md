# swift

iOSのフレームワークについては `iOS.md` に書いています

# 公式ドキュメント
Xcode のメニュの `Help -> Developer Document`

# クラス名取得

AAA クラスのとき

```
String(describing: AAA.self)
```

# 拡張


Swiftでは、すでに存在するクラス、構造体、列挙型、およびプロトコルに新しい機能を追加することができる。

下記のように拡張すると、String型で下記の拡張した機能を使うことができる

```
extension String {

    var nameCount:Int {
        get {
            .......
        }
    }

	func updateName(newName:String) {
        .......
    }
}
```

## 参考リンク
https://qiita.com/crea/items/4297bf60d222d661498f

# @escaping

- [Swift 3 の @escaping とは何か](https://qiita.com/mishimay/items/1232dbfe8208e77ed10e)