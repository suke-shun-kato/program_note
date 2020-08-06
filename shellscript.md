# シェルスクリプト

# 行の先頭

## bash使う場合

```
#!/bin/bash
```

## これは普通のシェル？

```
#!/bin/sh
```




# 分岐




## if 文

### 数字

```
if [ "$1" -le 1 ]; then
  echo '1以下'
else
  echo '1より上'
fi
```

### 文字列
```
if [ "$1" = aaa ]; then # 
  echo '変数は aaa や'
fi
```
[ の後、] の前にはスペースが必要

## switch 文

```
  case "$1" in
  up)
    echo 'up の分岐に入った'
    ;;
  stop)
    echo 'stop の分岐に入った'
    ;;
  down)
    echo 'down の分岐に入った'
    ;;
  *)
    echo 'その他（default） の分岐に入った'
    ;;
  esac
```



# サンプルプログラム

```
#!/bin/bash

# VOSとcainzの両方のコンテナを起動する
# 使い方
# ./makeup.sh up (VOSのディレクトリの場所)
# ./makeup.sh stop (VOSのディレクトリの場所)
# ./makeup.sh down (VOSのディレクトリの場所)

docker_compose() {
  case "$1" in
  up)
    docker-compose up -d
    ;;
  stop)
    docker-compose stop
    ;;
  down)
    docker-compose down
    ;;
  *)
    return 1
    ;;
  esac
}


# 引数のエラー処理
ERROR_MSG="第一引数に up stop down のどれかを指定してください"
if [ $# -le 0 ]; then
  echo "$ERROR_MSG"
  exit 1
fi

NETWORK_NAME=vos-shared-network

# 第一引数がない場合はデフォルトで ../vos ディレクトリをVOSのディレクトリの場所とする
if [ $# -le 1 ]; then
  VOS_DIR="$(dirname "$0")/../vos"
else
  VOS_DIR="$2"
fi

# cainz のルートディレクトリを変数に格納
CAINZ_DIR="$(
  cd "$(dirname "$0")" || exit 1
  pwd
)"

###########

# 共有ネットワークの作成
if [ "$1" = up ] && ! docker network ls | grep -q "$NETWORK_NAME"; then
  docker network create "$NETWORK_NAME"
fi

# vos の docker を起動／終了
cd "$VOS_DIR" || exit 1
docker_compose "$1" || {
  echo "$ERROR_MSG"
  exit 1
}

# cainz の docker を起動／終了
cd "$CAINZ_DIR" || exit 1
docker_compose "$1" || {
  echo "$ERROR_MSG"
  exit 1
}

# 共有networkを削除
# docker network inspect --format='{{range .Containers}}{{.Name}}{{end}}' vos-shared-network はネットワークに接続しているコンテナがない場合は空文字列が返る
# if [ "$1" = down ] && [ "$(docker network inspect --format='{{range .Containers}}{{.Name}}{{end}}' vos-shared-network)" = '' ]; then
if [ "$1" = down ] && docker network ls | grep -q "$NETWORK_NAME"; then
  docker network rm "$NETWORK_NAME"
fi

# コンテナ、ネットワークの確認
echo
echo 'docker container ls -a'
docker container ls -a

echo
echo 'docker network ls'
docker network ls

```

