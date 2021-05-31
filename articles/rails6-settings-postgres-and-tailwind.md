---
title: "Rails6の初期設定をする〜PostgreSQL, Tailwind CSS〜" 
emoji: "🔘"
type: "tech" 
topics: ["Ruby","Rails","PostgreSQL","tailwindcss"]
published: true
---
## 環境
開発環境はMacです。
基本的にbrewで環境整えました。

||バージョン|
|---|---|
|Ruby| 2.7.2|
|Rails| 6.1.3|
|PostgreSQL| 12.6|

## 手順

### アプリを作成

```sh
$ rails new appname -M -d postgresql
```

1. `rails new appname`でアプリ名「appname」を指定
2. `-M`でAction Mailerを組み込まないようにする
3. `-d postgresql`で使用するDBサーバー に「PostgreSQL」を指定

**参考**

- [railsコマンドのオプション](https://railsdoc.com/rails)

### 不要なroutesを削除（Action Mailbox）

作成したアプリのroutesを確認すると、すでにいろいろなroutesが設定されています。
今回のアプリで使用しない設定は削除します。

```sh
$ bin/rails routes
                                  Prefix Verb   URI Pattern                                                                                       Controller#Action
           rails_postmark_inbound_emails POST   /rails/action_mailbox/postmark/inbound_emails(.:format)                                           action_mailbox/ingresses/postmark/inbound_emails#create
              rails_relay_inbound_emails POST   /rails/action_mailbox/relay/inbound_emails(.:format)                                              action_mailbox/ingresses/relay/inbound_emails#create
           rails_sendgrid_inbound_emails POST   /rails/action_mailbox/sendgrid/inbound_emails(.:format)                                           action_mailbox/ingresses/sendgrid/inbound_emails#create
     rails_mandrill_inbound_health_check GET    /rails/action_mailbox/mandrill/inbound_emails(.:format)                                           action_mailbox/ingresses/mandrill/inbound_emails#health_check
           rails_mandrill_inbound_emails POST   /rails/action_mailbox/mandrill/inbound_emails(.:format)                                           action_mailbox/ingresses/mandrill/inbound_emails#create
            rails_mailgun_inbound_emails POST   /rails/action_mailbox/mailgun/inbound_emails/mime(.:format)                                       action_mailbox/ingresses/mailgun/inbound_emails#create
          rails_conductor_inbound_emails GET    /rails/conductor/action_mailbox/inbound_emails(.:format)                                          rails/conductor/action_mailbox/inbound_emails#index
                                         POST   /rails/conductor/action_mailbox/inbound_emails(.:format)                                          rails/conductor/action_mailbox/inbound_emails#create
       new_rails_conductor_inbound_email GET    /rails/conductor/action_mailbox/inbound_emails/new(.:format)                                      rails/conductor/action_mailbox/inbound_emails#new
      edit_rails_conductor_inbound_email GET    /rails/conductor/action_mailbox/inbound_emails/:id/edit(.:format)                                 rails/conductor/action_mailbox/inbound_emails#edit
           rails_conductor_inbound_email GET    /rails/conductor/action_mailbox/inbound_emails/:id(.:format)                                      rails/conductor/action_mailbox/inbound_emails#show
                                         PATCH  /rails/conductor/action_mailbox/inbound_emails/:id(.:format)                                      rails/conductor/action_mailbox/inbound_emails#update
                                         PUT    /rails/conductor/action_mailbox/inbound_emails/:id(.:format)                                      rails/conductor/action_mailbox/inbound_emails#update
                                         DELETE /rails/conductor/action_mailbox/inbound_emails/:id(.:format)                                      rails/conductor/action_mailbox/inbound_emails#destroy
new_rails_conductor_inbound_email_source GET    /rails/conductor/action_mailbox/inbound_emails/sources/new(.:format)                              rails/conductor/action_mailbox/inbound_emails/sources#new
   rails_conductor_inbound_email_sources POST   /rails/conductor/action_mailbox/inbound_emails/sources(.:format)                                  rails/conductor/action_mailbox/inbound_emails/sources#create
   rails_conductor_inbound_email_reroute POST   /rails/conductor/action_mailbox/:inbound_email_id/reroute(.:format)                               rails/conductor/action_mailbox/reroutes#create
                      rails_service_blob GET    /rails/active_storage/blobs/redirect/:signed_id/*filename(.:format)                               active_storage/blobs/redirect#show
                rails_service_blob_proxy GET    /rails/active_storage/blobs/proxy/:signed_id/*filename(.:format)                                  active_storage/blobs/proxy#show
                                         GET    /rails/active_storage/blobs/:signed_id/*filename(.:format)                                        active_storage/blobs/redirect#show
               rails_blob_representation GET    /rails/active_storage/representations/redirect/:signed_blob_id/:variation_key/*filename(.:format) active_storage/representations/redirect#show
         rails_blob_representation_proxy GET    /rails/active_storage/representations/proxy/:signed_blob_id/:variation_key/*filename(.:format)    active_storage/representations/proxy#show
                                         GET    /rails/active_storage/representations/:signed_blob_id/:variation_key/*filename(.:format)          active_storage/representations/redirect#show
                      rails_disk_service GET    /rails/active_storage/disk/:encoded_key/*filename(.:format)                                       active_storage/disk#show
               update_rails_disk_service PUT    /rails/active_storage/disk/:encoded_token(.:format)                                               active_storage/disk#update
                    rails_direct_uploads POST   /rails/active_storage/direct_uploads(.:format)                                                    active_storage/direct_uploads#create
```

routesのパスを見たところ、`Active Mailbox`と`Active Storage`の設定が入っているようです。
今回は`Action Mail`のみ削除します。
※ `Active Storage`は実際に作成しているアプリはGoogle Cloud Storageを使用する予定なので残しておきます

設定を削除

```ruby:config/application.rb
require "rails"
# Pick the frameworks you want:
require "active_model/railtie"
require "active_job/railtie"
require "active_record/railtie"
require "active_storage/engine"
require "action_controller/railtie"
# require "action_mailer/railtie" 
# ↓Action Mailboxの設定をコメントアウト↓ 
# require "action_mailbox/engine"
require "action_text/engine"
require "action_view/railtie"
require "action_cable/engine"
require "sprockets/railtie"
require "rails/test_unit/railtie"
```

routesの確認

```sh
$ bin/rails routes
                         Prefix Verb URI Pattern                                                                                       Controller#Action
             rails_service_blob GET  /rails/active_storage/blobs/redirect/:signed_id/*filename(.:format)                               active_storage/blobs/redirect#show
       rails_service_blob_proxy GET  /rails/active_storage/blobs/proxy/:signed_id/*filename(.:format)                                  active_storage/blobs/proxy#show
                                GET  /rails/active_storage/blobs/:signed_id/*filename(.:format)                                        active_storage/blobs/redirect#show
      rails_blob_representation GET  /rails/active_storage/representations/redirect/:signed_blob_id/:variation_key/*filename(.:format) active_storage/representations/redirect#show
rails_blob_representation_proxy GET  /rails/active_storage/representations/proxy/:signed_blob_id/:variation_key/*filename(.:format)    active_storage/representations/proxy#show
                                GET  /rails/active_storage/representations/:signed_blob_id/:variation_key/*filename(.:format)          active_storage/representations/redirect#show
             rails_disk_service GET  /rails/active_storage/disk/:encoded_key/*filename(.:format)                                       active_storage/disk#show
      update_rails_disk_service PUT  /rails/active_storage/disk/:encoded_token(.:format)                                               active_storage/disk#update
           rails_direct_uploads POST /rails/active_storage/direct_uploads(.:format)                                                    active_storage/direct_uploads#create
```

**参考**

- [Action Mailbox](https://railsguides.jp/action_mailbox_basics.html)
- [Action Mailboxの設定削除方法](https://blog.naoshihoshi.com/entry/2020/03/20/123000)

**補足**
ちなみに`Active Storage`の設定削除は[rails newオプションでできるそうです](https://reasonable-code.com/active-storage-disable/)。


### PostgreSQLの接続設定

[PostgreSQLのインストール](https://qiita.com/salvage0707/items/2640353331042e405559)と
`config/detabase.yml`の設定をしてください。

### Tailwind CSSの設定

#### Tailwind CSSのgemを追加
Gemfileに追記

```ruby:Gemfile
gem 'tailwindcss-rails'
```

インストール

```sh
$ bin/bundle install
$ bin/rails tailwindcss:install
```

**参考**

- [tailwindcss-railsのGithub](https://github.com/rails/tailwindcss-rails#installation)

