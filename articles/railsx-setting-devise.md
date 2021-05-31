---
title: "deviseの使い方" 
emoji: "💁"
type: "tech" 
topics: ["Rails","devise","Gem"]
published: true
---
# deviseの使い方
## 1, インストール方法

`gemfile`に`devise`を記述

`$ bundle install`

## 2,　ファイルの生成

`$ Rails g devise:install` で初期ファイルの生成

## 3, ユーザーモデルの作成

`$ Rails g devise User (モデル名)` でモデルの作成

`$ Rails db:migrate` でマイグレートする

## 4, viewファイルの生成


`$ rails g devise:views` でディレクトリをアプリ内に表示させる

## 5, フォームの作成
`form_for()`の定型引数`<%= form_for(resources, as:resource_name, url: regestration_path(resource_name)) %>`

`url:`のあとは新規登録、ログインなどで使い分ける

# 【主な使い方、知識】

## カラムの追加
* 最初は`:name`カラムがない。migration.fileに記述して、追加を行う

```ruby:db/migrate/YYYYMMDDHHMMSS_devise_create_users.rb
class DeviseCreateUsers < ActiveRecord::Migration[5.1]
  def change
    create_table :users do |t|
      ## Database authenticatable
      t.string :email,              null: false, default: ""
      t.string :encrypted_password, null: false, default: ""
      ## Recoverable
      t.string   :reset_password_token
      t.datetime :reset_password_sent_at
      ## Rememberable
      t.datetime :remember_created_at
      ## Trackable
      t.integer  :sign_in_count, default: 0, null: false
      t.datetime :current_sign_in_at
      t.datetime :last_sign_in_at
      t.string   :current_sign_in_ip
      t.string   :last_sign_in_ip
      ## Confirmable
      # t.string   :confirmation_token
      # t.datetime :confirmed_at
      # t.datetime :confirmation_sent_at
      # t.string   :unconfirmed_email # Only if using reconfirmable
      ## Lockable
      # t.integer  :failed_attempts, default: 0, null: false # Only if lock strategy is :failed_attempts
      # t.string   :unlock_token # Only if unlock strategy is :email or :both
      # t.datetime :locked_at
      
      t.string :name                  #ここを追加
      t.timestamps null: false
    end
    add_index :users, :email,                unique: true
    add_index :users, :reset_password_token, unique: true
    # add_index :users, :confirmation_token,   unique: true
    # add_index :users, :unlock_token,         unique: true
  end
end
```



## deviseモジュールの説明

【デフォルトの機能】

| module | 機能 |
|:--|:--|
| database_authenticatable | DBに保存するパスワードの暗号化(必須) |
| registerable | サインアップ処理 |
| recoverable | パスワードリセット |
| rememberable | クッキーにログイン情報を保持 |
| trackable | サインイン回数・時刻・IPアドレスを保存 |
| validatable | メールアドレスとパスワードのバリデーション |

【コメントアウトにより、追加可能な機能】

| module | 機能 |
|:--|:--|
| confirmable | メール送信による登録確認 |
| lockable | 一定回数ログインに失敗した際のアカウントロック |
| timeoutable | 一定時間でセッションを削除する |
| omniauthable | OmniAuthサポート |


【モジュール詳細】

| 機能 | 概要 |
|:--|:--|
| database_authenticatable | サインイン時にユーザーの正当性を検証するためにパスワードを暗号化してDBに登録します。認証方法としてはPOSTリクエストかHTTP Basic認証が使えます。 |
| registerable | 登録処理を通してユーザーをサインアップします。また、ユーザーに自身のアカウントを編集したり削除することを許可します。 |
| recoverable | パスワードをリセットし、それを通知します。 |
| rememberable | 保存されたcookieから、ユーザーを記憶するためのトークンを生成・削除します。 |
| trackable | サインイン回数や、サインイン時間、IPアドレスを記録します。 |
| validatable | Emailやパスワードのバリデーションを提供します。独自に定義したバリデーションを追加することもできます。 |
| confirmable | メールに記載されているURLをクリックして本登録を完了する、といったよくある登録方式を提供します。また、サインイン中にアカウントが認証済みかどうかを検証します。 |
| lockable | 一定回数サインインを失敗するとアカウントをロックします。ロック解除にはメールによる解除か、一定時間経つと解除するといった方法があります。 |
| timeoutable | 一定時間活動していないアカウントのセッションを破棄します。 |
| omniauthable | intridea/omniauthをサポートします。TwitterやFacebookなどの認証を追加したい場合はこれを使用します。 |

## カラムの追加を行った場合にすること
前提に、カラムの追加(rails db:migrate)を行っている
 `application_controller.rb`のファイルに以下を記述

```ruby:application_controller.rb
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  # 以下を記述
  before_action :configure_permitted_parameters, if: :devise_controller?

   protected

     def configure_permitted_parameters
       #以下の:name部分は追加したカラム名に変える
       devise_parameter_sanitizer.permit(:sign_up, keys: [:name]
     end

end
```

【以下説明】
`before_action :configure_permitted_parameters, if: :devise_controller?`
deviseを利用する機能（ユーザ登録、ログイン認証など）の場合、configure_permitted_parametersを実行します。

````
def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:name])
end
```

configure_permitted_parametersが実行されると、devise_parameter_sanitizer.permitでnameのデータ操作を許可するアクションメソッドを指定します。今回の場合、ユーザ登録(sign_up)の際、ユーザ名(name)のデータ操作が許可されることになります。ストロングパラメーター。



## routesパスの説明
* devise/session#    
    * ログイン機能

* devise/refistrations#
    * 登録機能

## ログインかを確認するメソッド
`user_signed_in?`　ログイン中は`true`を返す

##　ログイン中のユーザーの情報メソッド（session）
`current_user`に格納してある
例:

```ruby:rails.controller
def create
   @post_image = PostImage.new
   @post_image.id = current_user.id
   @post_image.save  
end
```

## 全てのページでログイン認証を儲けるコード
アクセス時に認証を必要とする

```ruby:app/controllers/application_controller.rb
   class ApplicationController < ActionController::Base
      before_action :authenticate_user!
   end
```

ログインしていると表示可能。ログイン認証されていなければrootパスへリダイレクトする

## ログイン時に別のカラムで認証する
`config/initializer/devise.rb`に以下を記述

```ruby
config.authentication_keys = [:name] # :nameを認証可能にしたいカラム名に変更
```

##　ログイン、ログアウト直後のリダイレクト先を変更

`app/controllers/application_controller.rb`に以下を記述

```
# サインイン後のリダイレクト先をマイページへ
	def after_sign_in_path_for(resource)
	  	flash[:notice] = "ログインに成功しました" #　 <-任意で
    	user_path(current_user.id)  #　指定したいパスに変更
  	end

  	# サインアウト後のリダイレクト先をトップページへ
  	def after_sign_out_path_for(resource)
	  	flash[:alert] = "ログアウトしました"
    	top_path
  	end
```

## エラーメッセージの表示方法
`ファイル.html.erb`ないの表示したい場所に`<%= devise_error_messages! %>`を表記する


## 参照元
[deviseまとめ](https://qiita.com/ShinyaKato/items/a098a741a142616a753e)
[deviseまとめ（複雑）](https://qiita.com/cigalecigales/items/f4274088f20832252374)


