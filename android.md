# Android


# ViewModel, LiveData

- [qiita - とにかく簡単にViewModelまとめ](https://qiita.com/KIRIN3qiita/items/7d833e2c010c0b2c02d9)
- [qiita - とにかく簡単にLiveDataまとめ](https://qiita.com/KIRIN3qiita/items/6f5c467a8abc7b89cbe7)


# メモ

class MainActivity: AppCompatActivity() {

    lateinit var textView: TextView

    // some transient state for the activity instance
    var gameState: String? = null

    companion object {
        const val GAME_STATE_KEY: String = "game_state_key"
        const val TEXT_VIEW_KEY: String = "text_view_key"
    }

    override fun onCreate(savedInstanceState: Bundle?) {


        // いつもの
        super.onCreate(savedInstanceState)

        setContentView(R.layout.main_activity)

        // savedInstanceState から値を再取得する
        gameState = savedInstanceState?.getString(GAME_STATE_KEY)

        // initialize member TextView so we can manipulate it later
        textView = findViewById(R.id.text_view)
    }


    //////// 回転
    // onStart() の後に値を復活させたいときに呼ぶ、アクティビティ破棄されないとこれは呼ばれてないので、実質onCreate() と同じ感じだがタイミングが違う
    override fun onRestoreInstanceState(savedInstanceState: Bundle?) {
        // savedInstanceState から値を再取得する
        textView.text = savedInstanceState?.getString(TEXT_VIEW_KEY)
    }

    // 最近はViewModelの方が良いらしい
    // invoked when the activity may be temporarily destroyed, save the instance state here
    override fun onSaveInstanceState(outState: Bundle?) {
        outState?.run {
            putString(GAME_STATE_KEY, gameState)
            putString(TEXT_VIEW_KEY, textView.text.toString())
        }
        // call superclass to save any view hierarchy
        super.onSaveInstanceState(outState)
    }

    ///// View の高さや幅をとりたいとき
    override public void onWindowFocusChanged(boolean hasFocus) {

        // フォーカスOFF（表示がOFF）になるときは途中で切り上げ
        if (!hasFocus) {
            return
        }
    }
}


12-09 22:57:13.727 D/Lifecycle: onCreate
12-09 22:57:13.879 D/Lifecycle: onStart
12-09 22:57:13.889 D/Lifecycle: onResume
--------------- 画面回転など -----------------------
12-09 22:57:19.375 D/Lifecycle: onPause
12-09 22:57:19.375 D/Lifecycle: onSaveInstanceState
12-09 22:57:19.379 D/Lifecycle: onStop
12-09 22:57:19.379 D/Lifecycle: onDestroy
12-09 22:57:19.434 D/Lifecycle: onCreate
12-09 22:57:19.507 D/Lifecycle: onStart
12-09 22:57:19.508 D/Lifecycle: onRestoreInstanceState
12-09 22:57:19.510 D/Lifecycle: onResume



///////// フラグメントのベストプラクティス
class ExampleFragment : Fragment() {
    //// いつもの
    override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle? ): View {

        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.example_fragment, container, false)
    }


    //// Activity にメソッドを実装させたい場合
    interface OnArticleSelectedListener {
        fun onArticleSelected(articleUri: Uri)
    }
    var listener: OnArticleSelectedListener? = null
    override fun onAttach(context: Context) {
        super.onAttach(context)

        listener = context as? OnArticleSelectedListener
        if (listener == null) {
            throw ClassCastException("$context must implement OnArticleSelectedListener")
        }

    }


    //// 回転
    companion object {
        const val KEY_AAA: String = "curChoice"
    }
    // 保存
    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        outState.putInt(KEY_AAA, curCheckPosition)
    }
    // 復帰は onCreateView() or onActivityCreated() のとき、任意のタイミングで



    ///// 初期化
    companion object {

        const val KEY_INDEX = "index"
        const val KEY_INDEX2 = "index2"
        /**
         * Create a new instance of DetailsFragment, initialized to
         * show the text at 'index'.
         */
        fun newInstance(index: Int, index2: String): ExampleFragment {

            return DetailsFragment().apply {
                arguments = Bundle().apply {
                    putInt(KEY_INDEX, index)
                    putString(KEY_INDEX2, index2)
                }
            }
        }
    }
}

///// view のベストプラクティス
class KotlinView : ConstraintLayout {

    constructor(context: Context) : this(context, null)
    constructor(context: Context, attrs: AttributeSet?) : this(context, attrs, 0)
    constructor(context: Context, attrs: AttributeSet?, defStyleAttr: Int): this(context, attrs, defStyleAttr, 0)
    constructor(context: Context, attrs: AttributeSet?, defStyleAttr: Int, defStyleRes: Int): super(context, attrs, defStyleAttr) {

        val a = context.obtainStyledAttributes(
                attrs, R.styleable.NavFooterButton, defStyleAttr, defStyleRes)
        try {
            a.getText(R.styleable.NavFooterButton_footer_label_text)
            a.getDrawable(R.styleable.NavFooterButton_footer_iconImage_drawable)
        } finally {
            a.recycle()
        }
    }
}




