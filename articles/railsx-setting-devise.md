---
title: "deviseã®ä½¿ãæ¹" 
emoji: "ð"
type: "tech" 
topics: ["Rails","devise","Gem"]
published: true
---
# deviseã®ä½¿ãæ¹
## 1, ã¤ã³ã¹ãã¼ã«æ¹æ³

`gemfile`ã«`devise`ãè¨è¿°

`$ bundle install`

## 2,ããã¡ã¤ã«ã®çæ

`$ Rails g devise:install` ã§åæãã¡ã¤ã«ã®çæ

## 3, ã¦ã¼ã¶ã¼ã¢ãã«ã®ä½æ

`$ Rails g devise User (ã¢ãã«å)` ã§ã¢ãã«ã®ä½æ

`$ Rails db:migrate` ã§ãã¤ã°ã¬ã¼ããã

## 4, viewãã¡ã¤ã«ã®çæ


`$ rails g devise:views` ã§ãã£ã¬ã¯ããªãã¢ããªåã«è¡¨ç¤ºããã

## 5, ãã©ã¼ã ã®ä½æ
`form_for()`ã®å®åå¼æ°`<%= form_for(resources, as:resource_name, url: regestration_path(resource_name)) %>`

`url:`ã®ãã¨ã¯æ°è¦ç»é²ãã­ã°ã¤ã³ãªã©ã§ä½¿ãåãã

# ãä¸»ãªä½¿ãæ¹ãç¥è­ã

## ã«ã©ã ã®è¿½å 
* æåã¯`:name`ã«ã©ã ããªããmigration.fileã«è¨è¿°ãã¦ãè¿½å ãè¡ã

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
      
      t.string :name                  #ãããè¿½å 
      t.timestamps null: false
    end
    add_index :users, :email,                unique: true
    add_index :users, :reset_password_token, unique: true
    # add_index :users, :confirmation_token,   unique: true
    # add_index :users, :unlock_token,         unique: true
  end
