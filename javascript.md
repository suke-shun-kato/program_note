# javascript

# for、foreach、配列を回すとき

配列を回すときは `.forEach()` ではなく、 `.filter()`, `.find()`, `.map()`, `.reduce()` を使おう

さらに `for` は `forEach` 以上に使わないようにする

## forEach

```
dogs.forEach(dog => {
  console.log(dog.name);
});
```

## for

```
let fruits = ["apple", "orange", "melon"]; // 長さが３の配列
for (let i = 0; i < fruits.length; i++) {
  console.log( fruits[i] );
}
```

# chromeでのconsoleでの確認

## HTMLElement型のときコンソール出力が、HTMLではなくクラスで出力したい場合

```
consoler.dir(element)
```

## <form>

```javascript
document.forms[i]   // フォームが２つある場合はforms[1]などにする
```

もしくは、例えば `name="aaaa"` のフォームだと下記

```
document.aaaa
```

### 下記でどこのURLにGET/POSTしているか分かる

```javascript
document.forms[0].action
```

### 下記で指定formの全てのinputなどのnameが分かる

```javascript
Array.prototype.forEach.call(document.forms[0], function(item, index) {
    console.log(index + ': ' + item.name);
});
```

## <select>

```javascript
document.getElementsByName('select_name')[0];
```

valueの値は下記

```javascript
document.getElementsByName('select_name')[0].value;
```