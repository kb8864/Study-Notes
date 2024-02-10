# deviseでログイン機能の実装

```
$ rails new sample_app
$ cd sample_app
$ rails generate scaffold book title:string memo:text
$ rails db:migrate
```

# deviseのセットアップ

**Gemfileに追記**

```
gem 'devise' # gemfileの一番下に記述
```

```
bundle install
```

# deviseの設定ファイルを作成する
```
bundle exec rails generate devise:install
```

deviseをインストール後のターミナル

```
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

  3. Ensure you have flash messages in app/views/layouts/application..erb.
     For example:

       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

     * Not required for API-only Applications *

  4. You can copy Devise views (for customization) to your app by running:

       rails g devise:views

     * Not required *
1は新規登録時にメール送るときの設定なので、ここでは割愛

2はroot to: "home#index"なので、つまりroutes.rbでroot to:になにか定義してね。という意味です。（URLが/のときのルーティング）

3はdeviseはデフォルトでフラッシュメッセージを表示できるので、使うならapp/views/layouts/application..erb.にコード書いてねーという意味ですね。
```

- 開発、本番環境の**デフォルトURLの設定**

```
Rails.application.configure do
  # 以下を追記
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
end
```

```
Rails.application.configure do
  # 以下を追記
  config.action_mailer.default_url_options = { host: 'www.example.com', port: 80 }
end
```

- **ルートルーティングの設定**

「devise」は、ユーザー登録の完了時やパスワードの変更後にデフォルトのリダイレクト先としてルートに遷移するようになっています。(デフォルトのリダイレクト先を変更しない場合は、ルートのルーティングを設定しておく必要があります。)→つまり、アプリではユーザー登録の完了時やパスワードの変更後のリダイレクト先をログイン画面にしてみるのもあり。例えばログイン先がタスクのindexに接続したい場合はroot to: "tasks#index”とか。

```
Rails.application.routes.draw do

  resources :books
    root to:'books#index'
end
```
deviseはサインインやパスワード変更などを行った後のデフォルトのリダイレクト先にルート(/)を使用します。

上記の設定により、リダイレクト先が/から/booksに変更



- **フラッシュメッセージの表示**

「devise」は、ログイン／ログアウト成功時などにフラッシュメッセージを設定します。必須ではありませんが、それらのフラッシュメッセージの表示を設定しておきます
app/views/layouts/application.html.erb
```
 <body>
  <% if notice.present? %>
    <p id="notice"><%= notice %><p>
  <% end %>

  <% if alert.present? %>
    <p id="alert"><%= alert %></p>
  <% end %>
  <%= yield %>
  </body>

```
app/views/books/index.html.erbとapp/views/books/show.html.erbの以下の行を削除します。
```
<p id="notice"><%= notice %></p>
```



