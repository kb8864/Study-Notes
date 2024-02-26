# rails側の設定


# Heroku用にPumaのセッティングを行う
Pumaとは、複数のリクエストを並行して処理することができる高速化を目的としたWebサーバ
「api/config」直下のpuma.rbを編集
[公式サイトから引用(https://devcenter.heroku.com/articles/deploying-rails-applications-with-the-puma-web-server#config)

 # heroku.ymlファイルを作成する
 heroku.ymlとは、Herokuアプリケーションの定義に使用するマニフェス、いわば本番環境用のdocker-compose.ymlファイル

 Rails用のheroku.ymlを作成
「api」ディレクトリ直下にheroku.ymlを作成

# heroku.ymlを編集する
<details>
  <summary>説明</summary>
  
```
# アプリ環境を定義する場所
setup:
  # アプリ作成時にアドオンを自動で追加する
  addons:
    - plan: heroku-postgresql
  # 環境変数を指定する
  config:
    # Rackへ現在の環境を示す
    RACK_ENV: production
    # Railsへ現在の環境を示す
    RAILS_ENV: production
    # log出力のフラグ(enabled => 出力する)
    RAILS_LOG_TO_STDOUT: enabled
    # publicディレクトリからの静的ファイルを提供してもらうかのフラグ(enabled => 提供してもらう)
    RAILS_SERVE_STATIC_FILES: enabled
# ビルドを定義する場所
build:
  # 参照するDockerfileの場所を定義(相対パス)
  docker:
    web: Dockerfile
  # Dockerfileに渡す環境変数を指定
  config:
    WORKDIR: app
# プロセスを定義
run:
  # Bundlerでインストールされたgemを使用してコマンドを実行
  web: bundle exec puma -C config/puma.rb

```

</details>



# docker imagesで

```
% docker images
REPOSITORY                          TAG            IMAGE ID       CREATED         SIZE
udemy_demoapp_v1-api                latest         36b680c9d5fc   2 hours ago     815MB
```
%
docker inspect -f="{{ .Config.Cmd }}" 36b680c9d5fc


#Compose と Rails
[docker-compose.yml](https://docs.docker.jp/compose/rails.html#:~:text=%E6%9C%80%E5%BE%8C%E3%81%AB%20docker%2Dcompose.yml%20%E3%81%8C%E5%8F%96%E3%82%8A%E3%81%BE%E3%81%A8%E3%82%81%E3%81%A6%E3%81%8F%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82)
# Dockerのドキュメントからコピペするコード
command: /bin/sh -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"

コンテナに入りこのコードを実行(apiサーバーのコンテナ内にはいる）
docker-compose run --rm api sh
```
~ # /bin/sh -c "echo TEST"
TEST
```


railsのｄockerfileの以下のコードを削除
```
CMD ["rails", "server", "-b", "0.0.0.0"]
```
次にdocker-compose build api
次にdocker images
```
 docker images
REPOSITORY                          TAG            IMAGE ID       CREATED              SIZE
udemy_demoapp_v1-api                latest         e90d9a0351c0   About a minute ago   815MB
```
```
docker inspect -f="{{ .Config.Cmd }}" e90d9a0351c0
```

使っていないdocker imageを全削除
docker image prune -f

# HerokuCILのプラグインmanifestを導入する
作成したheroku.ymlにsetupを記述したが、それおｗぽ導入うするための手順
[HerokuCLI-manifestのデプロイ解説](https://blog.cloud-acct.com/posts/u-setup-herokuyml-deploy)

# Herokuのリモートリポジトリ先を確認する
% git remote -v
heroku	https://git.heroku.com/demoapp-v1-api.git (fetch)
heroku	https://git.heroku.com/demoapp-v1-api.git (push)
origin	git@github.com:kb8864/rails7-Nuxt_api.git (fetch)
origin	git@github.com:kb8864/rails7-Nuxt_api.git (push)

# GitHubにpushする
[公式の手順](https://devcenter.heroku.com/articles/keys)

```
api % pwd
/Users/user/workspace/rails+Nuxt/udemy_demoapp_v1/api
% ssh-keygen -f ~/.ssh/id_rsa_JWT_heroke -C kono.loca
+---[RSA 3072]----+
|+     o +. ..    |
|o+   . *.=..     |
| *o . ..* + .    |
|. ++ . ..o =     |
|  . + o S.. o    |
|   + . ..... .   |
|  E   .o=o=o.    |
|      .oo*o.+.   |
|       .o+.. ..  |
+----[SHA256]-----+
% ls ~/.ssh
```

## herokuに公開鍵を追加する方法
heroku keys:add 追加した公開鍵のファイルパス
```
% heroku keys:add ~/.ssh/id_rsa_JWT_heroke.pub

Uploading /Users/user/.ssh/id_rsa_JWT_heroke.pub SSH key... done
```
追加したキーの確認方法（ターミナル）
heroku keys
```
% heroku keys
=== @gmail.com keys
ssh-rsa AAAAB3NzaC...SqwMsJoyc= kono.loca
```

## SSHキーの登録
[SSHキーの登録](https://devcenter.heroku.com/articles/keys#common-ssh-key-problems)
```
Host heroku.com
  HostName heroku.com
  IdentityFile /path/to/key_fileー＞~/.ssh/鍵
  IdentitiesOnly yes
```

## herokuのSSH接続（herokuはGitのSSH接続を2021年11月30日に非推奨）
% ssh -v git@git.heroku.com
git remote -vでHTTP接続になっていることを確認

```
% git remote -v
heroku	https://git.heroku.com/demoapp-v1-api.git (fetch)
heroku	https://git.heroku.com/demoapp-v1-api.git (push)
```
SSH接続へ変更するためにまず削除
 git remote remove heroku
```
% git remote -v
origin	git@github.com:kb8864/rails7-Nuxt_api.git (fetch)
origin	git@github.com:kb8864/rails7-Nuxt_api.git (push)
```


リモートリポジトリを追加
git remote add heroku git@heroku.com:<アプリ名>
api $ git remote add heroku git@heroku.com:demoapp-v1-api-app.git
```
api % git remote add heroku git@heroku.com:demoapp-v1-api-app.git
api % git remote -v
heroku	git@heroku.com:demoapp-v1-api-app.git (fetch)
heroku	git@heroku.com:demoapp-v1-api-app.git (push)
origin	git@github.com:kb8864/rails7-Nuxt_api.git (fetch)
origin	git@github.com:kb8864/rails7-Nuxt_api.git (push)
```

Heroku スタックの確認
```
%  heroku stack
 ▸    Couldn't find that app.
エラーが出ている・・・
```
以下の記事で解決
[ ▸ Couldn't find that app.」というエラーが出てきた時の対処メモ](https://qiita.com/at-946/items/ce4db1d80429d6984cfc)
dekita
```
 %  heroku stack
=== ⬢ demoapp-v1-api Available Stacks
* container
  heroku-20
  heroku-22
```

git commit 
git push
git push heroku main

 
# 本番環境のPostgreSQLを初期化の準備
railsマスターキーを登録する
 % tree config -L 1でmaster.key確認できる
 herokuの環境変数にマスターキーを登録する
 ```
api % pbcopy < config/master.key
api % heroku config:set RAILS_KEY=コピーしたマスターキー
```


# 本番環境のPostgreSQLを初期化
マイグレーションコマンド
```
heroku run rails db:migrate
```


<details>
  <summary>エラーが出たら・・</summary>
  
```
エラー対応。Herokuにmaster keyをセットする
このエラーはHerokuにmaster.keyをセットすれば直る。

第三者に見られたく無い情報なので、Herokuの環境変数に直接設定。

api $ heroku config:set RAILS_MASTER_KEY=<master key>
間違った場合はもう一度同じ環境変数名でセットすればOK
api $ heroku config

RACK_ENV:                 production
RAILS_ENV:                production
RAILS_LOG_TO_STDOUT:      enabled
RAILS_MASTER_KEY:         e4hdkljefwierlbjiej32376sldkv # OK!!
RAILS_SERVE_STATIC_FILES: enabled
もう一度データベースの初期化を行う

```

</details>
## 
