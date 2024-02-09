# deviseでログイン機能の実装

```html
rails new **devise_sample**
```

**Gemfileに追記**

```html
gem 'devise' # gemfileの一番下に記述
```

```html
bundle install

# deviseの設定ファイルを作成する
bundle exec rails generate devise:install
```

deviseをインストール後のターミナル

```html
Depending on your application's configuration some manual setup may be required:

  1. Ensure you have defined default url options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:

       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

     In production, :host should be set to the actual host of your application.

     * Required for all applications. *

  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root to: "home#index"

     * Not required for API-only Applications *

  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:

       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

     * Not required for API-only Applications *

  4. You can copy Devise views (for customization) to your app by running:

       rails g devise:views

     * Not required *
1は新規登録時にメール送るときの設定なので、ここでは割愛

2はroot to: "home#index"なので、つまりroutes.rbでroot to:になにか定義してね。という意味です。（URLが/のときのルーティング）

3はdeviseはデフォルトでフラッシュメッセージを表示できるので、使うならapp/views/layouts/application.html.erb.にコード書いてねーという意味ですね。
```

- 開発、本番環境の**デフォルトURLの設定**

```html
Rails.application.configure do
  # 以下を追記
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
end
```

```html
Rails.application.configure do
  # 以下を追記
  config.action_mailer.default_url_options = { host: 'www.example.com', port: 80 }
end
```

- **ルートルーティングの設定**

「devise」は、ユーザー登録の完了時やパスワードの変更後にデフォルトのリダイレクト先としてルートに遷移するようになっています。(デフォルトのリダイレクト先を変更しない場合は、ルートのルーティングを設定しておく必要があります。)→つまり、アプリではユーザー登録の完了時やパスワードの変更後のリダイレクト先をログイン画面にしてみるのもあり。例えばログイン先がタスクのindexに接続したい場合はroot to: "tasks#index”とか。

```html
% rails generate controller Statics index

Rails.application.routes.draw do
  # 以下を追記
  root 'statics#index'
end
```

```html
% bundle exec rails g controller Home index
Running via Spring preloader in process 61979
      create  app/controllers/home_controller.rb
       route  get 'home/index'
      invoke  erb
      create    app/views/home
      create    app/views/home/index.html.erb
      invoke  test_unit
      create    test/controllers/home_controller_test.rb
      invoke  helper
      create    app/helpers/home_helper.rb
      invoke    test_unit
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/home.scss
```

- **フラッシュメッセージの表示**

「devise」は、ログイン／ログアウト成功時などにフラッシュメッセージを設定します。必須ではありませんが、それらのフラッシュメッセージの表示を設定しておきます

```
<!DOCTYPE html>
<html>
  <head>
    <title>RailsDevise</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%# 以下を追記 %>
    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>
    <%= yield %>
  </body>
</html>
```

「devise」で使用するモデルを作成(#ログイン機能用のモデルを作成)

、モデルの作成、マイグレーションファイルの作成、ルーティングの追加などが行われます。

```html
% rails generate devise User
Running via Spring preloader in process 57022
      invoke  active_record
      create    db/migrate/20240208080455_devise_create_users.rb
      create    app/models/user.rb
      invoke    test_unit
      create      test/models/user_test.rb
      create      test/fixtures/users.yml
      insert    app/models/user.rb
       route  devise_for :users
```

- メールアドレス
- パスワード
- ログイン回数
- ログイン日時
- パスワードを忘れてしまった際のリカバリこれ以外に必要なカラムがある場合は、自身でカラムを追加する必要があります。
- deviseのデフォルトのテーブルでは、`名前を保存するカラムがありません`

なので、カラムを追加する必要があります。
マイグレーションファイルにt.string :nameを追加

$ bundle exec rails db:migrate





