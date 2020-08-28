# Apache

# .htaccess

## RewriteRule

```
# 下記を書かないといけない
RewriteEngine on
# リダイレクト設定
RewriteRule ^old.html$ http://www.example.com/new.html [L,R=301] 
```

### 301リダイレクト

301 Moved Permanently 恒久的に移動した

```
RewriteEngine on
RewriteRule ^old.html$ http://www.example.com/new.html [L,R=301] 
```


### 302リダイレクト

302 Found


[R=302]

[R]・・・Rだけだと302リダイレクトになる