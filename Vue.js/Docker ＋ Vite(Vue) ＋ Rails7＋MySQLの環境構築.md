# **Docker ＋ Vite(Vue) ＋ Rails7＋**MySQLの**環境構築**

## 以下の構造でディレクトリとファイルを作る

```html
.
├── docker-compose.yml
├── .env
├── .gitignore
├── backend
│   ├── Dockerfile
│   ├── Gemfile
│   ├── Gemfile.lock
│   ├── entrypoint.sh
├── frontend
   　　└── Dockerfile
```

```html
ファイルなどの作成

touch  docker-compose.yml .env .gitignore

mkdir backend
cd backend
touch Dockerfile Gemfile Gemfile.lock entrypoint.sh

mkdir frontend
cd frontend
touch Dockerfile
```

## ファイル編集

```html

.env
```

```html
WORKDIR=app
MYSQL_ROOT_PASSWORD=mypassword
```

## **`docker-compose.yml`の編集**

```html
version: '3.8'
services:
  db:
    image: mysql:8.0.31 # DockerHubにあるmysql8.0のイメージを使用
    command: --default-authentication-plugin=mysql_native_password # MySQL8.0以降の認証方式に元に戻す設定
    # MySQLのデータを永続化するために、名前付きボリュームmysql-storeを/var/lib/mysqlにマウント
    volumes:
      - mysql-store:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD #=== .envの環境変数から持ってくる
    ports:
      - '3306:3306' #=== ホストのポート3306をコンテナのポート3306にマッピングしています。これにより、ホストマシンからMySQLサーバーにアクセス

  # build:セクションでは、バックエンドのDockerイメージをビルドする方法を指定しています。contextとdockerfileを使用してビルドのコンテキストとDockerfileの場所を定義し、argsでビルド時の引数を指定
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
      args:
        WORKDIR: $WORKDIR
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    ports:
      - '3000:3000'
    volumes:
      - ./backend:/$WORKDIR
      - gem-store:/usr/local/bundle
    environment:
      TZ: Asia/Tokyo
      RAILS_ENV: development
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD
    depends_on:
      - db
    stdin_open: true
    tty: true
volumes:
  mysql-store:
  gem-store:
```

### **`docker-compose.yml`**解説

このコードは、Docker Composeを使用して複数のコンテナを定義し、管理するためのYAMLフォーマットのファイルです。Docker Composeは、複数のコンテナを含むアプリケーションを定義し、実行するためのツールで、このファイルでは主にMySQLデータベースサーバーとRailsアプリケーションのバックエンドサービスを設定しています。説明を段階的かつ論理的に進めていきます。

### Docker Composeファイルの構造

- `version: '3.8'`は、使用するDocker Composeのファイルフォーマットのバージョンを指定します。これは、ファイルが使用する機能のセットを決定します。

### サービスの定義

このファイルでは2つのサービス、`db`と`backend`が定義されています。

### `db`サービス

- `image: mysql:8.0.31`は、`db`サービスが使用するDockerイメージを指定します。ここではMySQLのバージョン8.0.31を使用しています。
- `command: --default-authentication-plugin=mysql_native_password`は、MySQLサーバーを起動する際の追加コマンドを指定します。これは、MySQLの認証プラグインを`mysql_native_password`に設定しています。
- `restart: always`は、Dockerコンテナが停止した場合に常に再起動するように設定しています。
- `volumes:`セクションでは、MySQLのデータを永続化するために、名前付きボリューム`mysql-store`を`/var/lib/mysql`にマウントします。
- `environment:`セクションでは、環境変数`MYSQL_ROOT_PASSWORD`を設定しています。これは`.env`ファイルから取得されることが示唆されています。
- `ports:`セクションでは、ホストのポート3306をコンテナのポート3306にマッピングしています。これにより、ホストマシンからMySQLサーバーにアクセスできます。

### `backend`サービス