end
```



## deviseã¢ã¸ã¥ã¼ã«ã®èª¬æ

ãããã©ã«ãã®æ©è½ã

| module | æ©è½ |
|:--|:--|
| database_authenticatable | DBã«ä¿å­ãããã¹ã¯ã¼ãã®æå·å(å¿é ) |
| registerable | ãµã¤ã³ã¢ããå¦ç |
| recoverable | ãã¹ã¯ã¼ããªã»ãã |
| rememberable | ã¯ãã­ã¼ã«ã­ã°ã¤ã³æå ±ãä¿æ |
| trackable | ãµã¤ã³ã¤ã³åæ°ã»æå»ã»IPã¢ãã¬ã¹ãä¿å­ |
| validatable | ã¡ã¼ã«ã¢ãã¬ã¹ã¨ãã¹ã¯ã¼ãã®ããªãã¼ã·ã§ã³ |

ãã³ã¡ã³ãã¢ã¦ãã«ãããè¿½å å¯è½ãªæ©è½ã

| module | æ©è½ |
|:--|:--|
| confirmable | ã¡ã¼ã«éä¿¡ã«ããç»é²ç¢ºèª |
| lockable | ä¸å®åæ°ã­ã°ã¤ã³ã«å¤±æããéã®ã¢ã«ã¦ã³ãã­ãã¯ |
| timeoutable | ä¸å®æéã§ã»ãã·ã§ã³ãåé¤ãã |
| omniauthable | OmniAuthãµãã¼ã |


ãã¢ã¸ã¥ã¼ã«è©³ç´°ã

| æ©è½ | æ¦è¦ |
|:--|:--|
| database_authenticatable | ãµã¤ã³ã¤ã³æã«ã¦ã¼ã¶ã¼ã®æ­£å½æ§ãæ¤è¨¼ããããã«ãã¹ã¯ã¼ããæå·åãã¦DBã«ç»é²ãã¾ããèªè¨¼æ¹æ³ã¨ãã¦ã¯POSTãªã¯ã¨ã¹ããHTTP Basicèªè¨¼ãä½¿ãã¾ãã |
| registerable | ç»é²å¦çãéãã¦ã¦ã¼ã¶ã¼ããµã¤ã³ã¢ãããã¾ããã¾ããã¦ã¼ã¶ã¼ã«èªèº«ã®ã¢ã«ã¦ã³ããç·¨éãããåé¤ãããã¨ãè¨±å¯ãã¾ãã |
| recoverable | ãã¹ã¯ã¼ãããªã»ãããããããéç¥ãã¾ãã |
| rememberable | ä¿å­ãããcookieãããã¦ã¼ã¶ã¼ãè¨æ¶ããããã®ãã¼ã¯ã³ãçæã»åé¤ãã¾ãã |
| trackable | ãµã¤ã³ã¤ã³åæ°ãããµã¤ã³ã¤ã³æéãIPã¢ãã¬ã¹ãè¨é²ãã¾ãã |
| validatable | Emailããã¹ã¯ã¼ãã®ããªãã¼ã·ã§ã³ãæä¾ãã¾ããç¬èªã«å®ç¾©ããããªãã¼ã·ã§ã³ãè¿½å ãããã¨ãã§ãã¾ãã |
| confirmable | ã¡ã¼ã«ã«è¨è¼ããã¦ããURLãã¯ãªãã¯ãã¦æ¬ç»é²ãå®äºãããã¨ãã£ãããããç»é²æ¹å¼ãæä¾ãã¾ããã¾ãããµã¤ã³ã¤ã³ä¸­ã«ã¢ã«ã¦ã³ããèªè¨¼æ¸ã¿ãã©ãããæ¤è¨¼ãã¾ãã |
| lockable | ä¸å®åæ°ãµã¤ã³ã¤ã³ãå¤±æããã¨ã¢ã«ã¦ã³ããã­ãã¯ãã¾ããã­ãã¯è§£é¤ã«ã¯ã¡ã¼ã«ã«ããè§£é¤ããä¸å®æéçµã¤ã¨è§£é¤ããã¨ãã£ãæ¹æ³ãããã¾ãã |
| timeoutable | ä¸å®æéæ´»åãã¦ããªãã¢ã«ã¦ã³ãã®ã»ãã·ã§ã³ãç ´æ£ãã¾ãã |
| omniauthable | intridea/omniauthããµãã¼ããã¾ããTwitterãFacebookãªã©ã®èªè¨¼ãè¿½å ãããå ´åã¯ãããä½¿ç¨ãã¾ãã |

## ã«ã©ã ã®è¿½å ãè¡ã£ãå ´åã«ãããã¨
åæã«ãã«ã©ã ã®è¿½å (rails db:migrate)ãè¡ã£ã¦ãã
 `application_controller.rb`ã®ãã¡ã¤ã«ã«ä»¥ä¸ãè¨è¿°

```ruby:application_controller.rb
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  # ä»¥ä¸ãè¨è¿°
  before_action :configure_permitted_parameters, if: :devise_controller?

   protected

     def configure_permitted_parameters
       #ä»¥ä¸ã®:nameé¨åã¯è¿½å ããã«ã©ã åã«å¤ãã
       devise_parameter_sanitizer.permit(:sign_up, keys: [:name]
     end

end
```

ãä»¥ä¸èª¬æã
`before_action :configure_permitted_parameters, if: :devise_controller?`
deviseãå©ç¨ããæ©è½ï¼ã¦ã¼ã¶ç»é²ãã­ã°ã¤ã³èªè¨¼ãªã©ï¼ã®å ´åãconfigure_permitted_parametersãå®è¡ãã¾ãã

````
def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:name])
end
```

configure_permitted_parametersãå®è¡ãããã¨ãdevise_parameter_sanitizer.permitã§nameã®ãã¼ã¿æä½ãè¨±å¯ããã¢ã¯ã·ã§ã³ã¡ã½ãããæå®ãã¾ããä»åã®å ´åãã¦ã¼ã¶ç»é²(sign_up)ã®éãã¦ã¼ã¶å(name)ã®ãã¼ã¿æä½ãè¨±å¯ããããã¨ã«ãªãã¾ããã¹ãã­ã³ã°ãã©ã¡ã¼ã¿ã¼ã



## routesãã¹ã®èª¬æ
* devise/session#    
    * ã­ã°ã¤ã³æ©è½

* devise/refistrations#
    * ç»é²æ©è½

## ã­ã°ã¤ã³ããç¢ºèªããã¡ã½ãã
`user_signed_in?`ãã­ã°ã¤ã³ä¸­ã¯`true`ãè¿ã

##ãã­ã°ã¤ã³ä¸­ã®ã¦ã¼ã¶ã¼ã®æå ±ã¡ã½ããï¼sessionï¼
`current_user`ã«æ ¼ç´ãã¦ãã
ä¾:

```ruby:rails.controller
def create
   @post_image = PostImage.new
   @post_image.id = current_user.id
   @post_image.save  
end
```

## å¨ã¦ã®ãã¼ã¸ã§ã­ã°ã¤ã³èªè¨¼ãå²ããã³ã¼ã
ã¢ã¯ã»ã¹æã«èªè¨¼ãå¿è¦ã¨ãã

```ruby:app/controllers/application_controller.rb
   class ApplicationController < ActionController::Base
      before_action :authenticate_user!
   end
```

ã­ã°ã¤ã³ãã¦ããã¨è¡¨ç¤ºå¯è½ãã­ã°ã¤ã³èªè¨¼ããã¦ããªããã°rootãã¹ã¸ãªãã¤ã¬ã¯ããã

## ã­ã°ã¤ã³æã«å¥ã®ã«ã©ã ã§èªè¨¼ãã
`config/initializer/devise.rb`ã«ä»¥ä¸ãè¨è¿°

```ruby
config.authentication_keys = [:name] # :nameãèªè¨¼å¯è½ã«ãããã«ã©ã åã«å¤æ´
```

##ãã­ã°ã¤ã³ãã­ã°ã¢ã¦ãç´å¾ã®ãªãã¤ã¬ã¯ãåãå¤æ´

`app/controllers/application_controller.rb`ã«ä»¥ä¸ãè¨è¿°

```
# ãµã¤ã³ã¤ã³å¾ã®ãªãã¤ã¬ã¯ãåããã¤ãã¼ã¸ã¸
	def after_sign_in_path_for(resource)
	  	flash[:notice] = "ã­ã°ã¤ã³ã«æåãã¾ãã" #ã <-ä»»æã§
    	user_path(current_user.id)  #ãæå®ããããã¹ã«å¤æ´
  	end

  	# ãµã¤ã³ã¢ã¦ãå¾ã®ãªãã¤ã¬ã¯ãåãããããã¼ã¸ã¸
  	def after_sign_out_path_for(resource)
	  	flash[:alert] = "ã­ã°ã¢ã¦ããã¾ãã"
    	top_path
  	end
```

## ã¨ã©ã¼ã¡ãã»ã¼ã¸ã®è¡¨ç¤ºæ¹æ³
`ãã¡ã¤ã«.html.erb`ãªãã®è¡¨ç¤ºãããå ´æã«`<%= devise_error_messages! %>`ãè¡¨è¨ãã


## åç§å
[deviseã¾ã¨ã](https://qiita.com/ShinyaKato/items/a098a741a142616a753e)
[deviseã¾ã¨ãï¼è¤éï¼](https://qiita.com/cigalecigales/items/f4274088f20832252374)


