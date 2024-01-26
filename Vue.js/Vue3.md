# 最初のVue
```
JS
<div id="app">
  
</div>
Vueの読み込み
<!-- <script src="https://unpkg.com/vue@3.1.5"></script> -->
```
```
HTML
const app = Vue.createApp({//Vue.createAppはインスタンスを作り変数appに入れます。
//appはapplicationの略で vmはViewModelの略です
  
})
app.mount('#app')
// '#app'でVueインスタンスのmountメソッドを呼び出しています
// これで HTMLページ内でVue.jsの世界が適用されるのは
// id=”app”を指定した要素の中ですよという指定になります
// 指定されたHTMl要素がVUeが構築したDOM置き換えられる

```
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