- `build:`セクションでは、バックエンドのDockerイメージをビルドする方法を指定しています。`context`と`dockerfile`を使用してビルドのコンテキストとDockerfileの場所を定義し、`args`でビルド時の引数を指定しています。
- `command:`は、コンテナが起動するときに実行されるコマンドを指定しています。Railsサーバーを起動する前に`server.pid`ファイルを削除し、`rails s`コマンドでRailsサーバーを起動します。
- `volumes:`セクションでは、バックエンドのソースコードと`gem-store`ボリュームをコンテナにマウントしています。これにより、開発中のコードの変更がコンテナにリアルタイムで反映され、Rubyのgemを永続化できます。
- `environment:`セクションでは、タイムゾーン、Railsの実行環境、MySQLのルートパスワードなど、バックエンドサービスの環境変数を設定しています。
- `depends_on:`は、`backend`サービスが`db`サービスに依存していることを示しています。これにより、`db`サービスが起動した後に`backend`サービスが起動します。
- `stdin_open: true`と`tty: true`は、コンテナがインタラクティブモードで実行されるように設定するために使用されます。これにより、コンテナがバックグラウンドで実行されずに、前面で実行され続けることを保証します。特に、Railsサーバーや他のインタラクティブなプロセスを実行する際に役立ちます。

### ボリュームの定義

- `volumes:`セクションの最後に、`mysql-store`と`gem-store`の2つの名前付きボリュームが定義されています。これらは、それぞれMySQLデータベースのデータと、RubyのGemパッケージを永続化するために使用されます。名前付きボリュームを使用することで、コンテナを再起動または再作成しても、これらのデータが保持されます。

### 追加情報

このDocker Composeファイルは、開発環境におけるバックエンドサービス（特にRailsアプリケーション）とデータベースサービス（MySQL）のセットアップを自動化するために設計されています。この構成を使用することで、開発者は複数のサービスを個別に設定する手間を省き、一貫した環境でアプリケーションを開発・テストできます。

また、`environment`セクションで使用されている`$MYSQL_ROOT_PASSWORD`や`$WORKDIR`などの変数は、`.env`ファイルや環境変数から値を取得することを想定しています。これにより、機密情報をファイルにハードコーディングすることなく、設定を柔軟に管理できます。

このDocker Composeファイルを使用するには、同じディレクトリに`.env`ファイルを作成し、必要な環境変数（例: `MYSQL_ROOT_PASSWORD`や`WORKDIR`）の値を定義する必要があります。そして、`docker-compose up`コマンドを実行することで、定義されたサービスがビルドされ、起動します。

この説明がプログラミング初学者にとって理解しやすいものであれば幸いです。もし追加で説明が必要な部分があれば、遠慮なく質問してください。

環境変数がちゃんと設定されているか確認

```html
$ docker-compose config
```

`Gemfile`のみ編集する

```html
source "https://rubygems.org"
gem "rails", "~> 7.0.4"
```

## **`Dockerfile`（backend側）の編集**

```html
FROM ruby:3.1.2-bullseye
#=== docker-compose.ymlから渡されたもの
ARG WORKDIR
#=== インストールするパッケージ
ARG RUNTIME_PACKAGES="build-essential tzdata git less"

ENV HOME=/${WORKDIR}

WORKDIR ${HOME}

RUN apt-get update \
    && apt-get install -y --no-install-recommends ${RUNTIME_PACKAGES} \
    && apt-get clean \
    && rm -rf /var/cache/apt/archives/* \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock

ENV LANG=C.UTF-8 \
    TZ=Asia/Tokyo

#=== Bundlerを最新のものに
RUN gem update --system \
    && gem install bundler
RUN bundle install

COPY . ./
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]

EXPOSE 3000
CMD ["rails", "server", "-b", "0.0.0.0"]
```

### **`Dockerfile`（backend側）**解説

このDockerfileは、Ruby on Railsアプリケーションを実行するためのDockerイメージを構築するための設定ファイルです。Dockerfileは、イメージの作成に必要な命令を含むテキストファイルで、ここに定義されたステップに従ってDockerイメージがビルドされます。以下、コードの各部分について段階的かつ論理的に説明します。

### 基本イメージの指定

```
FROM ruby:3.1.2-bullseye

```

