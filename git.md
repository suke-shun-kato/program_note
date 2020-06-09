# Git

# 初期設定

[gitの初期設定 - Qiita](https://qiita.com/suke/items/041cb9c66af96370d51a)

# git log Windows（コマンドプロンプト）で文字化け

[Windows10のgit logが文字化け（<E8><A4>...)するときの対処](https://qiita.com/Tachibana446/items/b6a869afa9959581dfc0)

# sshの設定

## SSH鍵の生成

```
ssh-keygen -t rsa -f (ファイル名)
```

- -t 鍵作成アルゴリズム（RSA形式）
- -f 鍵ファイル名
※最終的に「~/.ssh」に鍵を置くので「~./ssh」で上記コマンドを実行した方が良い

パスフレーズは設定しなくても良いのでEnter2回

- 秘密鍵：(ファイル名)
- 公開鍵：(ファイル名).pub
が作成される

## 公開鍵を設定

SSH鍵の中身の文字列をコピー(mac)

```
pbcopy < ~/.ssh/id_rsa.pub
```

GitHubやBitbucketなどの設定ページで上記を貼り付ける

## ホストの設定

~/.ssh/config

```
Host bitbucket.org
  IdentityFile ~/.ssh/bitbucket
  User git

Host github.com
  IdentityFile ~/.ssh/github
  User git

```

## SSHで繋がるかの確認

下記はBitbucketの場合

```
ssh -T git@bitbucket.org
```