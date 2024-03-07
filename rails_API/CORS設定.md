# CORS設定

CORSとは、Cross-Origin Resource Sharing（クロスオリジンリソースシェアリング）の略で、異なるオリジン間の通信（例えば）を許可する仕組み
同一オリジンポリシーは、不正なやりとりを防止すると同時に正当なやりとりも拒否してしまう。
その回避策として用意されているのがこのCORS

[CORS設定](https://cloud.google.com/storage/docs/cross-origin?hl=ja)

## 必要な理由
ウェブページAからウェブページBの情報を取得したい時、CORSがなければ、セキュリティの問題で情報を取得することができない。しかし、CORSが正しく設定されていれば、この情報の取得が安全に行われる。
CORSは主に、フロントエンドがバックエンドのデータにアクセスする際の制限や許可を管理する。

## オリジンの具体例

[オリジンの具体例](https://developer.mozilla.org/ja/docs/Glossary/Origin)

### オリジンとは、URLのスキーム・ホスト・ポートの３つの組み合わせの事
`http://www.exsample.com:80/index.html` というURLを例にすると、

スキーム: http://
ホスト: www.exsample.com
ポート: 80 (ポート番号は省略可)
AとBという２つのオリジンがあったとして、この3つがすべて一致したときのみ「AとBは同じオリジンである」

### 同一オリジンポリシーに違反していない例。

スキームとドメインが一致しているので同一オリジン

http://example.com/app1/index.html

http://example.com/app2/index.html

http://example.com/category/app1/index.html

### 同一オリジンポリシーに違反している。

スキームが異なるオリジン

http: //example.com/app1

https: //example.com/app2

ドメインが異なるオリジン

http://example.com

http://www.example.com

ポートが異なるオリジン

http://example.com

http://example.com:8080


# CORS設定の方法
Nuxt.js側にproxy（代理サーバ）を立てて通信を行う方法

Rails側でリクエストがくるドメインを許可する方法<=こっちがいい
これは
gem 'rack-cors'　というgemをインストールして他のフロントエンドとの通信を許可させる
root $ docker-compose build api で再ビルド


rack-corsの設定ファイルを編集
api/config/initializers/cors.rb 内のoriginsの部分に許容するドメインを指定する。
　docker-compose.ymlで指定した値を入れるため、
`origins ENV["API_DOMAIN"] || ""`を追加


docker-compose up
Nextのリクエストに答えて以前のエラーを解決できていることを確認
レスポンスの成功を表す200番も表示されている

以下のような画面になればOK


https://github.com/kb8864/Study-Notes/assets/128299525/0587d356-1b91-4fe0-9031-5cbbabd53b24





## 参考記事
[Rails APIでのCORS設定](https://qiita.com/mtoyopet/items/326ba62d485e9ef0dacd)

[CORSって何？CORSエラーが起きた時の対処方法は？](https://musclecoding.com/rails-api-cors/)
