# JWT
Gemに頼ったログイン機能から脱却。JWT。Nuxt.jsを使ってSPAを構築<details>
  <summary>説明</summary>
  
```
root(作業ディレクトリ)

/Users/user/workspace/Vue.js_practice_project/demoapp_v1

git init

Rails、Nuxt.jsディレクトリを作成
mkdir {api,front}

cd api
touch {Dockerfile,Gemfile,Gemfile.lock}
```

</details>



<details>
  <summary>railsを動かすためのDokerファイル</summary>
  
```
FROM ruby:2.7.1-alpine

ARG WORKDIR
ARG RUNTIME_PACKAGES="nodejs tzdata postgresql-dev postgresql git"
ARG DEV_PACKAGES="build-base curl-dev"

ENV HOME=/${WORKDIR} \
    LANG=C.UTF-8 \
    TZ=Asia/Tokyo

# ENV test（このRUN命令は確認のためなので無くても良い）
# HOMEという変数を展開。ENVで指定している
# RUN echo ${HOME}

# Dockerファイルで定義した命令を実行するコンテナ内の作業ディレクトリを定義。RUNやcopy、ADDやCMDなど
# コンテナ/app/Railsアプリ
WORKDIR ${HOME}
#ホスト側のファイルをコンテナにコピー
# COPY コピー元（ホスト）コピー先（コンテナ）
# COPY コピー元（ホスト）=dockerファイルがあるディレクトリ以下を指定（今回はapiディレクトリ）
# コピー先（コンテナ）＝カレントディレクトリ
COPY Gemfile* ./

RUN apk update && \
    apk upgrade && \
    apk add --no-cache ${RUNTIME_PACKAGES} && \
    apk add --virtual build-dependencies --no-cache ${DEV_PACKAGES} && \
    bundle install -j4 && \
    apk del build-dependencies

COPY . ./
#　コンテナ内で実行したいコマンドを定義。-b　定義した0.0.0.0アドレスに紐付け（バインド）する
# 通常コンテナ内で起動したRailsは外部のブラウザから参照することができないが、IPアドレスを0000にバインドすることによってコンテナのRailsを外部のブラウザから参照することが可能。
CMD ["rails", "server", "-b", "0.0.0.0"]

```

</details>


<details>
  <summary>Nuxtを動かすためのDockerファイル</summary>
  
```FROM node:20.11-alpine

ARG WORKDIR


ENV HOME=/${WORKDIR} \
    LANG=C.UTF-8 \
    TZ=Asia/Tokyo \
    HOST=0.0.0.0


WORKDIR ${HOME}
# コンテナ公開用のポート番号を指定
# docker-compose.ymlのportsで「8080」を指定するので、ここには3000番を入れてる
# EXPOSE ${CONTAINER_PORT}


```

</details>

<details>
  <summary>.gitignoreを編集</summary>
  
```
/.env
/.DS_Store

```

</details>

<details>
  <summary>.envを編集</summary>
  環境変数とは、開発・テスト・本番などの環境ごとに変化する値を入れる変数
  環境変数を管理すると言うよりか「docker-composeファイル、Dockerfile、コンテナ間で共通する値を変数化してDocker管理を楽にする」ことを目的
  
  
```
# commons
WORKDIR=app
API_PORT=3000
FRONT_PORT=8080

# db
POSTGRES_PASSWORD=password


```

</details>


<details>
  <summary>docker-compose.yml</summary>
  
```
# composeファイルのバージョン指定
version: "3.8"
# サービス(= コンテナ)
services:
  db:
    image: postgres:13.12-alpine
    environment:
      # OSのタイムゾーン
      TZ: UTC
      # postgresのタイムゾーン
      PGTZ: UTC
      POSTGRES_PASSWORD: $POSTGRES_PASSWORD
    volumes:
      - "./api/tmp/db:/var/lib/postgresql/data"

  api:
    # ベースイメージとなるDockerfileを指定。今回はargsキーを使用したい。
    build:
      context: ./api
      args:
        WORKDIR: $WORKDIR
    environment:
      POSTGRES_PASSWORD: $POSTGRES_PASSWORD
    volumes:
      - "./api:/$WORKDIR"
    # サービスの依存関係を定義(起動の順番)
    depends_on:
      - db
    # 公開したいポート番号:コンテナポート
    ports:
      - "$API_PORT:3000"

  front:
    build:
      context: ./front
      args:
        WORKDIR: $WORKDIR
    # コンテナで実行したいコマンド(CMD)
    command: yarn run dev
    volumes:
      - "./front:/$WORKDIR"
    ports:
      - "$FRONT_PORT:3000"
    depends_on:
      - api
```

</details>

