---
title: "Djangoã§mysqlã«æ¥ç¶" 
emoji: "ð´ð²"
type: "tech" 
topics: ["Python","MySQL"]
published: true
---
# Djangoã§ãã¼ã¿ãã¼ã¹ã®æ¥ç¶åãMySQLã«å¤æ´(æä½éã®è¨­å®)

çµæ§è©³ããæ¸ãã¾ã

# ç°å¢
`macOS High Sierra` 10.13.6
`Homebrew` 1.7.6 (mysqlã®ã¤ã³ã¹ãã¼ã«ã«ä½¿ç¨)
`mysql`  Ver 8.0.12 for osx10.13 on x86_64 (Homebrew)
`Python` 3.7.0
`Django` 2.0.1
`pip` 18.1

# æé è¡¨
1. `Homebrew`ã§`MySQL`ãã¤ã³ã¹ãã¼ã«
2. MySQLã®ãµã¼ãã¼ãèµ·å
3. ãã¼ã¿ãã¼ã¹ãä½æ
4. 'pip'ã§ããã±ã¼ã¸`PyMySQL`ãã¤ã³ã¹ãã¼ã«
5. `Django`ã¢ããªã¼ã·ã§ã³(ä½æããã¢ããª)ã®`settings.py`ã®ãã¼ã¿ãã¼ã¹æå ±ãå¤æ´
6. `Django`ã¢ããªã¼ã·ã§ã³(ä½æããã¢ããª)ã®`manage.py`ã«è¨è¿°ãè¿½å ï¼ã³ã¼ãã®åå®¹ãããããªãã®ã§ãææ§ï¼
7. `python manage.py migrate`ã§ãã¼ã¿ãã¼ã¹ã«ãã¼ãã«ãè¿½å 
8. `MySQL`ã§ä½æã§ãããç¢ºèª

(è¿½è¨)
models.pyã®èª¬æãå¿ãã¦ããã®ã§ä»ã®è¨äºã®ãªã³ã¯
[Django ãã¤ã°ã¬ã¼ã·ã§ã³ ã¾ã¨ã](https://qiita.com/okoppe8/items/c9f8372d5ac9a9679396)
[ã¯ããã¦ã® Django ã¢ããªä½æããã®2](https://docs.djangoproject.com/ja/2.1/intro/tutorial02/)

## 1. `Homebrew`ã§`MySQL`ãã¤ã³ã¹ãã¼ã«
ã¿ã¼ããã«ã§ã³ãã³ãå®è¡ãã¦ãMySQLã®ã¤ã³ã¹ãã¼ã«ç¢ºèªã¨ãã¼ã¸ã§ã³ç¢ºèª
(ä»¥ä¸ã¿ã¼ããã«ã§ã®ã³ãã³ãå®è¡ã¯$ ã«ã¦è¨è¿°)

```
$ brew install mysql
$ mysql --version
mysql  Ver 8.0.12 for osx10.13 on x86_64 (Homebrew)
```

## 2. MySQLã®ãµã¼ãã¼ãèµ·å

```
$ mysql.server start 
Starting MySQL
. SUCCESS! 
```
ï¼è£è¶³ï¼ãµã¼ãã¼åæ­¢ã¯ä»¥ä¸ã®ã³ãã³ã

```
$ mysql.server stop
Shutting down MySQL
.. SUCCESS!
```

## 3. ãã¼ã¿ãã¼ã¹ãä½æ
ãã¼ã¿ãã¼ã¹ã«æ¥ç¶

```
$ mysql -u root
```
ãã¼ã¿ãã¼ã¹ãä½æ(ãã¼ã¿ãã¼ã¹åã¯ sample)

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

## 4. `pip`ã§ããã±ã¼ã¸`PyMySQL`ãã¤ã³ã¹ãã¼ã«

```
$ pip install pymysql
$ pip freeze -l
PyMySQL==0.9.2
```

## 5. `Django`ã¢ããªã¼ã·ã§ã³(ä½æããã¢ããª)ã®`settings.py`ã®ãã¼ã¿ãã¼ã¹æå ±ãå¤æ´
ã¢ããªã±ã¼ã·ã§ã³ã®'setting.py'ã®è¨è¿°å¤æ´

```python:setting.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'sample', #ãä½æãããã¼ã¿ãã¼ã¹å
        'USER': 'root', # ã­ã°ã¤ã³ã¦ã¼ã¶ã¼å
        'HOST': '',
        'PORT': '',
    }
}
```

å¿è¦ã«å¿ãã¦ãªãã·ã§ã³ãè¿½å 

## 6. `Django`ã¢ããªã¼ã·ã§ã³(ä½æããã¢ããª)ã®`manage.py`ã«è¨è¿°ãè¿½å 
è¿½å ããããã±ã¼ã¸ãä½¿ç¨ããè¨è¿°ãè¿½å 

```python:manage.py
import pymysql
pymysql.install_as_MySQLdb()
```

## 7. `python manage.py migrate`ã§ãã¼ã¿ãã¼ã¹ã«ãã¼ãã«ãè¿½å 
ãã¤ã°ã¬ã¼ã·ã§ã³ãå®è¡(ãã¼ãã«ãä½æ)

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

## 8. `MySQL`ã§ä½æã§ãããç¢ºèª

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

ä½æãç¢ºèªï¼ï¼ï¼ï¼ï¼ï¼ï¼

## çµãã

