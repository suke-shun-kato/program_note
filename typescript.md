# --------TypeScript（コマンド）--------


# コマンド
| コマンド    | 同じコマンド   | 省略                   | 説明                                          |
|---------|----------|----------------------|---------------------------------------------|
| node    |          |                      | Node.js 本体（コマンド）                            |
| nvm     |          | Node Version Manager | Node.js のバージョン管理ツール                         |
| npm     |          | Node Package Manager | Node.js のパッケージ管理ツール（Node.jsインストールで使えるようになる） |
| npx     | npm exec |                      | npmでインストールしたパッケージを実行するコマンド                  |
| npx tsc |          | TypeScript Compiler  | TypeScript 本体（コマンド）                         |


# ファイル
- package.json: npmでのインストール済みパッケージが記載されているファイル
- node_modules/: パッケージインストール先のディレクトリ
- tsconfig.json: TypeScriptの設定ファイル

# nvm

## nvm自体をインストール

### nvmインストール前に

いずれかのバージョンのNode.jsのインストールが必要。

[Node.js 公式サイト](https://nodejs.org/ja/)のインストーラーでインストールが推奨らしい。

### nvmインストール

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

※`v0.39.3` の部分は[nvm公式](v0.39.3)に記載されている最新版に適宜置き換える。

### nvmインストール後

Macの場合だが、既に `~/.zshrc` に `nvm` コマンドへのパスを通すコマンドが追加されているが、ターミナルを起動したままだと追記前の状態のままのため、下記のコマンドを実行してパスを通す（または、ターミナル再起動でもいけるはず）

```shell
# 現在のシェルで .zshrc 実行
source ~/.zshrc
```

※`.zshrc` は zshシェルが起動されるたびに読み込まれる、ユーザーがカスタマイズ可能なファイル。

## nvmでNode.jsをインストール

### 指定バージョンをインストール

```shell
# v16.8.0 をインストール
nvm install 16.8.0
```

### 最新版をインストール

```shell
nvm install node
```
### 最新のLTS（安定）版をインストール

```shell
# v16.8.0 をインストール
nvm install stable
```

## Node.jsのバージョン

### 確認

```shell
nvm ls
```

## 切り替え

```shell
# v18.15.0 に切り替え
nvm use 18.15.0
```

```shell
# LTS（安定）のバージョンに切り替え
nvm use stable
```

## default のNode.jsのバージョンを変更する

バージョンを変更してもターミナルを再起動すると default で設定されたバージョンに戻る

```shell
# v19.8.1 をデフォルトに設定する
nvm alias default 19.8.1
```

```shell
# 安定版（stable）のバージョンをデフォルトに設定する
nvm alias default stable
```

# TypeScript のインストール

## インストール前の準備

作業ディレクトリを作成してそこに移動して、下記のコマンドで package.json を作成する

 ```shell
 npm init --yes
 ```

package.json に下記の変更を加える

```json
  "main": "index.js",
```

　↓

```json
  "main": "index.js",
  "type": "module",
```

## インストール

下記で typescript と @types/node のパッケージをインストール

 ```shell
 npm install --save-dev typescript @types/node
 ```

## 初期化

下記でTypeScriptの設定ファイルである tsconfig.json を作成

 ```shell
npx tsc --init
 ```

## TypeScript の設定

```json
{
  "compilerOptions": {
    "target": "es2022",  /* 2016→2022 */

  },
  "include": ["./src/**/*.ts"]    /* 追加、srcディレクトリ以下の全ての.tsファイルがコンパイル対象 */
}
```

# TypeScript の実行

## JavaScriptへのコンパイル（変換）

 tsconfig.json に記載されている環境でTypeScriptがコンパイルされる

 ```shell
npx tsc
 ```

## 変換したJavaScriptの実行

```shell
node dist/index.js
```


# --------TypeScript（文法）--------

# 宣言

```typescript
// 定数、こっちをメインに使う
const aaa: string = "文字ですよ";

// 変数
let bbb: number = 1111; 
```

# 型

|    型     | 説明                                                         |
| :-------: | ------------------------------------------------------------ |
|  number   | 数値。整数や浮動小数点などの違いはない。中身は全て8バイトの浮動小数点） |
|  bigint   | 任意精度整数。ES2020で登場。最大値はない。number型より遅い   |
|  string   | 文字列                                                       |
|  boolean  | 真偽                                                         |
|   null    |                                                              |
| undefined |                                                              |
| シンボル  |                                                              |


# キャスト

```typescript
String(aaa);
Number(aaa);
BigInt(aaa);
Boolean(aaa);
```

# 演算子

| 演算子 | 説明                                     |
| :----: | ---------------------------------------- |
|   +    | 足し算。文字列結合。``を使った方がよい。 |
|   -    |                                          |
|   *    |                                          |
|   /    |                                          |
|   %    | 余り                                     |
|   **   | 乗数。2**5 は 2の5乗                     |

# オブジェクト

## スプレッド構文

JavaScriptの構文

```typescript
const o1 = {aaa: {bbb: 9}};
const o2 = {...o1};
o1.aaa.bbb = 10;
console.log(o2.aaa.bbb); // 10になる
```

## プロパティで型を宣言するとき

```typescript
const o: {
    aaa: number, 
    bbb?: string 
} = {
    aaa: 111, 
    bbb: "text"
};
```

- type interfaceは使わない
- ? オプショナル
- 部分型

# 関数

## 関数宣言

```typescript
function range(min: number, max: number): number[] {
    const result = []
    for (let i=min; i<max; i++) {
        result.push(i)
    }
    
    return result
}
```

## 関数式

```typescript
// 定義の前でも関数を使える
let numbers = range(1,3)

// 定義
const range = function (min: number, max: number): number[] {
    const result = []
    for (let i=min; i<max; i++) {
        result.push(i)
    }
    
    return result
}
```

## アロー関数式

### 通常形

```typescript
// 定義
const calcAverage = (num1: number, num2: number): number => {
    return (num1 + num2) / 2
}

// 実行。定義の後でないといけない。
let numbers = calcAverage(2,5)
```

### 省略形

```typescript
// 定義
const calcAverage = (num1: number, num2: number): number => (num1 + num2) / 2

let numbers = calcAverage(2,5)
```

### メソッド記法
