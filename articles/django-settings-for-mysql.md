---
title: "Djangoでmysqlに接続" 
emoji: "🇴🇲"
type: "tech" 
topics: ["Python","MySQL"]
published: true
---
# Djangoでデータベースの接続先をMySQLに変更(最低限の設定)

結構詳しく書きます

# 環境
`macOS High Sierra` 10.13.6
`Homebrew` 1.7.6 (mysqlのインストールに使用)
`mysql`  Ver 8.0.12 for osx10.13 on x86_64 (Homebrew)
`Python` 3.7.0
`Django` 2.0.1
`pip` 18.1

# 手順表
1. `Homebrew`で`MySQL`をインストール
2. MySQLのサーバーを起動
3. データベースを作成
4. 'pip'でパッケージ`PyMySQL`をインストール
5. `Django`アプリーション(作成したアプリ)の`settings.py`のデータベース情報を変更
6. `Django`アプリーション(作成したアプリ)の`manage.py`に記述を追加（コードの内容がわからないので、曖昧）
7. `python manage.py migrate`でデータベースにテーブルを追加
8. `MySQL`で作成できたか確認

(追記)
models.pyの説明を忘れていたので他の記事のリンク
[Django マイグレーション まとめ](https://qiita.com/okoppe8/items/c9f8372d5ac9a9679396)
[はじめての Django アプリ作成、その2](https://docs.djangoproject.com/ja/2.1/intro/tutorial02/)

## 1. `Homebrew`で`MySQL`をインストール
ターミナルでコマンド実行して、MySQLのインストール確認とバージョン確認
(以下ターミナルでのコマンド実行は$ にて記述)

```
$ brew install mysql
$ mysql --version
mysql  Ver 8.0.12 for osx10.13 on x86_64 (Homebrew)
```

## 2. MySQLのサーバーを起動

```
$ mysql.server start 
Starting MySQL
. SUCCESS! 
```
（補足）サーバー停止は以下のコマンド

```
$ mysql.server stop
Shutting down MySQL
.. SUCCESS!
```

## 3. データベースを作成
データベースに接続

```
$ mysql -u root
```
データベースを作成(データベース名は sample)

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+

mysql> create database sample;
Query OK, 1 row affected (0.06 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sample             |
| sys                |
+--------------------+
```

## 4. `pip`でパッケージ`PyMySQL`をインストール

```
$ pip install pymysql
$ pip freeze -l
PyMySQL==0.9.2
```

## 5. `Django`アプリーション(作成したアプリ)の`settings.py`のデータベース情報を変更
アプリケーションの'setting.py'の記述変更

```python:setting.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'sample', #　作成したデータベース名
        'USER': 'root', # ログインユーザー名
        'HOST': '',
        'PORT': '',
    }
}
```

必要に応じてオプションを追加

## 6. `Django`アプリーション(作成したアプリ)の`manage.py`に記述を追加
追加したパッケージを使用する記述を追加

```python:manage.py
import pymysql
pymysql.install_as_MySQLdb()
```

## 7. `python manage.py migrate`でデータベースにテーブルを追加
マイグレーションを実行(テーブルを作成)

```
$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
/Users/masaki/.virtualenvs/env1/lib/python3.7/site-packages/pymysql/cursors.py:170: Warning: (3719, "'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.")
  result = self._query(query)
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying sessions.0001_initial... OK
```

## 8. `MySQL`で作成できたか確認

```
$ mysql -u root
mysql> use sample
Database changed
mysql> show tables;
+----------------------------+
| Tables_in_sample           |
+----------------------------+
| auth_group                 |
| auth_group_permissions     |
| auth_permission            |
| auth_user                  |
| auth_user_groups           |
| auth_user_user_permissions |
| django_admin_log           |
| django_content_type        |
| django_migrations          |
| django_session             |
+----------------------------+
```

作成を確認！！！！！！！

## 終わり

