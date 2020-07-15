# シェルスクリプト

# 行の先頭

## bash使う場合

```
#!/bin/bash
```

## これは普通のシェル？

```
#!/bin/sh
```

# chmod ファイル権限変更

```
chmod 777 hoge.txt
```

|モード(数字)|モード(アルファベット)|権限|
|---|---|---|
|4|r|読み取り|
|2|w|書き込み|
|1|x|実行|

```
chmod u+x hoge.txt
```

# curl ステータスコード

```
$ curl -s http://localhost:8080/app/health -o /dev/null -w '%{http_code}\n'
```