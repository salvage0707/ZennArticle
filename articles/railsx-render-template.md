---
title: "部分テンプレート" 
emoji: "🐍"
type: "tech" 
topics: ["Rails"]
published: true
---
# 部分テンプレート
## 使い方
### 1,テンプレートファイルの記述
任意のファイルに`_????.html.erb`ファイルを作成し、コードをかく

### 2,表示を宣言
表示させたい任意のファイルの中に

```
 <%= render '!!!!/????', (変数名): (入れたい変数名,オブジェクト) %>
``` 

## 使用例
* practiceファイルを作成
* practiceファイル内にテンプレートファイル`_pre.html.erb`を作成
* practiceファイル内の`prefile.html.erb`内に`_pre.html.erb`を表示

###
```erb:practice/_pre.html.erb
  <h1> <%= title %> </h>
```

```practice/prefile.html.erb
  <%= render 'practice/pre', title: "ハリネズミカフェ" %>
```

生成されるコードは

```html
<h1> ハリネズミカフェ </h1>
```

