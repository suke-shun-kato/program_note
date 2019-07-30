# Android

# Preference

## preference key

- リソースに記述するのが一番
- preferences.xml とコード内両方で使うた
- Constant（定数）には書かない方がよい

# AsyncTask
https://qiita.com/maromaro3721/items/c9e16068d13e8ca217f5

# Intent

## アクティビティ、サービス、ブロードキャスト

|受信|送信|明示的／暗示的|備考|
|:---|:--- |:--- |:---|
|Activity|`Context#startActivity()`|明示的・暗示的| |
| |`Activity#startActivityForResult()`|明示的・暗示的いける？|Intent先から戻るとき値を返せる|
|Service|`Context#startService()`|明示的・暗示的は非推奨|サービスを起動|
| |`Context#bindService()`|明示的・暗示的はエラー|バインドされたサービスを開始|
|BroadcastReceiver|システム|明示的・暗示的は非推奨のがある|Wi-Fiや電池の状態など|
| |`Context#sendBroadcast()`|明示的・暗示的はいけるか？| |
| |`Context#sendOrderedBroadcast()`|明示的・暗示的はいけるか？|これは何か調べる|
| |`Context#sendStickyBroadcast()`| |非推奨（API21,Android5.0から）|

BroadcastReceiverは`Context#registerReceiver()`で登録できる。

ContextはActivity,Serviceなど

## 明示的・暗示的

### 明示的Intent

### 暗示的Intent

# AsyncTask, 非同期

ベストプラクティスは下記になる。

```
public class TwitterOAuthPreference extends Preference {

    private OAuth1RequestToken mOAuth1RequestToken;
    
    protected void onClick() {
        GetRequestTokenAsyncTask getRequestTokenAsyncTask = new GetRequestTokenAsyncTask(
                this, mOAuth10aService, mTextCantAccessAuthPage);

        // requestTokenを取得 → それで認可ページを開く
        getRequestTokenAsyncTask.execute();
    }
    
    /************************************
     * リクエストトークンを取得 → 認可ページへ飛ばす
     *
     * 非同期処理中に画面回転などが起こると途中で中断される
     */
    private static class GetRequestTokenAsyncTask extends  AsyncTask<Void, Void, OAuth1RequestToken> {
        private final OAuth10aService mOAuth10aService;
        private final String mTextCantGetRequestToken;
        private final WeakReference<TwitterOAuthPreference> mTwitterOAuthPreferenceWeakReference;

        GetRequestTokenAsyncTask( TwitterOAuthPreference twitterOAuthPreference,
                OAuth10aService oAuth10aService, String textCantGetRequestToken) {

            super();

            mTwitterOAuthPreferenceWeakReference = new WeakReference<>(twitterOAuthPreference);
            mOAuth10aService = oAuth10aService;
            mTextCantGetRequestToken = textCantGetRequestToken;
        }

        /************************************
         * RequestTokenの取得
         * WEBにアクセスして時間がかかるので非同期処理
         * @return リクエストトークン
         */
        @Override
        @Nullable
        protected OAuth1RequestToken doInBackground(Void... params) {
            try {
                return mOAuth10aService.getRequestToken();
            } catch (Exception e) {
                return null;
            }
        }

        /************************************
         * 認可ページを開く
         * 非同期処理の結果処理、
         * @param oAuth1RequestToken リクエストトークン
         */
        @Override
        protected void onPostExecute(@Nullable OAuth1RequestToken oAuth1RequestToken) {
            TwitterOAuthPreference twitterOAuthPreference
                    = mTwitterOAuthPreferenceWeakReference.get();

            // フィールドに保存
            twitterOAuthPreference.setOAuth1RequestToken(oAuth1RequestToken);

            if (oAuth1RequestToken == null) {
            // リクエストトークン取得できなかったとき（ネットつながってないとき）
                Toast.makeText(
                        twitterOAuthPreference.getContext(),
                        mTextCantGetRequestToken, Toast.LENGTH_SHORT)
                        .show();
            } else {
            // リクエストトークン取得できたとき
                //// 認可ページにインテント
                // 認可ページのURLを取得
                String urlAuthorizationPage = mOAuth10aService.getAuthorizationUrl(oAuth1RequestToken);

                Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(urlAuthorizationPage));
                twitterOAuthPreference.getContext().startActivity(intent);
            }

        }
    }

    /**
     * AsyncTask内で使う用
     * @return mOAuth1RequestToken
     */
    private OAuth1RequestToken getOAuth1RequestToken() {
        return mOAuth1RequestToken;
    }

    /**
     * * AsyncTask内で使う用
     * @param oAuth1RequestToken oAuth1RequestToken
     */
    private void setOAuth1RequestToken(OAuth1RequestToken oAuth1RequestToken) {
        mOAuth1RequestToken = oAuth1RequestToken;
    }
}
```

# API level（API レベル）

```
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1) {
    ...
}
```

# debug用の設定

## 基本的な考え

- AndroidのアプリケーションIDの後ろに `.debug` をつける

`xyz.goodistory.autowallpaper` だと `xyz.goodistory.autowallpaper.debug`

- Javaのパッケージ名はそのまま

`xyz.goodistory.autowallpaper` だと `xyz.goodistory.autowallpaper` のまま


## 基本の変更

下記URLを参考に設定する
[Androidのデバッグ版とリリース版でパッケージ名、バージョン名、アプリ名、アイコンを変更する](https://qiita.com/masaibar/items/87cd03d3824ae8e1e16a)

## マニフェスト app/src/main/AndroidManifest.xml

```
<provider
    android:authorities="${applicationId}.fileprovider" />
<!-- APPLICATION_ID -->
```

## ソースコード内でのアプリケーションID取得

` Context#getPackageName() ` で `.debug` 付きのIDを取得できる


# アノテーション（@,annotation）

## @RequiresApi  @TargetApiの違い

`@RequiresApi`は呼び出し元でAPIレベルによる処理の分岐を書かないといけない

`@TargetApi`は単にワーニングを出すだけ

### 記述例

```
@RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
```


```
@TargetApi(Build.VERSION_CODES.LOLLIPOP)
```


# パーミッション

## パーミッション許可が必要なときの処理 

```
// パーミッションの許可状態を取得
final int permissionStatus = ContextCompat.checkSelfPermission(
                getContext(), Manifest.permission.READ_EXTERNAL_STORAGE);
if (permissionStatus == PackageManager.PERMISSION_GRANTED) {
// 許可OKの状態の場合
        // 普通に処理を書く

} else {
// 許可NG、していない状態の場合
    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.READ_CONTACTS)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[] {Manifest.permission.READ_CONTACTS},
                MY_PERMISSIONS_REQUEST_READ_CONTACTS);

        // MY_PERMISSIONS_REQUEST_READ_CONTACTS is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```

https://developer.android.com/training/permissions/requesting.html

## 初期化コマンド

```
$ adb shell pm clear パッケージ名
```