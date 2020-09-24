# Laravel

# リンク集

- [Laravel - 公式](https://laravel.com/)
- [Document（ドキュメント）- ReadDouble](https://readouble.com)・・・こっちのが見やすい
- [APIドキュメント(8.x)](https://laravel.com/api/8.x/)

## 用語

- LTS・・・Long Term Support（LTS）


# 基本

## 流れ

1. ユーザーのリクエストがwebサーバーへ到着
1. public/index.phpを実行
1. composerから出力されたvendor/autoload.phpが実行 → クラスマップの情報を取得
1. bootstrap/app.phpを実行
1. Illuminate\Foundation\Applicationがnewされインスタンス化
1. Kernelが生成され、HTTPリクエスト情報からRequestインスタンスを生成しKernel->handle()メソッドへ渡し処理
1. 実行結果をユーザーへレンスポンス

## ディレクトリ構成

|ディレクトリ|役割|
|:---|:---|
|app/Http/Kernel.php|グローバルミドルウェアの登録、ミドルウェアのグループ登録|
|app/Http/Controllers/|コントローラー|
|app/Http/Middleware/|ミドルウェア|
|app/Providers/|サービスプロバイダ、DI|
|config/app.php|サービスプロバイダの登録|
|routes/*.php|ルーター、ミドルウェアの登録|
|routes/api.php|api用|
|routes/channels.php|ブロードキャストチャンネル、WEBソケットとか|
|routes/console.php||
|routes/web.php|通常はこれ、WEB用|
|vendor/|ライブラリ|


## フレームワーク名前空間構成

|名前空間|役割|
|:---|:---|
|Illuminate\Foundation\Application|大本のapp、$this->app|
|Illuminate\Support\Facades|ファサード|

`vendor/laravel/framework/src/` がライブラリのディレクトリ

# ルーティング、ミドルウェア

[Laravel 7.x ルーティング - 公式](https://readouble.com/laravel/7.x/ja/routing.html)

## ファイル

`routes/` に全てルーティングの設定が入っている。

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

# ファサード

staticインターフェースを提供

下記メソッドなど

```
Cache::get('key');
View::make('profile');
```

なるべく使わない方がよい