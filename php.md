# PHP

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
