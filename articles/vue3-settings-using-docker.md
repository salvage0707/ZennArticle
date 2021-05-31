---
title: "dockerでvue3系環境を作成する" 
emoji: "👇"
type: "tech" 
topics: ["Docker","Vue.js","dockerfile","docker-compose"]
published: true
---
# 手順
1. docker, docker-composeをインストール(他の記事で導入してください)
2. Dockerfileを作成
3. docker-compose.ymlを作成
4. コンテナをbuildし起動する
5. コンテナ内に入り、確認をする
6. vueプロジェクトを作成


## Dockerfileを作成

アプリケーションを作成するディレクトリへ移動する。
カレントディレクトリにvueファイルが展開される。

`Dockerfile`を新規作成し、コンテナの指定などの設定情報を書き込む。

```
$ cd workdir
$ vi Dockerfile
```

```:Docker
FROM node:8.11.3-alpine

WORKDIR /app

RUN apk update && \
    npm install -g npm @vue/cli
```
## docker-compose.ymlを作成
`docker-compose.yml`を新規作成し、コンテナの起動時の設定を書き込む
`vue_app` => 任意のアプリ名
`9000:9000` => ポート指定したい場合

```:docker-compose.yml

version: '3'
services:
  vue_app:
    build: .
    ports:
      - 8080:8080
    volumes:
      - .:/app
    stdin_open: true
    tty: true
    command: /bin/sh
```

## コンテナをbuildし起動する

```
$ docker-compose build
$ docker-compose up -d
```

## コンテナ内に入り、確認をする
コンテナ内部に入りvue.jsのバージョンを確認する
`3.x.x`になっていればOK

```
/app # docker-compose exec vue_app sh
/app # vue --version
3.5.5
```

## vueプロジェクトを作成
プロジェクトを作成する。
`my_app`  =>  任意のプロジェクト名
`npm install`は時間がかかる

```
/app # vue create my_app
/app # cd my_app
/app/my_app # npm install
/app/my_app # npm run serve
```
`localhost:8080`にアクセスしvueの画面が出ればOK

