# iOS

swiftの文法とかは `swift.md` に書いています

# DB, CoreData

## メモ
Entity = Table = モデル = NSManagedObject

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