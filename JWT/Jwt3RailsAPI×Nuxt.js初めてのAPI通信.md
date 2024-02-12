# RailsAPI×Nuxt.js初めてのAPI通信
- docker-compose.ymlにAPI通信のURLを登録する
- Rails側に"Hello"を返すコントローラーを作成する
- 最終的にブラウザ上にHelloを表示

# docker-compose.ymlにAPI通信用のURLを環境変数として登録
apiサービスにはenvironmentに「API_DOMAIN」を、
frontサービスにはargsに「API_URL」を追加
```
 api:
  ...
  environment:
    POSTGRES_PASSWORD: $POSTGRES_PASSWORD
    API_DOMAIN: "localhost:$FRONT_PORT"       # 追加
  ...

	front:
    ...
      args:
        WORKDIR: $WORKDIR

        API_URL: "http://localhost:$API_PORT" # 追加    
    ...
```
frontサービスで指定したargsはDockerfileに渡されます。

そこで「front」ディレクトリのDockerfileに、引数を受け取るARG命令を追加し、環境変数として登録します。
この設定によりNuxt.jsのアプリ内で「API_URL」が参照できるようになります。

## front/Dockerfile
```
FROM node:20.11-alpine

ARG WORKDIR
# 追加
ARG API_URL

ENV HOME=/${WORKDIR} \
    LANG=C.UTF-8 \
    TZ=Asia/Tokyo \
    HOST=0.0.0.0 \
    # 追加
    API_URL=${API_URL}

WORKDIR ${HOME}

```

# Dockerfileを書き換えたのでfrontイメージをビルド
docker-compose build front

# RailsにHelloを返すコントローラーを作成

```
docker-compose run --rm api rails g controller api::v1::hello
api::v1::hello ... Railsの「controllers/api/v1」ディレクトリ以下にコントローラーを作成
ディレクトリがなければ自動で作成
```

# 作成したhello_controller.rbに"Hello"のjsonを返すアクションを追加
```
class Api::V1::HelloController < ApplicationController
  # 追加
 def index
   render json: "Hello"
 end
end
```

# api/config/routes.rb
namespaceを付けた時のRailsのルートはhttp://localhost:3000/api/v1/helloとなる
```
Rails.application.routes.draw do
  # 追加
  namespace :api do
    namespace :v1 do
      # api test action
      resources :hello, only:[:index]
    end
  end
en
```