- `FROM`命令は、イメージのベースとなる親イメージを指定します。ここでは、Ruby 3.1.2をDebian Bullseyeバージョンに基づいて提供する公式のRubyイメージを使用しています。これは、アプリケーションの実行環境を提供します。

### ビルド引数の定義

```
ARG WORKDIR

```

- `ARG`命令は、ビルド時にDockerfileに渡される引数を定義します。`WORKDIR`は、`docker-compose.yml`ファイルから渡される想定で、アプリケーションの作業ディレクトリを指定するために使用されます。

### 環境変数の設定

```
ENV HOME=/${WORKDIR}

```

- `ENV`命令は、イメージ内で使用される環境変数を設定します。ここでは、ホームディレクトリのパスを設定しています。

### 作業ディレクトリの設定

```
WORKDIR ${HOME}

```

- `WORKDIR`命令は、Dockerコンテナ内の作業ディレクトリを設定します。ここでは、前のステップで設定したホームディレクトリを作業ディレクトリとして使用します。

### パッケージのインストール

```
RUN apt-get update \\
    && apt-get install -y --no-install-recommends ${RUNTIME_PACKAGES} \\
    && apt-get clean \\
    && rm -rf /var/cache/apt/archives/* \\
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

```

- `RUN`命令は、イメージビルド中にシェルコマンドを実行します。この命令は、パッケージリストの更新、必要なパッケージのインストール、そしてキャッシュファイルの削除を行います。

### Gemfileのコピー

```
COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock

```

- `COPY`命令は、ファイルやディレクトリをホストからイメージ内にコピーします。ここでは、`Gemfile`と`Gemfile.lock`をアプリケーションディレクトリにコピーしています。

### 言語とタイムゾーンの設定

```
ENV LANG=C.UTF-8 \\
    TZ=Asia/Tokyo

```

- ここでは、コンテナの言語設定をUTF-8に、タイムゾーンを東京に設定しています。

### Bundlerの更新とGemのインストール

```
RUN gem update --system \\
    && gem install bundler
RUN bundle install

```

- RubyGemsシステムの更新、最新のBundlerのインストール、そして`Gemfile`に記載された依存関係のインストールを行います。

### アプリケーションコードのコピー

```
COPY . ./

```

- アプリケーションの

残りの部分（`COPY . ./`）は、ホストマシン上の現在のディレクトリ（Dockerfileが存在するディレクトリ）から、コンテナの作業ディレクトリ（`WORKDIR`で指定されたディレクトリ）へと、アプリケーションのソースコードやその他のファイルをコピーします。これにより、ビルドプロセス中にアプリケーションのコードがDockerイメージ内に含まれるようになります。

### エントリーポイントスクリプトの設定

```
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]

```

- まず、`entrypoint.sh`という名前のスクリプトファイルをホストマシンから`/usr/bin/`ディレクトリにコピーします。次に、`RUN chmod +x /usr/bin/entrypoint.sh`コマンドで、このスクリプトが実行可能になるようにファイルのパーミッションを変更します。最後に、`ENTRYPOINT`命令を使用してコンテナが起動する際に実行されるデフォルトのコマンドを指定します。ここでは、コンテナ起動時に`entrypoint.sh`スクリプトが実行されるように設定しています。このスクリプトは、アプリケーションを起動する前に必要な前処理（例えば、データベースマイグレーションの実行や既存のサーバープロセスのクリーンアップなど）を行うために使われることが多いです。

### ポートの公開

```
EXPOSE 3000

```

- `EXPOSE`命令は、Dockerコンテナ内でリッスンしているポートを指定します。この例では、Railsアプリケーションのデフォルトポートである3000番ポートを公開しています。これにより、Dockerコンテナ外部から3000番ポートにアクセスできるようになります。

### デフォルトコマンドの設定

```
CMD ["rails", "server", "-b", "0.0.0.0"]

```

- `CMD`命令は、コンテナが起動する際に実行されるデフォルトのコマンドを指定します。この例では、`rails server -b 0.0.0.0`コマンドを使用してRailsサーバーを起動し、コンテナのIPアドレスの全てのインターフェースでリッスンします。これにより、Dockerホストマシンや他のコンテナからアクセスできるようになります。

