---
title: "Go開発環境構築(Mac)" 
emoji: "🗻"
type: "tech" 
topics: ["golang","Go","環境構築","Mac"]
published: true
---
# Go開発環境構築(Mac)
## 環境情報
||情報|
|---|---|
|OS| macOS High Sierra 10.13.6|
|Homebrew| 2.4.8|
|Go| 1.14.5 darwin/amd64|


## 目次
1. Golangのインストール
1. GOPATHの設定
1. 開発支援ツール

## Golangのインストール
[Go公式のインストーラ](https://xn--go-hh0g6u.com/doc/install) でも出来ますが、Macなのでパッケージ管理のHomebrewを使ってインストールしていきます。

①  インストール & 確認

``` sh
$ brew install go
$ go version
go version go1.14.5 darwin/amd64
```

## GOPATHの設定
外部パッケージのリソースなどが保存されるPATH情報。 実行バイナリもGOPATH内に配置されるため、PATHを通しておく。

① 現在のGOPATHを確認する。標準では`$HOME/go`が設定されている。

```sh
$ go env GOPATH
```
② GOPATHの向き先を再設定する。ターミナルで`export`を実行するだけだと、ターミナルを再起動した場合に設定した値が消えてしまうため、`~/.bashrc`に記載し、起動時に毎回実行されるように設定。

```sh:.bashrc
# 以下を追記する
export GOPATH=$(go env GOPATH)
export PATH=$PATH:$GOPATH/bin
```

③ `.bashrc`を読み込む。（ターミナル再起動でもOK）

```sh
$ source ~/.bashrc
```

## 開発支援用ツール
Goが標準で提供している機能は`go help`で確認可能。
標準での提供はしていないが、[公式に開発されている有用なコマンドラインツール](https://godoc.org/golang.org/x/tools/cmd)があるため、必用に応じてインストールする。

### gofmt
コードフォーマッターのコマンド。インデントや改行位置などを自動調整する。

```go
type Todo struct {
  ID uint64 `json:"id"`
  Title string `json:"title"`
  Finished *bool `json:"finished,default:18"`
  CreatedAt time.Time `json:"created_at"`
}
```

↓

```go
type Todo struct {
  ID        uint64    `json:"id"`
  Title     string    `json:"title"`
  Finished  *bool     `json:"finished,default:18"`
  CreatedAt time.Time `json:"created_at"`
}
```

### goimports
`gofmt`の上位互換のようなツール。コードフォーマッターの機能とプラスしてパッケージを読み込む`import`文の挿入と削除を自動的に行う。

※ Goは使用されていないパッケージがimport宣言されている場合にコンパイルエラーになる

### go vet
バグの原因となりそうなコードを検出するコマンド。

```sh
$ go vet main.go
$ go vet .
$ go vet ./...
```
### golint
Goらしくないコーディングスタイルに対して警告をするコマンド。

```sh
$ golint ./...
$ golint -set_exit_status ./...
```
※ `-set_exit_status`を指定することで、警告が出た場合にコマンドをエラー終了する。CIの実行などはこのオプションを指定するとよい。

### go doc（godoc）
ドキュメント閲覧用のコマンド。`godoc`の方がブラウザで確認できるため、ドキュメントが見やすい。
`go doc`・・・対象のパッケージのドキュメントが標準出力される。
`godoc`・・・GOMODに設定されている`.mod`ファイルをもとに、パッケージのドキュメントが*ブラウザで*確認可能。

```sh
$ go doc -all fmt
```

```sh
$ godoc
```

## 参考
[改訂2版 みんなのGo言語](https://www.amazon.co.jp/dp/B07VPSXF6N/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1)

