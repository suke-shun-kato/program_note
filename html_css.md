# HTML&CSS

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

## HTMLタグにstyle属性で直書き

```
<p><span style="font-size:larger; color:red;">重要！</span></p>
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

.clearfix::after {
  content: "";
  display: block;
  clear: both;
}
```

# display

## block
p、div、ul、h1〜h6などのタグの初期値はコレ

## inline
a、span、imgなどのタグの初期値はコレ

## inline-block
##none

# margin

## 記述

```css
.class {
    margin: 上 右 下 左;
}
```

## 上でとるか、下でとるか

`<div>`や`<li>`は上

`<h1>`は下

# border

```css
.aaa {
    border: 1px solid #333333;
}

```

# ul,ol,li,

## 「・」を消す

```css
ul {
  list-style: none;
}
```

# em rem px

rem を使うのが一番よいっぽい？

- (CSSのfont-sizeが%とかemとかremとかvwで指定されてると、ビビっちゃう君と僕を救う2分)[https://qiita.com/39_isao/items/e8242901ba1aadb75676]