このDockerfileを使うことで、Railsアプリケーションの開発、テスト、デプロイが容易になります。Dockerを使用することで、アプリケーションが必要とする依存関係や環境を一貫して管理でき、開発者間や異なる環境間での「動かない！」という問題を解消できます。

## **`entrypoint.sh`の編集**

```html
#!/bin/bash
set -e

rm -f /app/tmp/pids/server.pid

exec "$@"
```

**バックエンド側をビルドしてプロジェクトを新規作成する**

```html
まずはビルドする

docker-compose build

```

```html
次にプロジェクト作成

docker-compose run --rm backend rails new . -f -B -d mysql --api --skip-test --skip-turbolinks
```

```html
コマンドの引数を紹介
### rails new オプション補足 ###
# -f　　　既にあるファイルを上書き(Gemfile/Gemfile.lock)
# -B　　　bundle installコマンドを実行しない
# -d　　　データベースを指定(デフォルトはsqlite)
# --api　　　APIモードで作成する
# --skip-test　　　minitestを作成しない
# --skip-turbolinks　　　turbolinksを作成しない
```

## **`backend/config/database.yml`にコードを追加編集**

`host`を`docker-compose.yml`に記載したサービス名、 `password`を`.env`に記載したものを`ENV`で取得する。

この時、やりがちなのが**`docker-compose.yml`**ファイルで定義したMySQLサービスの名前の変更忘れだったりする

MySQLサービスの名前は**`db`**として定義されているのに、**`config/database.yml`**内の**`host`の名前が`localhost`だったりするのでちゃんと確認する（やらかした）**

```html
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: <%= ENV['MYSQL_ROOT_PASSWORD'] %>
  host: db

development:
  <<: *default
  database: app_development

test:
  <<: *default
  database: app_test

production:
  <<: *default
  database: app_production
  username: app
  password: <%= ENV["APP_DATABASE_PASSWORD"] %>
```

不安ならば、特に、**`host`**と**`password`**の値が**`.env`**ファイルや**`docker-compose.yml`**ファイルで指定したものと一致しているかを確認する

ちなみに**`password`**は**`.env`**ファイルから取得してることも忘れずに。

**動作確認**

```html
$ docker-compose build

$ docker-compose run --rm backend bundle install

$ docker-compose run --rm backend rails db:create

$ docker-compose up
```

localhost:3000にアクセス

## 次はfrontendの編集。

## **docker-compose.ymlの編集**

docker-compose.ymlの**`services`**セクションの下に以下のコードを追加、既に定義されている**`db`**と**`backend`**サービスの後に新しい**`frontend`**サービスの設定を追加(volumes:の上)

```html
frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
      args:
        WORKDIR: $WORKDIR
    volumes:
      - ./frontend:/$WORKDIR:cached
    ports:
      - '5173:5173'
    depends_on:
      - backend
    tty: true
    command: yarn dev --host
```

全体（インデントが正しいか確認する）

```html
version: '3.8'
services:
  db:
    image: mysql:8.0.31 # DockerHubにあるmysql8.0のイメージを使用
    command: --default-authentication-plugin=mysql_native_password # MySQL8.0以降の認証方式に元に戻す設定
    # MySQLのデータを永続化するために、名前付きボリュームmysql-storeを/var/lib/mysqlにマウント
    volumes:
      - mysql-store:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD #=== .envの環境変数から持ってくる
    ports:
      - '3306:3306' #=== ホストのポート3306をコンテナのポート3306にマッピングしています。これにより、ホストマシンからMySQLサーバーにアクセス

  # build:セクションでは、バックエンドのDockerイメージをビルドする方法を指定しています。contextとdockerfileを使用してビルドのコンテキストとDockerfileの場所を定義し、argsでビルド時の引数を指定
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
      args:
        WORKDIR: $WORKDIR
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    ports:
      - '3000:3000'
    volumes:
      - ./backend:/$WORKDIR
      - gem-store:/usr/local/bundle
    environment:
      TZ: Asia/Tokyo
      RAILS_ENV: development
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD
    depends_on:
      - db
    stdin_open: true
    tty: true

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
      args:
        WORKDIR: $WORKDIR
    volumes:
      - ./frontend:/$WORKDIR:cached
      - node_modules:/$WORKDIR/node_modules
    ports:
      - '5173:5173'
    depends_on:
      - backend
    tty: true
    command: yarn dev --host

volumes:
  mysql-store:
  gem-store:
  node_modules:
```

