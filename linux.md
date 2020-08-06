# linux

# ipアドレス、ドメイン調査

`nslookup`と`host`がある

`nslookup`の方が詳しい

## URLからip addressを調べる

```
nslookup (URL)   # http:// の部分はなし
```

# chmod ファイル権限変更

## 直接数字で指定

```
chmod 777 hoge.txt
```

|モード(数字)|モード(アルファベット)|権限|
|---|---|---|
|4|r|読み取り|
|2|w|書き込み|
|1|x|実行|

## ＋とか使って指定する場合

```
chmod u+x hoge.txt
```

# curl ステータスコード

```
$ curl -s http://localhost:8080/app/health -o /dev/null -w '%{http_code}\n'
```