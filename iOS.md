# iOS

swiftの文法とかは `swift.md` に書いています

# TableView

## TableViewCell の中にUIオブジェクトを入れるとき

普通にstoryboardで 「TableViewCellの中にUIオブジェクトを入れる」 方法と 「xibを作成する」 方法の二種類がある


- TableViewCellの中にUIオブジェクトを入れる
:    1回だけ使う場合

- xibを作成する
:    他の場所でも使うとき


### TableViewCellの中にUIオブジェクトを入れる

https://qiita.com/Ajyarimochi/items/0154030bde239d703806

### xibを作成する

https://qiita.com/hiromasa-fun/items/d4d115a63939246ff3cc


# DB, CoreData

## メモ

- Entity = Table = NSManagedObject
- ◯◯.xcdatamodel = BD = NSPersistentContainer

## 参考リンク

- [Documentation > Core Data - 公式](https://developer.apple.com/documentation/coredata)
- [CoreDataをやさしく使う - Qiita](https://qiita.com/touyoubuntu/items/5133ba503da74bb39063)

## SQLiteの中身があるディレクトリ

`AppDelegate.swift` に下記の記述を追加

```
func application(
    _ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

	//追加、このpathにSQLiteのDBのファイルがある
    let path = NSSearchPathForDirectoriesInDomains(.applicationSupportDirectory, .userDomainMask, true)
    print("=====\(path)=====")

    return true
}
```

### 参考リンク

https://webhoric.com/programming/swift/swift-coredata-sqlite/