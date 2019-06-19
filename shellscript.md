# シェルスクリプト

#curl ステータスコード

```
$ curl -s http://localhost:8080/app/health -o /dev/null -w '%{http_code}\n'
```