### この時に出たエラー

エラーメッセージ「services.ports must be a mapping」に対処するため、提供された`docker-compose.yml`ファイルを見直しました。ファイルの内容に基づき、`frontend`サービスの定義に構文エラーが見つかりました。インデント（字下げ）が正しくないため、Docker Composeがファイルを正しく解析できていないようです。

### 問題の箇所

`frontend`サービスの定義で、`build`セクション以下のプロパティが`frontend`サービスに対して適切にインデントされていません。これにより、YAMLファイルの構造が壊れ、Docker Composeがファイルを解析できなくなっています。

### 修正後の`docker-compose.yml`

```yaml
version: '3.8'
services:
  db:
    image: mysql:8.0.31
    command: --default-authentication-plugin=mysql_native_password
    volumes:
      - mysql-store:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD
    ports:
      - '3306:3306'

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
      args:
        WORKDIR: $WORKDIR
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    ports:
      - '3000:3000'
    volumes:
      - ./backend:/$WORKDIR
      - gem-store:/usr/local/bundle
    environment:
      TZ: Asia/Tokyo
      RAILS_ENV: development
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD
    depends_on:
      - db
    stdin_open: true
    tty: true

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
      args:
        WORKDIR: $WORKDIR
    volumes:
      - ./frontend:/$WORKDIR:cached
    ports:
      - '5173:5173'
    depends_on:
      - backend
    tty: true
    command: yarn dev --host

volumes:
  mysql-store:
  gem-store:

```

### 修正点

- `frontend`サービスの`build`セクション以下のすべての行を、`frontend:`の下に正しくインデント（2スペースまたは4スペースの字下げ）しました。これにより、YAMLファイルの階層構造が修正され、`frontend`サービスの設定が正しく認識されるようになります。
- YAMLでは、インデントに一貫性が必要です。`services:`の下の各サービス（`db`, `backend`, `frontend`）と、それらのサービスのプロパティ（`build`, `volumes`, `ports`など）は、適切にインデントされている必要があります。

### まとめ

YAMLファイルではインデントが構造を定義するため、正しいインデントが非常に重要です。上記の修正を行った後、再度`docker-compose build`を実行して、問題が解決されたか確認してください。

## **`フロントエンドのDockerfile`の編集（この`Dockerfile`はエラーが発生する。後で見返せるように取っておく）**

```html
FROM node:16.17.1-bullseye
ARG WORKDIR

ENV HOME=/${WORKDIR}

WORKDIR ${HOME}

RUN apt update \
    && yarn install

EXPOSE 5173
CMD ["yarn", "dev", "--host"]
```

## **フロント側をビルドしてプロジェクトを新規作成する**

まずはビルドする。

docker-compose build

プロジェクトの新規作成。
docker-compose run --rm frontend npm init vite@latest

<aside>
💡 この時脳死でエンターしてると変な設定をしてしまう

Ok to proceed? (y) y
✔ Project name: … vite-project
✔ Select a framework: › Vue
✔ Select a variant: › JavaScript

```html
$ docker-compose run --rm frontend npm init vite@latest
Starting xxx_db_1 ... done
Starting xxx_backend_1 ... done
Creating xxx_frontend_run ... done
Need to install the following packages:
  create-vite@3.1.0
Ok to proceed? (y) y
✔ Project name: … vite-project
✔ Select a framework: › Vue
✔ Select a variant: › JavaScript

Scaffolding project in /app/vite-project...

Done. Now run:

  cd vite-project
  npm install
  npm run dev
```

