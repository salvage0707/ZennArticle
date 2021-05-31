---
title: "Djangoで開発開始を少しでも楽に！！！" 
emoji: "❣"
type: "idea" 
topics: ["Python","ShellScript","Django"]
published: true
---
# PythonのDjangoの作業開始、終了を少しでも楽に！
注意
django
mysql
python
Virtualenvwrapper
を使用している状態でのスクリプトです

## コマンドの説明
django　環境の構築（？）を簡単にしてくれる
workon　と mysqlのサーバー立ち上げ、終了をコマンド一つで行う

## ~/.bash_profile に関数を追加

```shell:.bash_profile
function workpy() {
    # 変数に初期値を設定
    env_name=${2:-env1}
    work=${1:-none}
    # 引数を表示
    echo work status : $work
    echo workon name : $env_name

    if [ $work = start ]; then
        workon $env_name
        mysql.server start

    elif [ $work = end ]; then
        deactivate
        mysql.server stop

    else
        echo "workpy [start or end] env_name"
    
    fi
}
```

作成後に下記のコマンドを実行

```
$ source ~/.bash_profile
```


##　作成したコマンドの使い方
作業開始

```
$ workpy start [env_name]
```
[env_name]は省略可能（デフォルトでenv1を使用）

作業終了

```
$ workpy end
```



