---
title: "テストケースの作成" 
emoji: "🙅🏽"
type: "idea" 
topics: ["Java","JUnit","testing"]
published: true
---
# 目次
- よくある失敗
- 不必要なテストパターンが多すぎる
- 異常系テストが足りていない
- テストケースの考え方
- 正常系と異常系
- 同値分割
- 境界値分析
- エラーの推測
- テストケースの粒度
- よいテストケースとは

##　よくある失敗

1. 不必要なテストパターンが多すぎる

「全ての組み合わせを網羅しなくては」と考えてしまうあまり、意味のないテストケースをたくさん作り込んでしまうタイプです。先ほどの例で示したように、組み合わせが多くなるとあっという間に天文学的なパターン数になってしまいます。

2. 異常系テストが足りていない

　「数値の項目にカタカナを入力したら」、「データベースに接続出来なくなったら」のような、異常なパターンのテストがそもそも足りていないタイプです。実際のシステム運用では、想定していない事態は頻繁に起こります。異常系テストが足りていないと、そのような時にすぐ壊れる脆弱なアプリケーションになってしまいます。

## テストケース4つの考え方

1. 正常系と異常系
　正常系とは、「その対象が想定している入力に対して期待どおりの出力を行なうかどうか」という考え方です。
　対して異常系は、「その対象が想定していない入力に対してきちんと対処できるかどうか」という考え方です。

2. 同値分割
　同値分割とは、「入力を意味のあるグループ（同値クラス）に分け、各グループから代表値を選ぶ方法」です。 同じような意味を持つ入力値によるテストを避けることができたり、意味のある入力値によるテストを見逃さないというメリットがあります。
　
　例として、以下のような仕様を持ったアプリケーションがあるとします。
　

・ユーザの年齢をテキストボックスに入力する。
・年齢は0～200までの数値が入力可能である。
・入力後、「チェック」ボタンを押す。
・入力された値に応じて、異なるメッセージボックスが画面上に表示される。

```
0～19が入力された場合：「未成年です」というメッセージボックス
20～99が入力された場合：「成人しています」というメッセージボックス
100～200が入力された場合：「とても長生きですね」というメッセージボックス
```

このアプリケーションにおいて、入力可能な数値の種類はたくさんありますが、実はそのグループは3つしかないことが分かります。同値分割では、これら各グループの中からそれぞれ10、80、150のように代表値を選び、テストを実施します

3. 境界値分析
　境界値分析とは、「同値クラス間の境界の値を入力とする方法」です。 メソッドの実装ミスは、一般的に境界値で生じやすいため、境界値分析を行なうことでミスを防ぐことができます。
　先ほどのアプリケーションですと、19と20、99と100がその境界値にあたります。
仕様書の「以下」と「未満」を取り違えたり、プログラムのif文中で不等号「＜」と「≦」を誤るなどして混入したバグは、この手法を用いて検出することが出来ます。

4. エラーの推測
　エラー推測は、エラーが発生しそうなデータパターンを推測し、テストケースを作成する手法です。このようなデータパターンというのは、ある程度形式知化されています。具体的には、以下のようなものがよく用いられます。

1. 最大値・最小値、最大値より大きい値・最小値より小さい値
　言語やアプリケーションの仕様によって、入力可能な数値や文字長の最大値・最小値は決まっています。
　その値を超えた場合に、どのような動作となるかを検証します。
※このパターンは、エラー推測ではなく境界値分析に分類されることもあります。

2. 小数
　浮動小数点数のように、桁数が大きなデータを扱うと丸め誤差が生じてしまうものをテストします。

3. 空文字・スペース・ゼロ・NULL
　プログラムは「データが存在しない場合」や「NULLを参照した場合」に誤動作が発生しやすくなります。

4. 入力されることを意図していない文字種
　数値の項目に漢字を入力する、氏名の項目に記号を入力するなど、本来は入力されるべきでない文字種に対しバリデーションが機能していることを検証します。

5. うるう年、存在しない日付・時刻
　日付の項目にうるう年を入力し、正しく扱えることを確認します。また、「2015/14/12」、「26:00:00」のように存在しない日付・時刻を入力してみることもあります。

　
　以上の観点を踏まえて、次のような手順でテストケースを洗い出します。

　
1. 正常系はどういうケースが考えられるか
2. 異常系はどういうケースが考えられるか
3. 1-2について、同値分割するとどのような入力値が考えられるか
4. 1-2について、境界値分析するとどのような入力値が考えられるか
5. 1-4を踏まえ、サービスが要求する品質と工数を考慮してテストケースとする
　
　テストケースを洗い出す際は、正常系よりも異常系をしっかりと考えることで、安定したアプリケーションを構築することができます。

3. テストケースの粒度
　前章で「サービスが要求する品質と工数を考慮して……」と示しました。

　たとえば、開発中のサービスが顧客から費用をもらう性質のプロダクトの場合、可能な限りテストケースを網羅すべきでしょう。 決済機能といったクリティカルな部分であれば、なおさらです。

　この場合、ドメイン/アプリケーション層だけでなく、インタフェース層（Viewなど）までも十分にテストを行なうことも検討した方がよいかもしれません。

　一方で、広告だけで成り立ち、かつ機密情報を扱わないtoCサービスの場合、テストの実装に時間を割くよりも、ユーザのために機能開発にコストをかけた方がよいかもしれません。
　
　テストは書こうと思えばいくらでも書けますが、その粒度はサービスの性質に応じて柔軟に決めます。

