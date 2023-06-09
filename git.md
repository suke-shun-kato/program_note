# Git

# 初期設定

[gitの初期設定 - Qiita](https://qiita.com/suke/items/041cb9c66af96370d51a)

# git log Windows（コマンドプロンプト）で文字化け

[Windows10のgit logが文字化け（<E8><A4>...)するときの対処](https://qiita.com/Tachibana446/items/b6a869afa9959581dfc0)

# コミット時間の変更

## 直近のコミット時間を現在の時間に修正

Author時間もCommiter時間も現在の時刻に修正される

```shell
git commit --amend --reset-author
```

# git push
## 途中までのコミットをpushする

下記のコマンドでpushできる
```
git push <repository> <ｓｒｃ>:<dst>
```

一つ前のコミットを origin リポジトリの feature/xxx ブランチにpush
```
git push origin HEAD~:feature/xxxx
```

foo ブランチを origin リポジトリの bar ブランチにpush
```
git push origin foo:bar
```

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
pbcopy < ~/.ssh/(鍵ファイル名).pub
```

GitHubやBitbucketなどの設定ページで上記を貼り付ける

## ホストの設定

~/.ssh/config を下記のように書くと、接続先別に使用する「鍵」と「ユーザー名」を設定できる

```
Host bitbucket.org
  IdentityFile ~/.ssh/bitbucket
  User git

Host github.com
  IdentityFile ~/.ssh/github
  User git

```

- Host - 接続先のSSHのアドレス
- IdentityFile - 鍵のファイルパス
- User - 接続先のUser名

例えば１番目だと `git@bitbucket.org` が接続先で、鍵の場所が `~/.ssh/bitbucket` である


## SSHで繋がるかの確認

### GitHubの場合

```
ssh -T git@github.com
```

### Bitbucketの場合

```
ssh -T git@bitbucket.org
```