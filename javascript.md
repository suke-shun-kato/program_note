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

### 下記でどこのURLにGET/POSTしているかわかる

```javascript
document.forms[0].action
```

## <select>

```javascript
document.getElementsByName('select_name')[0];
```

valueの値は下記

```javascript
document.getElementsByName('select_name')[0].value;
```