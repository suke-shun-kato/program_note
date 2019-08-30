# facebook

# はじめにの前に

facebook上で開発者としてアプリを作成しないといけない

下記から作成できる

- [facebook for developers](https://developers.facebook.com/) -> マイアプリ（右上） -> アプリの制作

登録時には色々と登録しないといけないが、以下の項目が難しかったのでメモ

## プライバシーポリシーのURL

これがないと使えない機能がある

英語で自動生成してくれるサイトがある（下記）

- [App Privacy Policy Generator](https://app-privacy-policy-generator.firebaseapp.com/)


## キーハッシュ

これも登録

### デバッグ用

Windows だと下記コマンドで生成

```
keytool -exportcert -alias androiddebugkey -keystore %HOMEPATH%\.android\debug.keystore | openssl sha1 -binary | openssl base64
```

キーストアのパスワードは何でもよい

### 製品用

後ほど書く


# はじめに

下記の２種類のAPIが存在する

- グラフAPI

こっちを使う

- マーケティングAPI

# 基本

下記の3つの要素がある

- nodes
- edges
- fields 

GET https://graph.facebook.com/your-facebook-user-id/photos?fields=name&access_token=your-access-token
GET https://graph.facebook.com/{your-user-id}?fields=birthday,email,hometown
https://graph.facebook.com/{your-user-id}/photos?fields=height,width&access_token={your-user-access-token}"
https://graph.facebook.com/{your-user-id}?fields=feed.limit(3)&access_token={your-access-token}
    feed fields edge の代わりにクエリで fields=feed.limit(3)
    
GET https://graph.facebook.com/(object-id)/(edge-name)?access_token=your-access-token&fields=(fields-name),(fields-name),...

## nodes

|node|object-id|
|---|---|
|user|(facebook-user-id) or me|


## edges
  photos
  feed

## fields
    name field


# 写真、画像取得方法

## ユーザーの写真（複数）

###リクエスト


```http request
GET /{user-id}/photos
```

###レスポンス

```json
{
    "data": [],
    "paging": {}
}
```

## ユーザーの写真（複数）、アップロードしたもの

### リクエスト

```http request
GET /{user-id}/photos?type=uploaded
```
もしくは
```http request
GET /{user-id}/photos/uploaded
```
## 写真単体の情報




概要
Graph APIの利用
リファレンス


TODO
アプリ作る
アクセストークン取得するときパーミッションが必要


