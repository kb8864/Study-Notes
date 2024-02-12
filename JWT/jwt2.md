apiとfrontのリポジトリを分けてコミット
```
% cd api
```
vimで以下の部分を追記
```
vi .gitignore

# orignal
.DS_Store
```

git commitまで！

# ロントコンテナ上でGitがインストールされていない
```
 % cd front

git init
git add -A
git commit -m

```

# 子ディレクトリをサブモジュール化する手順

NGパターン
```
 % git ls-files
.gitignore
api
docker-compose.yml
front/.config/sao-nodejs/config.json
front/.editorconfig
front/.eslintrc.js
front/.gitignore
front/Dockerfile

front/.がたくさんあるということはサブモジュールとして認識されていない
これは「api」ディレクトリは「root」リポジトリのサブモジュールとして扱われているため。
何故「api」ディレクトリだけサブモジュール化されたのか?→それは$ rails new を実行したときにRailsがGitの初期化も同時に行ってくれたから

```

それでは

子ディレクトリである「front」を、
親ディレクトリである「root」の
サブモジュールとして変更

## 1. 子ディレクトリの名前を変更
```
% git mv front front_cp
user@usernoAir-2 demoapp_v1 % ls
api			docker-compose.yml	front_cp
```

## 2. 「front_cp」を「front」に名前を変更し、サブモジュールとして追加
サブモジュールコマンドの実行
```
git submodule add ./front_cp front
```
なぜわざわざ「front_cp」に名前を変更したかと言うと、追加するサブモジュールの名前が既に存在する場合、エラーとなりサブモジュールを追加できないから


## 3. 追加されたサブモジュールを確認
```
% ls -a -1
.
..
.env
.git
.gitignore
api
docker-compose.yml
front_cp
```
ルートディレクトリでサブモジュールの記述を行う
```
 % git ls-files
```
