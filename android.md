# Android

# AsyncTask
https://qiita.com/maromaro3721/items/c9e16068d13e8ca217f5

# Intent

## アクティビティ、サービス、ブロードキャスト

|受信|送信|明示的／暗示的|備考|
|:---|:--- |:--- |:---|
|Activity|`Context#startActivity()`|明示的・暗示的| |
| |`Activity#startActivityForResult()`|明示的・暗示的いける？|Intent先から戻るとき値を返せる|
|Service|`Context#startService()`|明示的・暗示的は非推奨|サービスを起動|
| |`Context#bindService()`|明示的・暗示的はエラー|バインドされたサービスを開始|
|BroadcastReceiver|システム|明示的・暗示的は非推奨のがある|Wi-Fiや電池の状態など|
| |`Context#sendBroadcast()`|明示的・暗示的はいけるか？| |
| |`Context#sendOrderedBroadcast()`|明示的・暗示的はいけるか？|これは何か調べる|
| |`Context#sendStickyBroadcast()`| |非推奨（API21,Android5.0から）|

BroadcastReceiverは`Context#registerReceiver()`で登録できる。

ContextはActivity,Serviceなど

## 明示的・暗示的

### 明示的Intent

### 暗示的Intent