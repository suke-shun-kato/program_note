# Java


# コンスタラクタ

PHPと違って**継承されない**ので注意！

ただし、例外で暗黙的な呼び出しは起こる

## 暗示的な呼び出し

```java
class AaaClass {
    // コンストラクタ書いてなくても空のコンストラクタが呼ばれている
    /*
    public AaaClass() {
    }
    */
}

```

## 暗黙的な呼び出し、継承

```java
class SuperClass {
    public SuperClass() {
        // なにかの処理
    }
}

class SubClass extends SuperClass {
    // コンストラクタがなくても暗黙的に下記のコンストラクタが書かれているのと同じ挙動をする
    /*
    public SubClass() {
        super();
    }
    */
}
```


# オーバーロード, overload

- 同じメソッド名で引数が異なるものを複数定義できる
- abstract で引数を可変で定義することはできない
- 戻り値は同じ型にしないといけない

```
    public int aaa() {
        return 0;
    }
    public int aaa(int num) {
        return 100/num;
    }

```



# 例外, エラー, Exception, Error

## 継承
```
Throwable
    Error // プログラムで捕捉すべきではない
        OutOfMemoryError
        ....
    Exception // 検査例外, プログラム中で捕捉しなければならない
        IOException
        SQLException
        ....
        RuntimeException // 実行時例外, プログラムの中で補足しなくてもいい, 実行時例外をどこでも捕捉しなければ、最終的に例外が発生したスレッドが終了します。
            NumberFormatException
            ArithmeticException
            OutOfBoundsException
                ArrayIndexOutOfBoundsException
            ClassCastException
            IllegalArgumentException
                NumberFormatException
            IllegalStateException
            NullPointerException
            ....
```

## 参考

- [Javaの例外処理について](https://qiita.com/k4h4shi/items/2c39edaeef1c92f6644a)

