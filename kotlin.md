# Kotlin

# Android Developer を読むときのメモ

## !

`context: Context!` は


- Kotlinの `context: Context?` または `context: Context`　
- Java の `Context context` にあたる

```kotlin
(context: Context!, attrs: AttributeSet!, defStyleAttr: Int, defStyleRes: Int)
```

# スコープ関数、`run` `let` `apply` `also` `with` 

## 基本構文

```kotlin
var toolbar = findViewById<Toolbar>(R.id.toolbar)
toolbar.setNavigationIcon(R.drawable.icon)
toolbar.setNavigationOnClickListener(view -> doSomething())

//run
findViewById<Toolbar>(R.id.toolbar)
  .run {
    setNavigationIcon(R.drawable.icon)
    setNavigationOnClickListener({ doSomething() })
  }
```


## 使い分け

`with` だけ構文が違うのであまり使われてないっぽい

| |戻り値|スコープ内のオブジェクト|用途|
|:---|:---:|:---:|:---|
|`run`|スコープ内の戻り値|`this`|対象オブジェクト自身のみで操作|
|`let`|スコープ内の戻り値|`it`|複雑なもの|
|`apply`|対象オブジェクト|`this`|対象オブジェクト自身の処理を書く場合|
|`also`|オブジェクト自身|`it`|複雑なもの|


## 参考リンク
- [Qiita - KotlinのRun, Let, Apply, Alsoを使い分け](https://qiita.com/JohnSmithWithHaruhi/items/e8f411c379483d4902aa)
- [Qiita - Kotlin スコープ関数 用途まとめ](https://qiita.com/ngsw_taro/items/d29e3080d9fc8a38691e)


# コンストラクタ
プライマリコンストラクタが定義されているクラスでは、セカンダリコンストラクタでインスタンスを生成する場合、
直接的または間接的にプライマリコンストラクタを呼び出す必要があります。

init はどのコンストラクタでも実行される

this でプライマリでinitでセカンダリ