# dockerの基本コマンドは3つ
コンテナを起動する-> イメージ から コンテナを起動する コマンド`container run`
イメージを作る->Dockerfile から イメージを作成する コマンド`image build`
コンテナを操作する->コンテナ に 命令を送る コマンド`container exec`->`container run`

# Dockerコマンドの手順書が必要な時は
Docker Compose を使い詳細な命令は 全部 Yaml に書いて GitHub で共有 する

## コンテナ起動時のミス
```
docker container run \
    --publish 8080:80  \
    nginx:1.21
```
出力されたエラー
```
```
nable to find image '8080:80' locally
docker: Error response from daemon: pull access denied for 8080, repository does not exist or may require 'docker login': denied: requested access to the resource is denied.
See 'docker run --help'.
```
これは8080:80という名前のイメージをローカルで見つけることができず、さらにリモートリポジトリからのpullも拒否されている
--publish オプションの後ろ、nginx:1.21（コンテナイメージの名前とタグ）が新しい行に移動してしまっている
Dockerは8080:80をイメージ名として解釈しようとしてしまっている
正解は
```
docker container run --publish 8080:80 nginx:1.21

```
--publish オプション
