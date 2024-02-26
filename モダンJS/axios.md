# axiosとは

[axiosの基本的な使い方](https://qiita.com/s_taro/items/30114cfa370aac6c085f)
[JavaScriptでAPI通信を簡単にする方法](https://smallit.co.jp/blog/a2478/)
[axios入門](https://axios-http.com/ja/docs/intro)
[Vueとaxios](https://reffect.co.jp/vue/vue-axios-learn)


# axiosとは
axiosはPromiseベースのHTTP Clientライブラリのこと。
GETやPOSTのHTTPリクエストを使ってサーバからデータの取得、サーバへのデータ送信を通してデータの追加、更新、削除を簡単に行うことができる。

# なぜAxiosが使われるのか？

<details>
  <summary>説明</summary>
  
```
1　プロミスベースだから。
 AxiosはプロミスベースのAPIを提供する。これにより、非同期処理をより簡単に扱うことが可能。
２　ブラウザとNode.jsの両方で動作するから。
　これにより、フロントエンドとバックエンドの両方で同じHTTPクライアントライブラリを使用することが可能
３　リクエストのキャンセルができるから
　　ユーザーがページを離れるなどした場合に、未完了のHTTPリクエストをキャンセルする機能を提供できる
４　リクエストとレスポンスのインターセプト
　これにより、送信前のリクエストや受信後のレスポンスを捕捉して、データを加工したり、ログを取ったりすることが可能
５　自動的なJSONデータの変換
サーバーからのレスポンスを自動的にJSON形式に変換を行うことができる
```

</details>


# Axiosの基本的な使用方法
```
// Axiosをインポート
import axios from 'axios';

// GETリクエストを送信(外部から情報を取得する際の基本になるHTTPのリクエストメソッド。getの引数にURLを入れるだけでURLに対してGETリクエストを送ることができる。リクエスト後に戻される値はすべてresponseの中に保存される。)

axios.get('https://example.com/data')
  .then(function (response) {
    // レスポンスの処理(axiosの処理が成功した場合に処理させたいことを記述)
    console.log(response.data);
  })
  .catch(function (error) {
    // エラーの処理
    console.error(error);
  });

```
