# API

基本的に[Web API: The Good Parts](https://www.amazon.co.jp/Web-API-Parts-%E6%B0%B4%E9%87%8E-%E8%B2%B4%E6%98%8E/dp/4873116864)の記載内容をまとめたもの

# URL

後ほど追記する

# HTTPヘッダー

## 基本（クライアント）

リクエストで下記を必ず指定する。

- Content-Type
- Accept

### Content-Type

- クライアント側がリクエストでどんなデータ形式を送信したか

#### 例

JSON形式

```
Content-Type: application/json
```

RFC 8259でアップデートされたJSON仕様では、文字のエンコードとして「UTF-8」が必須となったので、最近は下記の書き方はしない。

```
Content-Type: application/json; charset=UTF-8
```

### Accept

- クライアント側がどんなデータ形式が欲しいか

#### 例

```
Accept: application/json
```

## 基本（サーバー）

### Content-Type

- サーバー側がレスポンスでどんなデータ形式を送信したか

```
Content-Type: application/json
```

こちらも、`application/json; charset=UTF-8` という書き方はしない。

## キャッシュ（サーバー）

基本的にサーバー側がキャッシュの状況を定義したヘッダーを返して、クライアント側に制御してもらう。

キャッシュの状況を定義したヘッダーは下記のどちらかになる。

- Cache-Control 
- Expires 

### Cache-Control

- HTTP1.1で定義
- 現在時刻からの秒数
- Dateヘッダーに現在の時刻が記載されているので、それから計算する

#### 例、現在から3600秒後にキャッシュが切れる

```
Cache-Control: max-age:3600
```

#### 例、キャッシュをして欲しくない

```
Cache-Control: no-cache
```

厳密には「キャッシュしたデータは検証モデルによって確認が必要」という意味だが、単にキャッシュをして欲しくない場合で使われる場合もある。

「検証モデル」については後ほど追記予定（現在は説明しない）

#### 例、キャッシュをしてはならない

```
Cache-Control: no-store
```

### Expires

- HTTP1.0 で定義
- 有効期限日時
- RFC1123形式で日時を記載する

#### 例

##### 2014/10/4 23:59:45 (GMT) までキャッシュできる

```
Expires: Thu, 04 Oct 2014 23:59:45 GMT
```

## セキュリティ（サーバー）

### XSS対策

サーバー側で下記のヘッダーありのレスポンスを返す

```
Content-Type: application/json
```

下記のヘッダは一時期サポートされたが、現在はサポートされていないので不要

```
X-XSS-Protection: 1; mode=block
```

### JSONハイジャック

基本的にJSONハイジャック対策（古いIEではXSS対策にもなる）

```
X-Content-Type-Options: nosniff
```


### iframe対策内に表示させないように


iframe内で表示できないようにする

```
X-Frame-Options: deny
```

## セキュリティー（クライアント）

### CSRF対策

- クライアント側では下記のヘッダーを付けてリクエストをする

- フォームによるXSRFさせない対策（フォームはX-Requested-With のヘッダーは付けられないため）

```
X-Requested-With: XMLHttpRequest
```

XMLHttpRequest(XHR) はJavaScriptのオブジェクト名。JsonやXMLを送るときに使用されるオブジェクト。

また、**サーバー側で XMLHttpRequest があるかどうかチェックする。**ない場合はレスポンスを返さない。



## CORS（サーバー）

サーバー側がレスポンスで下記ヘッダーを返して、ブラウザがその値を見てOKかNGかを判断する

- Access-Control-Allow-Origin
- Access-Control-Allow-Methods
- Access-Control-Allow-Headers

### Access-Control-Allow-Origin

- レスポンス時にブラウザがOrigin（フロント側のURL）とAccess-Control-Allow-Origin（サーバーが返す値） が同じURLか判断してOKかNGかを決める


#### 例、URL指定

```
Access-Control-Allow-Origin: http://localhost:8080
```

#### 例、どのURLでもOK

```
Access-Control-Allow-Origin: *
```

#### サーバー側の実装

サーバー側で返すURLが指定されている場合は、リクエストの `Origin`  ヘッダーからURLを取得して指定されているURLかどうかチェックする。NGの場合は `403 Forbidden` を返す。

# レスポンスボディ

後ほど記載