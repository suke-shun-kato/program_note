# PHP

# ECサイトベスト・プラクティス

## GET, 表示ページ

### INSERT（新規作成）用

1. URLの値やクエリパラメータの値（customer_idやproduct_idなどの値）をバリデーション
1. 初期値を取得（すでに`<form>`の`name`と同じ構造になっている）
1. バリデーションエラー時に戻ってきた場合は既に入力した`<form>`の`name`の値を上書きする
1. 表示

### UPDATE（編集・更新）用

1. URLの値やクエリパラメータの値（customer_idやproduct_idなどの値）をバリデーション
1. DBからデータ取得（Model取得
1. その値を`<form>`用に値を変換
1. バリデーションエラー時に戻ってきた場合は既に入力した`<form>`の`name`の値を上書きする
1. 表示

## POST, INSERT/UPDATE処理

###  INSERT

1. `<form>`の値をバリデーション
1. バリデーションNGのときはセッションに`<form>`の値を保存して元のページにリダイレクト
1. `<form>`の`name`のデータ構造からDB用のデータ構造に変換（コントローラーかモデルにメソッド作るのが良い）
1. DBに保存（トランザクションはコントローラーの一番外側でかけるのが良い）

# 例外, エラー, Exception, Error

## 継承
```
Throwable
    Error // アプリケーションロジック中で捕捉してはいけない 
        TypeError
        ParseError
        AssertionError
        ArithmeticError
        DivisionByZeroError
    Exception
        LogicException  // Error と同じようにアプリケーションロジック中で捕捉してはいけない
            BadFunctionCallException
                BadMethodCallException
            DomainException
            InvalidArgumentException
            LengthException
            OutOfRangeException
        RuntimeException 
            OutOfBoundsException 
            OverflowException 
            RangeException 
            UnderflowException 
            UnexpectedValueException 
            PDOException 
```

## アプリケーションロジックで補足catchすべきか

| |してはいけない|なるべくしない|しないといけない|
|:---:|:---|:---|:---|
|システム|Error|||
|ユーザー定義|LogicException|RuntimeException|Exception(Logic,Runtime以外)|

## 参考

- [PHP でどのように Exception/RuntimeException/LogicException を使い分けるか](https://qiita.com/tanakahisateru/items/e3e24f3825c4ba0c60e6)


# HTTPステータスコード

## ステータスコード（status code）をその場で返す  

```
http_response_code(404);
exit; // 必ずすること、しないとプログラムがそのまま走る
```

# DOM(HTML,XML)

## SimpleXMLElement

### 文字列に変換

```
$simpleXMLElement->asXML();
```

## DomDocument

### root作成

```
// <?xml version="1.0" encoding="UTF-8"？> ？大文字→小文字にする
$dOMDocument = new DOMDocument('1.0', 'UTF-8');
```

### 新規要素作成

```
$dOMElement = $dOMDocument->createElement('name', 'value'); // <name>value</name>
// new DOMDocument() は読み取り専用になるからダメ！
```

### 要素作成＆追加

```
$dOMNode = $dOMNode->appendChild(
    $dOMDocument->createElement('name', 'value') );
```

## クラスの継承関係
DomDocument DOMNode
DomElement DomNode
DOMNode <- DOMDocument DOMのルートクラス
        <- DOMElement
        <- DOMCharacterData <- DOMText

# EXCEL（エクセル）,CSV

## PhpSpreadsheet

今のところこれが一番良い

### 参考サイト

- [PhpSpreadsheetの使い方](https://qiita.com/sudnonk12/items/a0d58cc0f6ff1c6e2765)