</aside>

プロジェクトの整理(必ず一個一個丁寧にやる）

```html
$ ls -A frontend/vite-project 
.gitignore     .vscode        README.md      index.html     package.json   public         src            vite.config.js

$ mv frontend/vite-project/* /app/
mv: /app/ is not a directory

$ mv frontend/vite-project/* frontend 

$ ls -A frontend/vite-project        
.gitignore .vscode

$ mv frontend/vite-project/.gitignore frontend/ && mv frontend/vite-project/.vscode frontend 

$ ls -A frontend/vite-project

$ rmdir frontend/vite-project
```

## `yarn`を使用するため以下を実行する

docker-compose run --rm frontend yarn install

### エラー発生

```html
次にdocker-compose run --rm frontend yarn install を行ったらでたエラーがこれです。

```
% docker-compose run --rm frontend yarn install
[+] Running 2/0
 ⠿ Container docker_rails7_vue_sample-db-1       Running                                                                  0.0s
 ⠿ Container docker_rails7_vue_sample-backend-1  Running                                                                  0.0s
yarn install v1.22.19
info No lockfile found.
[1/4] Resolving packages...
[2/4] Fetching packages...
error vite@5.0.12: The engine "node" is incompatible with this module. Expected version "^18.0.0 || >=20.0.0". Got "16.17.1"
error Found incompatible module.
info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
```
```

このエラーメッセージは、`vite`パッケージが現在のNode.jsのバージョン（16.17.1）と互換性がないことを示しています。`vite@5.0.12`はNode.jsのバージョン`^18.0.0`または`>=20.0.0`を必要としますが、Dockerfileで使用されているNode.jsのイメージはバージョン`16.17.1`です。この互換性の問題を解決するためには、以下の方法があります。

### DockerfileのNode.jsのバージョンを更新する

フロントエンドのDockerfileで使用されているNode.jsのバージョンを、`vite`が要求するバージョンに更新する必要があります。例えば、Node.jsの最新のLTS（Long Term Support）バージョンや、`vite`がサポートするバージョンに合わせます。

### Dockerfileの修正例

```
FROM node:18-bullseye
ARG WORKDIR

ENV HOME=/${WORKDIR}

WORKDIR ${HOME}

RUN apt update \\
    && yarn install

EXPOSE 5173
CMD ["yarn", "dev", "--host"]

```

この例では、`FROM node:18-bullseye`に更新していますが、`vite`が要求するバージョンに応じて、`node:20-bullseye`など他のバージョンにすることもできます。Node.jsのイメージのバージョンを変更した後は、Dockerイメージを再ビルドする必要があります。

### Dockerイメージの再ビルド

Dockerfileを修正した後、以下のコマンドを実行してフロントエンドのDockerイメージを再ビルドします。

```bash
docker-compose build frontend

```

このコマンドは、`frontend`サービスのDockerイメージをビルドし直します。ビルドが成功したら、再び`yarn install`を実行してみてください。

### まとめ

この問題は、特定のNode.jsパッケージが要求するNode.jsのバージョンと、Dockerイメージ内で使用されているNode.jsのバージョンとの間の互換性に関連しています。Node.jsのバージョンを適切なものに更新することで、エラーを解決し、`yarn install`を成功させることができます。

## **DockerfileのNode.jsのバージョンを更新する**

FROM node:18-bullseyeに変更

```html
FROM node:18-bullseye
ARG WORKDIR

ENV HOME=/${WORKDIR}

WORKDIR ${HOME}

RUN apt update \
    && yarn install

EXPOSE 5173

```

## **Dockerイメージの再ビルド**

docker-compose build frontend
docker-compose run --rm frontend npm init vite@latest

<aside>
💡 この時脳死でエンターしてると変な設定をしてしまう

Ok to proceed? (y) y
✔ Project name: … vite-project
✔ Select a framework: › Vue
✔ Select a variant: › JavaScript

```html
$ docker-compose run --rm frontend npm init vite@latest
Starting xxx_db_1 ... done
Starting xxx_backend_1 ... done
Creating xxx_frontend_run ... done
Need to install the following packages:
  create-vite@3.1.0
Ok to proceed? (y) y
✔ Project name: … vite-project
✔ Select a framework: › Vue
✔ Select a variant: › JavaScript

Scaffolding project in /app/vite-project...

Done. Now run:

  cd vite-project
  npm install
  npm run dev
```

</aside>

```html
$ ls -A frontend/vite-project 
.gitignore     .vscode        README.md      index.html     package.json   public         src            vite.config.js

$ mv frontend/vite-project/* /app/
mv: /app/ is not a directory

$ mv frontend/vite-project/* frontend 

$ ls -A frontend/vite-project        
.gitignore .vscode

$ mv frontend/vite-project/.gitignore frontend/ && mv frontend/vite-project/.vscode frontend 

$ ls -A frontend/vite-project

$ rmdir frontend/vite-project
```

## `yarn`を使用するため以下を実行する

docker-compose run --rm frontend yarn install

## **`Dockerfile` / `docker-compose.yml` の追加記述**

```html
#=== 以下を追加
COPY . ./
COPY package.json /app/package.json
COPY yarn.lock /app/yarn.lock

結果
FROM node:18-bullseye
ARG WORKDIR

ENV HOME=/${WORKDIR}

WORKDIR ${HOME}

RUN apt update \
    && yarn install

COPY . ./
COPY package.json /app/package.json
COPY yarn.lock /app/yarn.lock

EXPOSE 5173
=== 以下を追加
CMD ["yarn", "dev", "--host"]
```

```html
インデントに気をつけてdocker-compose.ymlに以下の追加

- node_modules:/$WORKDIR/node_modules

node_modules:
```

docker-compose.ymlの最終結果

```html

version: '3.8'
services:
  db:
    image: mysql:8.0.31 # DockerHubにあるmysql8.0のイメージを使用
    command: --default-authentication-plugin=mysql_native_password # MySQL8.0以降の認証方式に元に戻す設定
    # MySQLのデータを永続化するために、名前付きボリュームmysql-storeを/var/lib/mysqlにマウント
    volumes:
      - mysql-store:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD #=== .envの環境変数から持ってくる
    ports:
      - '3306:3306' #=== ホストのポート3306をコンテナのポート3306にマッピングしています。これにより、ホストマシンからMySQLサーバーにアクセス

  # build:セクションでは、バックエンドのDockerイメージをビルドする方法を指定しています。contextとdockerfileを使用してビルドのコンテキストとDockerfileの場所を定義し、argsでビルド時の引数を指定
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
      args:
        WORKDIR: $WORKDIR
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    ports:
      - '3000:3000'
    volumes:
      - ./backend:/$WORKDIR
      - gem-store:/usr/local/bundle
    environment:
      TZ: Asia/Tokyo
      RAILS_ENV: development
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD
    depends_on:
      - db
    stdin_open: true
    tty: true

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
      args:
        WORKDIR: $WORKDIR
    volumes:
      - ./frontend:/$WORKDIR:cached
      - node_modules:/$WORKDIR/node_modules
    ports:
      - '5173:5173'
    depends_on:
      - backend
    tty: true
    command: yarn dev --host

volumes:
  mysql-store:
  gem-store:
  node_modules:
```

**動作確認

docker-composeでコンテナを起動させる**

```html
$ docker-compose build --no-cache

$ ❯ docker-compose up
```

http://localhost:5173/ にアクセス


できたー！！！
<img width="476" alt="スクリーンショット 2024-02-07 21 43 11" src="https://github.com/kb8864/Study-Notes/assets/128299525/5e7d806c-6784-40c0-affe-21c71c5a269f">

この状態でlocalhost:3000にもアクセスできる
<img width="570" alt="スクリーンショット 2024-02-07 21 44 47" src="https://github.com/kb8864/Study-Notes/assets/128299525/13bc910b-fb92-4035-8604-fea86a4757fd">

### 参考記事
[参考記事](https://qiita.com/Shuhei_Nakada/items/148ce4c7c5bd4d11bcf0)
