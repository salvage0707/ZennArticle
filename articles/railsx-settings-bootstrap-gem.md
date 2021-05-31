---
title: "bootstrapの導入(gem)" 
emoji: "🙌🏽"
type: "tech" 
topics: ["Rails","bootstrap"]
published: true
---
# bootstrap
## 1,gemに記述する
```ruby:Gemfile
  gem 'bootstrap-sass'
  gem 'jquery-rails'
```

```sh
$ bundle inatall
```

## 2,ファイル名(拡張子)を変更する

 ```sh
$ cd app/assets/stylesheets
$ rename application.css application.scss
```

*直接の変更でも可能

## 3,二つのファイルに記述を追加

```scss:app/assets/stylesheets/application.scss
*= require_tree .
    *= require_self
    */
    @import "bootstrap-sprockets";  /*ここを追加*/
    @import "bootstrap";            /*ここを追加*/

```

```js:app/assets/javascripts/application.js
//= require rails-ujs
  //= require turbolinks
  //= require jquery
  //= require bootstrap-sprockets
  //= require_tree .
```


##　リンク
[Bootstrap日本語リファレンス](http://bootstrap3.cyberlab.info/)



