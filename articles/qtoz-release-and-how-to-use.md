---
title: "QiitaからZennに移行するアプリを作成しました" 
emoji: "🔯"
type: "tech" 
topics: ["Rails","gcp","個人開発","Zenn"]
published: true
---
## アプリ概要
Qiitaに投稿した記事をZennに移行するためのアプリを作成しました！
移行を検討している方、何となく使ってみたい方使ってみてください！！！！！

※ 移行後もQiitaに投稿されていた記事は削除されません。


## アプリのURL
「QtoZ（キュートゥーゼン）」ぜひ使ってみてください！
今後少しずつ機能追加や修正を行います。フィードバックなどはこちらの記事のコメントしていただけると嬉しいです！

https://qtoz.salv.dev/

※ App Engineを使っているため、初期レスポンスに時間がかかる場合があります m(__)m

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/76b81f76-45fb-1c4b-0f1d-637455c3185c.png)





## はじめに

前からすごくお世話になっているQiitaですが、最近は非常に使いにくく感じています。
この前行われたホーム画面の変更。パーソナライズされたおすすめ記事の一覧は良いのですが、以前から技術のキャッチアップで使用していた、トレンド一覧がなくなってしまい少し残念。
また、おすすめ記事もあまり興味のないものが一覧で表示されるため、日に日にQiitaを見なくなっています。

そんな中で去年リリースされた[Zenn](https://zenn.dev/)!!!!

- UI奇麗
- ローカルで記事作成できる
- GIthubで記事管理できる
- 本の作成ができる
- 決済機能がある

かなりいい！！！！
他にも良い機能があり、追加機能のリリースも短期間で行われているため、サービスとして気になりました。

あれ？これ、Zennに移行しちゃう？ってな感じで、移行することにしました。




## なぜ作ったのか

QiitaからZennに移行するためには、markdownファイルの先頭に、投稿に必要な情報をyaml形式で追記しないといけません。
Qiitaの記事は画面からコピーしたり、Qiita APIで簡単に記事を持って来れますが、形式の変更が結構めんどくさいです。


正直、自分だけ移行するのであれば、アプリ作るより手作業の方が早いのですが、zennに移行したい方がいるのではないか？ということでアプリを作成しました。



## 使い方

### 1. ログイン
右上のログインボタンからログインできます。
Qiita APIを使用した、OAuth認証を使用しているので、Qiitaアカウントがあればログインできます。


### 2. Qiitaから自分の記事をインポートする
ヘッダーの「インポート」から記事のインポートが可能です。
インポート時、各記事にランダムで絵文字を設定したい場合は、絵文字入力欄に入力せずインポートを実行してください。

絵文字入力欄に入力された場合、全記事に入力された絵文字が設定されます。
（今考えると、全ての記事に同じ絵文字を設定する人はあまりいないと思うので、いらなかったかな。。。）

記事のインポートは何回でも行えます。最初からやり直した場合は、再度本手順から行ってください。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/c460c5fc-0a54-be2d-f590-4b08c62d8cd8.png)


### 3. 記事個別の設定
ヘッダーの「記事設定」から記事の設定確認や編集などの操作が可能です。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/9d5b5251-d956-cdde-4cbf-db7f923f9c67.png)



#### Qiitaで対象記事を確認する場合
「ACTIONの目のアイコン」をクリックすることで、別タブが開き、Qiitaサービス上で対象記事が表示されます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/cd677457-5a8f-0295-1392-af11cc094cdf.png)


#### 編集する場合
「ACTIONの鉛筆アイコン」をクリックすることで、記事の編集ができます。
編集可能な項目は以下です。

- slag（記事を一意にする値）
- タイトル
- 絵文字
- 記事タイプ（tech/idea）
- topics（タグ）
- published（公開設定）

※ 絵文字を未入力にして更新すると、ランダムな絵文字が設定されます。

**編集画面の表示**
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/914de47a-b6b4-ec49-c3c7-507cb5558ba0.png)

**編集画面**

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/88d25ebe-88cf-be81-c4fc-51c1e4daba4e.png)




#### 削除する場合
「ACTIONのゴミ箱アイコン」をクリックすることで、移行対象から削除することができます。
**※ Qiita上からは削除されませんのでご安心ください！！！**

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/a441e347-4ccf-54f9-3486-e02e7cef7a22.png)



### 4. 記事のダウンロード
ヘッダーの「ダウンロード」から、設定した記事をダウンロードできます。
形式はZipなので、ダウンロード後に解凍してください。

「記事をZipにする」を押したら、ファイル作成処理が開始されます。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/4b01a9a6-cb37-8db4-b7de-f30cc7a9d181.png)


実行後、数分で処理が完了するので画面をリロードしてください。
ステータスが「完了」になっている行の「ダウンロードアイコン」を押していただくと、Zipファイルがダウンロードできます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/224082/fe952c5b-c01f-a415-1ca4-5c86dc27e79f.png)


### 5. Zennにアップロード
ZennとGithubの連携を行うことでアップロードできます。
詳細Zenn公式の「[GitHubリポジトリでZennのコンテンツを管理する](https://zenn.dev/zenn/articles/connect-to-github)」をご確認ください。


## 注意点
Qiitaでは正しく表示されていたmarkdown記法でも、Zennでは表示されないケースがありますので、[Zenn CLIを使用したローカルプレビュー](https://zenn.dev/zenn/articles/zenn-cli-guide#%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E3%81%99%E3%82%8B)などの方法で、表示確認することをお勧めします。