「devise」で使用するモデルを作成(#ログイン機能用のモデルを作成)

# モデルの作成、マイグレーションファイルの作成、ルーティングの追加などが行われます。
ユーザーリソースへアクセスするためのルーティングも生成されます

```
% bundle exec rails generate devise User
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
bundle exec rails db:migrateを実行して、マイグレーションを実行し、usersテーブルをセットアップします。これにより、認証に関するすべての操作で利用可能なUserモデルが作成


# ログインしていない時に登録した内容を確認できないようにする
各アクションが実行される前にユーザー認証を行うように設定
before_action :authenticate_user!: これはRailsのコールバックの一種で、コントローラ内のどのアクションが実行される前にも、指定されたメソッド（この場合は:authenticate_user!）を実行するように設定します。:authenticate_user!はDevise gemによって提供されるヘルパーメソッドで、ユーザーがログインしていない場合はログインページにリダイレクトさせる機能を持ちます

app/controllers/application_controller.rb
```
class ApplicationController < ActionController::Base
  before_action :authenticate_user!
end
```

# アカウント編集とログアウトのリンクを追加
アプリケーションの全てのビューに共通のHTML構造を提供するapp/views/layouts/application.html.erbに以下の内容を追加
ユーザーがログインしているかどうかに応じて、異なるナビゲーションオプションを提供するために使われる。ログインしているユーザーには、プロフィールの編集やログアウトのオプションが表示される。また、アプリケーション全体で通知やアラートメッセージを表示することができる。

```
  <% if user_signed_in? %>
    <div class="menu-container">
      <div class="title">Logged in as <%= current_user.email %></div>
      <ul>
        <li>
          <%= link_to 'Edit profile', edit_user_registration_path %>
        </li>
        <li>
          <%= link_to 'Logout', destroy_user_session_path, method: :delete %>
        </li>
      </ul>
    </div>
  <% end %>
```
<details>
  <summary>説明</summary>
  
```
<% if user_signed_in? %>: Deviseのヘルパーメソッドuser_signed_in?を使用して、ユーザーがログインしているかどうかを確認。この条件がtrueの場合、ログインしているユーザー向けのメニューを表示。

<%= current_user.email %>: ログインしているユーザーのメールアドレスを表示します。current_userはDeviseが提供する現在のログインユーザーを参照するためのヘルパーメソッド。

<%= link_to 'Edit profile', edit_user_registration_path %>: ユーザーが自分のプロフィールを編集するためのリンクを生成。

<%= link_to 'Logout', destroy_user_session_path, method: :delete %>: ログアウトのリンクを生成。method: :deleteオプションは、リンクがクリックされたときにHTTP DELETEリクエストを発行するようにRailsに指示します。これはDeviseのログアウト機能がDELETEリクエストを使用するため。

```

</details>

# ユーザー一覧画面とユーザー詳細画面の作成
、Userモデルにnameとself_introductionカラムを追加
```
$ rails generate migration add_name_and_self_introduction_to_users name:string self_introduction:text
$ rails db:migrate

```

config/routes.rbに以下のルーティングを追
resources :users, only: %i(index show)

# usersコントローラーを作成
```
bundle exec rails generate controller users index show --skip-routes --no-helper --no-assets --no-test-framework
```
app/controllers/users_controller.rb
```
class UsersController < ApplicationController
  def index
    @users = User.all
  end

  def show
    @users = User.find(params[:id])
  end

  # private

end

```

# ユーザ一ー覧画面とユーザー詳細画面を編集
app/views/users/index.html.erb
```
<h1>ユーザー</h1>

<table>
  <thead>
    <tr>
      <th>Eメール</th>
      <th>氏名</th>
      <th></th>
    </tr>
  </thead>

  <tbody>
    <% @users.each do |user| %>
      <tr>
        <td><%= user.email %></td>
        <td><%= user.name %></td>
        <td><%= link_to '詳細', user %></td>
      </tr>
    <% end %>
  </tbody>
</table>
```

app/views/users/show.html.erb
```
<h1>ユーザーの詳細</h1>

<p>
  <strong>Eメール:</strong>
  <%= @user.email %>
</p>

<p>
  <strong>氏名:</strong>
  <%= @user.name %>
</p>

<p>
  <strong>自己紹介文:</strong>
  <%= @user.self_introduction %>
</p>

<% if current_user == @user %>
  <%= link_to '編集', edit_user_registration_path %> |
<% end %>
<%= link_to '戻る', users_path %>
```

# app/views/layouts/application.html.erbにユーザー一覧画面へのリンクを追加
<%= link_to 'ユーザー', users_path %>

```
  <body>
    <% if user_signed_in? %>
    <div class="menu-container">
      <div class="title">メニュー</div>
      <ul>
        <li><%= link_to '本のページ', books_path %></li>
        <li><%= link_to 'ユーザー', users_path%></li>
      </ul>
      <div class="title">Logged in as <%= current_user.email %></div>

      <ul>
        <li><%= link_to 'Edit profile', edit_user_registration_path %></li>
        <li>
          <%= link_to 'Logout', destroy_user_session_path, method: :delete %>
        </li>
      </ul>
    </div>
    <% end %> <% if notice.present? %>
    <p id="notice"><%= notice %></p>
    <% end %> <% if alert.present? %>
    <p id="alert"><%= alert %></p>
    <% end %> <%= yield %>
  </body>

```

# ここで「devise」の日本語化を行う
「devise」で作成されたフォームやフラッシュメッセージを日本語化するには、config/ディレクトリ配下のapplication.rbに以下を追記
```
  class Application < Rails::Application
    # 以下を追記
    config.i18n.default_locale = :ja
  end
```
## Gemfileに以下を追記しbundle installを実行
gem 'devise-i18n'
ここまでの画面

https://github.com/kb8864/Study-Notes/assets/128299525/64132bbd-0b04-4cc7-b96f-94b2905a0436


# サインアップ画面とアカウント編集画面の編集するため、
以下のコマンドを実行して、devise:i18n:viewsを生成
devise-i18n/app/views/devise/registrations/edit.html.erbのedit.html.erbとnew.html.erbが生成されます
```
bundle exec rails g devise:i18n:views -v registrations
      create    app/views/devise/registrations
      create    app/views/devise/registrations/edit.html.erb
      create    app/views/devise/registrations/new.html.erb
```

# app/views/devise/registrations/_profile_fields.html.erbを手動作成
```<div class="field">
  <%= f.label :name %>
  <%= f.text_field :name %>
</div>

<div class="field">
  <%= f.label :self_introduction %>
  <%= f.text_area :self_introduction %>
</div>
```

## 生成されたapp/views/devise/registrations/new.html.erbに_profile_fields.html.erbにいくrenderパスを追加
```
   <%= render 'devise/registrations/profile_fields', f: f %>
```

## 生成されたapp/views/devise/registrations/edir.html.erbにprofile_fields.html.erbにいくrenderパスを追加
```
  <%= render 'devise/registrations/profile_fields', f: f %>

```



# ストロングパラメータ(Strong Parameters)を追加
[ストロングパラメータ](https://techracho.bpsinc.jp/hachi8833/2023_09_01/133452#strong-params)
Deviseが使用するパラメータをカスタマイズするので、Deviseに関連するコントローラーのアクションが実行される前にconfigure_permitted_parametersメソッドを呼び出す。
これは、configure_permitted_parametersメソッドをカスタマイズすることで、ユーザー登録時やアカウント更新時のパラメータに
ユーザー名や自己紹介文など、追加のユーザー情報を扱うことを許可させるため。
→```  before_action :configure_permitted_parameters, if: :devise_controller?
```を追加する

では、追加のユーザー情報をパラメターとして許可するのはわかったが実際にどのように追加するのか？
→configure_permitted_parametersメソッドを使用して、Deviseのユーザー登録（:sign_up）とアカウント更新（:account_update）の際に許可させる

```
class ApplicationController < ActionController::Base
  before_action :authenticate_user! #特定のページや機能へのアクセスをログインしたユーザーに限定したい時に使う
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    keys = %i[name self_introduction]
    devise_parameter_sanitizer.permit(:sign_up, keys: keys)
    devise_parameter_sanitizer.permit(:account_update, keys: keys)
  end
end

```
上記により、サインアップ時 (Devise::RegistrationsController#create)と
アカウント編集時(Devise::RegistrationsController#update)のみnameカラムとself_introductionカラムの更新を許可します。


# i18nで日本語化
[i18n](https://ama-tech.hatenablog.com/rails-i18n)
config/initializers/locale.rbを手動で作成
```
I18n.available_locales = [:en, :ja]
I18n.default_locale = :ja

```
## 日本語のロケールファイルを作成
config/locales/ja.ymlを作成
```
ja:
  activerecord:
    models:
      book: 本
    attributes:
      user:
        name: 氏名
        self_introduction: 自己紹介文
  views:
    common:
      show: 詳細
      edit: 編集
      back: 戻る
      title_show: "%{name}の詳細"
      sign_out: ログアウト
  layouts:
    application:
      menu: メニュー
      sign_in_as: "%{email} としてログイン中"
  # devise-i18n-viewsをオーバーライド
  devise:
    registrations:
      edit:
        title: "アカウント編集"
```

## 各viewページに翻訳を反映
app/views/layouts/application.html.erbの中身を編集
```
  <body>
    <% if user_signed_in? %>
      <div class="menu-container">
        <div class="title"><%= t('.menu') %></div>
        <ul>
          <li>
            <%= link_to Book.model_name.human, books_path %>
          </li>
          <li>
            <%= link_to User.model_name.human, users_path %>
          </li>
        </ul>
        <div class="title"><%= t('.sign_in_as', email: current_user.email) %></div>
        <ul>
          <li>
            <%= link_to t('devise.registrations.edit.title'), edit_user_registration_path %>
          </li>
          <li>
            <%= link_to t('views.common.sign_out'), destroy_user_session_path, method: :delete %>
          </li>
        </ul>
      </div>
    <% end %>
```

## app/views/users/index.html.erbを編集

```
この部分を編集する
編集前
<h1>ユーザー一覧</h1>

<table>
  <thead>
    <tr>
      <th>Eメール</th>
      <th>氏名</th>
      <th></th>
    </tr>
  </thead>

編集後

<h1><%= User.model_name.human %></h1>

<table>
  <thead>
    <tr>
      <th><%= User.human_attribute_name(:email) %></th>
      <th><%= User.human_attribute_name(:name) %></th>
      <th></th>
    </tr>

  </thead>
```

## app/views/users/show.html.erbを同様に編集
```
<h1><%= t('views.common.title_show', name: User.model_name.human) %></h1>

<p>
  <strong><%= User.human_attribute_name(:email) %>:</strong>
  <%= @user.email %>
</p>

<p>
  <strong><%= User.human_attribute_name(:name) %>:</strong>
  <%= @user.name %>
</p>

<p>
  <strong><%= User.human_attribute_name(:self_introduction) %>:</strong>
  <%= @user.self_introduction %>
</p>

<% if current_user == @user %>
  <%= link_to t('views.common.edit'), edit_user_registration_path %> |
<% end %>
<%= link_to t('views.common.back'), users_path %>
```
