---
title: "ページャの実装(kaminari)" 
emoji: "🏂🏾"
type: "tech" 
topics: ["Rails","kaminari"]
published: true
---
# kaminariによる実装

## gemを入れる
```ruby:Gemfile
  gem 'kaminari'
```

その後`$ bundle install`を実行

## ファイルの生成
`$ rails generate kaminari:views default`を実行し、ファイルを生成

## ファイルに書き込む

1. `controller`でインスタンス変数に格納する

```ruby:controller
  def index
      @pages = Page.page(params[:page])
  end
```

2. viewにページネーションを表示

```erb:views
  <%= @pages.each do |f| %>
    .
    .
    .
  <% end %>

  #ここを記述
  <%= paginate @pages, class: "paginate"  %>
```

##　カスタマイズ
### 一覧の表示数の変更
```ruby:config/initializers/kaminari_config.rb
  Kaminari.configure do |config|
      config.default_per_page = 5
  end
```