# 開発の基礎

## アプリケーションのコンポーネント

下記の4要素から構成される

- アクティビティ
- サービス
- ブロードキャスト レシーバ
- コンテンツ プロバイダ

## 参考URL
- [公式 開発の基礎](https://developer.android.com/guide/topics/fundamentals.html?hl=ja)


# fragment フラグメント

## アクティビティのレイアウトXMLで作成

コード内で書かなくよい

res/layout/activity.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment android:name="com.example.news.ArticleListFragment"
            android:id="@+id/list"
            android:layout_weight="1"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
    <fragment android:name="com.example.news.ArticleReaderFragment"
            android:id="@+id/viewer"
            android:layout_weight="2"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
</LinearLayout>
```

## Activityのコード内で作成

```java
import androidx.appcompat.app.AppCompatActivity;

public class MySettingsActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // フラグメントを動的に追加
        getSupportFragmentManager()
            .beginTransaction()
            // activityのxmlのフラグメントを入れる場所を指定
            .add(R.id.fragment_container, new ExampleFragment())    // replace()とかでもいける？
            .commit();
    }
}
```


## 参考
- [公式 フラグメント](https://developer.android.com/guide/components/fragments?hl=JA)




# Locale と TimeZone

## Locale

言語と国（時差TimeZoneではない）

### androidに設定されている値を取得

```java
 Locale.getDefault();    // java.util.Locale
```

## TimeZone

タイムゾーン、時差

### 日付取得、Dateオブジェクト取得

#### 設定されているタイムゾーンで取得

```java
// Locale.getDefault()は時差ではない、言語・地域の設定
// androidに設定されているデフォルトのTimeZoneが設定される
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("HH:mm", Locale.getDefault());

// 日時を取得
Date date = simpleDateFormat.parse(yyyymmddhhmmss);
```

#### コード上でタイムゾーンを設定して取得

```java
// インスタンスを取得
SimpleDateFormat simpleDateFormat = (SimpleDateFormat) SimpleDateFormat.getDateTimeInstance();

// 文字列の形式をセット
simpleDateFormat.applyPattern("yyyy-MM-dd hh:mm:ss");

// TimeZoneを指定, ここでは +0時間を指定
simpleDateFormat.setTimeZone( TimeZone.getTimeZone("UTC") );

// 文字列から日時を取得
Date date = simpleDateFormat.parse("2019/02/02 15:12:00");
```

### androidに設定されている値を取得

```java
TimeZone.getDefault();  // java.util.TimeZone;
```

### Dateオブジェクトからunixtimeを取得

```java
// unixtime, ミリ秒
date.getTime();
```


# View, Preference

## コンストラクタ

下記の4つのコンストラクタがあるが、使い分けは？

```java
View(Context context)
View(Context context, AttributeSet attrs)
View(Context context, AttributeSet attrs, int defStyleAttr)
View(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) // API Level 21から
```

3番目と4番目は下記のスタイル定義を使う（2番目もデフォルトのを使用しているが・・・）
- `res/values/styles.xml` 

XMLの属性を増やす場合
- `res/values/attrs.xml`

### 参考リンク

- [Qiita Viewの4つのコンストラクタ](https://qiita.com/alzybaad/items/aca3049b6a13ab78f945)

## inflate

下記の感じでinflate() する

 ```java
// LauoutInflate のインスタンスを取得
LayoutInflater inflater = LayoutInflater.from(context);

// resIDは XMLファイルのID R.layout.main_page など
inflater.inflate(resId, rootViewGroup);
```

# Preference

## preference key

- リソースに記述するのが一番
- preferences.xml とコード内両方で使うた
- Constant（定数）には書かない方がよい


## custom preference カスタムプリファレンス

1. preference の preferences.xml 


```xml
ダイアログに表示するレイアウトファイルを作成してxmlにセット
android:dialogLayout="@layout/fragment_time_preference_dialog"
```

2. sss

```java

   @Override
    public void onBindViewHolder(PreferenceViewHolder holder) {
        super.onBindViewHolder(holder);

        //// クリックしたときリスナーをセット
        getPreferenceManager().setOnDisplayPreferenceDialogListener(
                new PreferenceManager.OnDisplayPreferenceDialogListener() {
                    /**
                     * Called when a preference in the tree requests to display a dialog.
                     *
                     * @param preference The Preference object requesting the dialog.
                     */
                    @Override
                    public void onDisplayPreferenceDialog(Preference preference) {
                        preference.setFragment(TimePreferenceDialog.class.getName());
                    }
                });
    }
```

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




# FileProvider　ファイルプロバイダ

ファイルプロバイダはファイルに関する**コンテンツプロバイダ**

主に「`file://` スキーマ（ `File` クラス、ファイルpath）」から「 `content://`スキーマ (`Uri` クラス)」に変換するために使用する

※現在のandroidではfileスキーマは直接使えないので、contentスキーマに変換する必要がある


## ファイルの種類

Androidには下記の種類のファイルがある

- 内部ストレージ

他のアプリからアクセスできない

