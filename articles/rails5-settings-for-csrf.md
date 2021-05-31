---
title: "Rails 5.2系 protect_from_forgery設定(csrfを対策を解除する)" 
emoji: "👊🏼"
type: "tech" 
topics: ["Ruby","Rails","csrf"]
published: true
---
# csrfを対策を解除する

5.2以前は`protect_from_forgery `が`application_controller.rb`に書いてあり、コメントアウトで解消していたが、5.2以降は以下の記述を追加することで、対応

~~~ ruby:application.rb
module TodoSample
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    config.load_defaults 5.2

    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration can go into files in config/initializers
    # -- all .rb files in that directory are automatically loaded after loading
    # the framework and any gems in your application.

    # 以下の行を追加
    + config.action_controller.default_protect_from_forgery = false 
  end
end
~~~

