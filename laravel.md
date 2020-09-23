# Laravel

# ルーティング、ミドルウェア

[Laravel 7.x ルーティング - 公式](https://readouble.com/laravel/7.x/ja/routing.html)

## ファイル

`routes/` に全てルーティングの設定が入っている。

下記のファイルがある

- api.php: ブロードキャスト
- channels.php: ブロードキャストチャンネル、WEBソケットとか
- console.php
- web.php: 通常はこれ。Webページ用。

## 基本ルーティング

### 直接ビューを返す

```
Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
    //
    return "post id: $postId, comment id: $commentId";
});
```

`/posts/1/comments/2` のULRの場合は、クロージャーの引数が `$postId = 1`, `$commentId = 2` の値になる

### コントローラーに値を渡す

```
Route::get('/user', 'UserController@index');
```

UserController コントローラーの index アクション

## ミドルウェア

[Laravel 7.x ミドルウェア - 公式](https://readouble.com/laravel/7.x/ja/middleware.html)

```
protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    ....
];
```

ミドルウェアパラメータ

## ルートグループ

```
Route::middleware(['first', 'second'])->group(function () {
    Route::get('/', function () {
        // firstとsecondミドルウェアを使用
    });

    Route::get('user/profile', function () {
        // firstとsecondミドルウェアを使用
    });
});
```

## 名前
```
Route::get('user/{id}/profile', function ($id) {
    //
})->name('profile');

// url取得
$url = route('profile', ['id' => 1, 'photos' => 'yes']);

// /user/1/profile?photos=yes
```