# HTML

# CSS（スタイルシート）の書き方

## 読み込み

```html
<html>
<head>
    <link rel="stylesheet" type="text/css" href="xxx.css">
</head>
<body>
    <p>段落となります。</p>
</body>
</html>
```

## HTMLに直書き

```html
<html>
<head>
<style>
    .aaa {
        margin-top: 10px;
    }
</style>
</head>
<body>
  <p class="aaa">段落となります。</p>
</body>
</html>

```

# クリアフィックス

```html
<div class="clearfix">
    <div class="box-a">aaa</div>
    <div class="box-b">bbb</div>
</div>

```

```css
.box-a {
    float: left;
}
.box-b {
    float: right;
}

.clearfix::after{
  content: "";
  display: block;
  clear: both;
}
```