- 外部ストレージ

他のアプリからアクセスできる、逆に他のアプリの外部ファイルへアクセスできる

## マニフェストでの宣言

ファイルプロバイダを使うには、下記のマニフェストの記述をしないといけない

### manifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
    package="xyz.goodistory.autowallpaper">
    <provider
        android:name="androidx.core.content.FileProvider"
        android:authorities="${applicationId}.fileprovider"
        android:grantUriPermissions="true"
        android:exported="false">

        <!--ファイルプロバイダで使うストレージの種類を記述-->
        <meta-data
            android:name="android.support.FILE_PROVIDER_PATHS"
            android:resource="@xml/file_paths" />
    </provider>
</manifest>
```

`${applicationId}` で自分のアプリケーションIDを取得できる（例: `xyz.goodistory.autowallpape.debug`）

### /res/xml/file_paths.xml

上記の `@xml/file_paths` は下記のように記述する

```xml
<paths>
    <!--下記のうち最低1つ記述しないといけない-->

    <!--内部ストレージ-->
    <files-path name="name" path="images/"/>     
    <cache-path name="name" path="./" />

    <!--外部ストレージ、他アプリ-->
    <external-path name="name" path="./" />

    <!--外部ストレージ、自アプリ-->
    <external-files-path name="name" path="./" />
    <external-cache-path name="name" path="./" />
    <external-media-path name="name" path="./" />
</paths>
```


## `File` クラス（fileスキーマ）からcontentスキーマのuriを取得するプログラム

```java
// 内部ストレージの画像が入っているディレクトリを取得
File imagePath = new File(Context.getFilesDir(), "images");

// 画像ディレクトリのdefault_image.jpgのパスを取得
File newFile = new File(imagePath, "default_image.jpg");

// contentスキーマに変換, 
// getPackageName()はアプリケーションID名
Uri contentUri = getUriForFile(getContext(), getPackageName() + ".fileprovider", newFile);
```

## 対応表


|ファイルのタイプ|xml要素名|XML`path`属性|メソッド|ファイルPath|
|:---|:---|:---|:---|:---|
|内部ストレージ、ファイル|files-path|./|context.getFilesDir()|/data/user/0/xyz.goodistory.autowallpaper.debug/files|
|内部ストレージ、キャッシュ|cache-path|./|context.getCacheDir()|/data/user/0/xyz.goodistory.autowallpaper.debug/cache|
|外部ストレージ、他アプリ|external-path|./|Environment.getExternalStorageDirectory() API29で非推奨|/storage/emulated/0|
| | |pictures/|Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_PICTURES) API29で非推奨|/storage/emulated/0/Pictures|
| | |music/|Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_MUSIC) API29で非推奨|/storage/emulated/0/Music|
|外部ストレージ、自アプリ、ファイル|external-files-path|./|context.getExternalFilesDir(null)|/storage/emulated/0/Android/data/xyz.goodistory.autowallpaper.debug/files|
| | |pictures/|context.getExternalFilesDir(Environment.DIRECTORY_PICTURES)|/storage/emulated/0/Android/data/xyz.goodistory.autowallpaper.debug/files/Pictures|
| | |music/|context.getExternalFilesDir(Environment.DIRECTORY_MUSIC)|/storage/emulated/0/Android/data/xyz.goodistory.autowallpaper.debug/files/Music|
|外部ストレージ、自アプリ、キャッシュ|external-cache-path|./|context.getExternalCacheDir()|/storage/emulated/0/Android/data/xyz.goodistory.autowallpaper.debug/cache|
|外部ストレージ、自アプリ、メディア|external-media-path|./|context.getExternalMediaDirs() API21～|/storage/emulated/0/Android/media/xyz.goodistory.autowallpaper.debug|

※アプリケーションIDが `xyz.goodistory.autowallpaper.debug` のとき

※ファイルPathは機種によって異なるかも、ZenFone4で確認

## 参考リンク

- [公式 - ストレージオプション](https://developer.android.com/guide/topics/data/data-storage?hl=ja)
- [公式 - FileProvider](https://developer.android.com/reference/android/support/v4/content/FileProvider)
- [Qiita - Android 画像ファイルを扱う際のFileとUriまとめ](https://qiita.com/wakamesoba98/items/98b79bdfde19612d12b0)




# MediaStore

SQLiteにアクセスするときと同じようにContentResolver経由でアクセスする

## 参考リンク

- [公式 - コンテンツ プロバイダの基本](https://developer.android.com/guide/topics/providers/content-provider-basics#ContentURIs)


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

` Context#getPackageName() ` で `.debug` 付きのアプリケーションIDを取得できる

※メソッド名がPackageNameになっているがJavaのパッケージ名ではない


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

```shell script
adb shell pm clear パッケージ名
```

パッケージ名は下記のコマンドで出る

```shell script
adb shell pm list package
```


# adb

```shell script
# パッケージ名一覧
adb shell pm list package

# パーミッション一覧
adb shell pm list permissions

# アプリの初期化（パーミッションなど）
adb shell pm clear パッケージ名
```




