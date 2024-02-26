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
