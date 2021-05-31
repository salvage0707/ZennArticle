---
title: "GCP環境に合わせてRailsの開発環境を構築する〜Ruby2.7, Rails6.2.3,PostgreSQL12〜" 
emoji: "🐑"
type: "tech" 
topics: ["Ruby","Rails","GAE","PostgreSQL","cloudsql"]
published: true
---
# GCP環境に合わせてRailsの開発環境を構築する

## 目次
1. 想定する本番環境
2. Rubyのインストール
3. PostgreSQLのインストール
4. Railsのインストール

## 想定する本番環境
### Google Cloud Platform
アプリをのせる環境です。ローカル環境に影響しないものは省略。

|項目|サービス|
|---|---|
|Webサーバー| Google App Engin|
|RDB |Google Cloud SQL(Postgres)|

### 構築するローカル環境
今回構築するローカルの環境は、DockerやVagrantなどは使用せず、Mac本体に構築していきます。

|項目 |バージョン| |
|---|---|---|
|Ruby |2.7.2|GAEが対応している最新バージョンが2.7系|
|PostgreSQL|12.6 | CloudSQLのデフォルトバージョンが12系|
|Rails| 6.1.3|Railsの最新バージョン |

## Rubyのインストール
### rbenv
rbenvのインストール方法は省略

### 2.7.2をインストール
```sh
$ rbenv install --list-all | grep 2.7
1.8.6-p287
2.0.0-p247
2.2.7
2.7.0-dev
2.7.0-preview1
2.7.0-preview2
2.7.0-preview3
2.7.0-rc1
2.7.0-rc2
2.7.0
2.7.1
2.7.2
jruby-9.2.7.0
rbx-2.2.7
rbx-2.7
rbx-2.71828182
$ rbenv install 2.7.2
$ rbenv global 2.7.2
$ ruby -v 
ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x86_64-darwin19]
```

## PostgreSQLのインストール
### PostgreSQLをインストール
```sh
$ brew search postgres
==> Formulae
check_postgres        postgresql            postgresql@10         postgresql@11         postgresql@12        postgresql@9.4        postgresql@9.5        postgresql@9.6        postgrest
==> Casks
navicat-for-postgresql                   postgres                                 postgrespreferencepane                   sqlpro-for-postgres                      homebrew/cask-versions/postgres-beta
$ brew install postgresql@12
```

### PostgreSQLを使えるように設定
`@`をつけてバージョン指定したPostgreSQLはそのままだとPATHが通っておらず使用できない。
ローカルの環境にPostgreSQLが正しく設定できていないと、`bundle install`でこけるため、設定する。

```sh
# 設定できていないからpsqlコマンドが使えない
$ psql —version
-bash: psql: command not found
```

brewパスを通すため、`brew info`でインストール情報を確認。

```sh
$ brew info postgresql@12
postgresql@12: stable 12.6 (bottled) [keg-only]
Object-relational database system
https://www.postgresql.org/
/usr/local/Cellar/postgresql@12/12.6_1 (3,225 files, 41.7MB)
  Poured from bottle on 2021-03-10 at 22:47:38
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/postgresql@12.rb
License: PostgreSQL
==> Dependencies
Build: pkg-config ✔
Required: icu4c ✔, krb5 ✔, openssl@1.1 ✔, readline ✔
==> Caveats
This formula has created a default database cluster with:
  initdb --locale=C -E UTF-8 /usr/local/var/postgresql@12
For more details, read:
  https://www.postgresql.org/docs/12/app-initdb.html

postgresql@12 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have postgresql@12 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/postgresql@12/bin:$PATH"' >> /Users/hogeuser/.bash_profile

For compilers to find postgresql@12 you may need to set:
  export LDFLAGS="-L/usr/local/opt/postgresql@12/lib"
  export CPPFLAGS="-I/usr/local/opt/postgresql@12/include"

For pkg-config to find postgresql@12 you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/postgresql@12/lib/pkgconfig"


To have launchd start postgresql@12 now and restart at login:
  brew services start postgresql@12
Or, if you don't want/need a background service you can just run:
  pg_ctl -D /usr/local/var/postgresql@12 start
==> Analytics
install: 14,693 (30 days), 32,591 (90 days), 58,050 (365 days)
install-on-request: 14,392 (30 days), 31,721 (90 days), 56,936 (365 days)
build-error: 0 (30 days)
```

> If you need to have postgresql@12 first in your PATH, run:
>  echo 'export PATH="/usr/local/opt/postgresql@12/bin:$PATH"' >> /Users/hogeuser/.bash_profile

表示されている通り、コマンドを実行する。

```sh
$ echo 'export PATH="/usr/local/opt/postgresql@12/bin:$PATH"' >> /Users/hogeuser/.bash_profile

# .bash_profileの更新を反映（ターミナルの再起動でもOK）
$ source ~/.bash_profile 

# インストールできたか確認
$ psql -V 
psql (PostgreSQL) 12.6
```

### 起動
```sh
$ pg_ctl start -D /usr/local/var/postgresql@12
```

### 接続
```sh
$ psql -d postgres
``` 


## Railsのインストール

```sh
$ gem install -v 6.1.3 rails
```

