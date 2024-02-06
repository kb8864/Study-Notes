# 最初のVue

<details>
  <summary>HTML</summary>
  
```

<html lang="ja">
CDNでVueの読み込み
 <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>

<div id="app">
イベントハンドラ  
</div>

   <script>
      new Vue({
        el: '#app', //#appというIDを持つDOM要素に紐付け
        data: {
          //アプリケーションの状態を保持する変数が含まれている。
          message: ''
        },
      });
    </script>
  </body>
</html>

```

</details>
   

<details>
  <summary>JS</summary>
  
```
const app = Vue.createApp({//Vue.createAppはインスタンスを作り変数appに入れます。
//appはapplicationの略で vmはViewModelの略です
  
})
app.mount('#app')
// '#app'でVueインスタンスのmountメソッドを呼び出しています
// これで HTMLページ内でVue.jsの世界が適用されるのは
// id=”app”を指定した要素の中ですよという指定になります
// 指定されたHTMl要素がVUeが構築したDOM置き換えられる

```

</details>

# 現在の日付とフォーマットされた料金を表示するシンプルなウェブページを作成する
- dataオブジェクトに定義されたデータを基に動的なHTMLコンテンツを生成するため、{{ }}内にページがレンダリングされるたびにHTMLに挿入される。その時のJavaScript式↓
- {{ date.getFullYear() }}：dataオブジェクト内で定義されたnew Date()によって生成された現在の日付と時間のオブジェクトを挿入。
- getFullYear(): Dateオブジェクトのメソッドで、4桁の年（例：2023）を返す
- {{ date.getMonth() + 1 }}：getMonth(): Dateオブジェクトのメソッドで、月を返しますが、0から始まるため（0は1月、1は2月...）、実際の月より1小さい値です。+ 1: 実際の月を得るために、getMonth()の結果に1を加算する
- {{ date.getDate() }}：月の日付を返します（1から31
- {{ Number(price).toLocaleString() }}：toLocaleString(): 数値を、ブラウザのロケール設定に基づいた文字列に変換するメソッドです。例えば、英語のロケールでは10,000のように数値をフォーマットし、日本語のロケールでは10,000（千区切り）と表示されます。

<details>
  <summary>HTML</summary>
  
```<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      本日は {{ date.getFullYear() }} 年 {{ date.getMonth() + 1 }} 月 {{
      date.getDate() }} 日です。<br />
      料金は {{ Number(price).toLocaleString() }} 円になります。
    </div>
    <script>
      new Vue({
        el: '#app',
        data: {
          date: new Date(), //現在の日付と時間のデータを保持するキー名
          price: 10000,
        },
      });
    </script>
  </body>
</html>

```

</details>

# 

<details>
  <summary>HTML</summary>
  
```
```

</details>



















# データバインディングとは？
データと描画を同期する仕組み
テキストのデータバインディング

DOMの更新を自動化するデータバインディングを行うには テンプレートで使用するすべてのデータはリアクティブ（＝反応的）データとして定義する必要がある。

```
JS
const app = Vue.createApp({
  data:() =>({message: 'Hello Vue/js'
    
  })
})
app.mount('#app')
//①アプリで使用するデータの宣言は、dataオプションで使用する。
//dataオプションは テンプレート側から参照できる値を格納したオブジェクト
```
```
HTML
<div id="app">
		<!--   <div></div>の中がVueの世界 -->
  <p>{{message}}</p>
</div>
<!-- データオプションに定義したmessageぷプロパティの値をブラウザで表示 -->

<!-- {{}}で囲んでいるのは、dataオプションがmessageぷプロパティの値に置き換わっている
 -->
<!-- <script src="https://unpkg.com/vue@3.1.5"></script> -->
```
# dataオプションにオブジェクトや配列要素を設定
dataオプションには カンマ区切りで複数のプロパティを定義することができます

```
JS
const app = Vue.createApp({
  data: () => ({
    message: 'Hello Vue.js!',
    count: 99,
    user: {
      lastName: 'Nakamura',
      firstName: 'Yuta',
      prefecture: 'Tokyo'
    },
    colors: ['Red', 'Green', 'Blue']
  })
})
app.mount('#app')
```
```
HTML
<div id="app">
  <p>{{message}}</p>
  <p>{{count}}</p>
  <p>{{user.prefecture}}</p>
  <p>{{colors[1]}}</p>
</div>
<!-- 
データオプションに定義したmessageぷプロパティの値をブラウザで表示 -->

<!-- {{}}で囲んでいるのは、dataオプションがmessageプロパティの値に置き換わっている
 -->




<!-- <script src="https://unpkg.com/vue@3.1.5"></script> -->
```
[Vue3の基本文法](https://qiita.com/takayuki-nakamura/items/86b763b2405384554017)

# v-bind
HTMLエレメントの要素とVueインスタンスdataとの接続に使用します。

属性へのデータバインディングには
v-bindディレクティブを使います
```
JS
const app = Vue.createApp({
  data: () => ({
    message: 'あああああああ'
  
  })
})
app.mount('#app')
```
```
HTML
<div id="app">
  <input type="text" v-bind:value="message">
</div>
<!-- value=の属性にはマスタッシュ構文は使えない。なぜなら、マスタッシュ構文はテキストコンテンツのための記法のため
-->
<!-- 属性へのデータバインディングには
v-bindディレクティブを使います
 -->
<!-- <script src="https://unpkg.com/vue@3.1.5"></script> -->
```

# v-if
条件分岐v-ifをHTML要素に加えることで条件を付け足すことができます
```
JS
const app = Vue.createApp({
    data() {
        return {
            product: 'Socks',
            inStock: false
        }
    }
})
app.mount('#app')
```
```
HTML
<div id="app">
<p v-if="inStock">In Stock</p>
<p v-else>Out of Stock</p>
</div>
<!-- <script src="https://unpkg.com/vue@3.1.5"></script> -->

→inStock: falseならOut of Stock

→inStock: falseならIn Stock

この場合、Vueインスタンスdataの`inStock`で判断している
```
