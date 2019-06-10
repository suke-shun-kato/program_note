# Twitter API

## ページ
[twitter developer](https://developer.twitter.com/)

## 各アプリの設定
1. アカウント名 > Apps 
1. アプリを選択

## APIリファレンス
1. Docs
1. [API reference index](https://developer.twitter.com/en/docs/api-reference-index)

## token (Access Token)取得までの流れ

### はじめに

今はOAuth1.0 でしている（もしかしたら2.0でできるかも、一部だけの機能かも）

### 図

[これ](https://www.slideshare.net/nemupm/o-auth-one)

### アクセストークン取得ページ

下記の2種類ある

#### GET oauth/authenticate

- 認証
- callback urlにリダイレクト

#### GET oauth/authorize
- 認可
- PIN Code