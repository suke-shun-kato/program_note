# javascript

# chromeでのconsoleでの確認

## HTMLElement型のときコンソール出力が、HTMLではなくクラスで出力したい場合

```
consoler.dir(element)
```

## <form>

```javascript
document.forms[0]   // フォームが２つある場合はforms[1]などにする
```

### 下記でどこのURLにGET/POSTしているか分かる

```javascript
document.forms[0].action
```

## 下記で指定formの全てのinputなどのnameが分かる

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