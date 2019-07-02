# javascript

# DOM

## chrome でのフォームの確認方法

```
document.forms[0]   // フォームが２つある場合はforms[1]などにする
```

下記でどこのURLにGET/POSTしているかわかる

```
document.forms[0].action
```