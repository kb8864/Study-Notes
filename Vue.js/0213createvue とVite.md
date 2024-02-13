# 開発ツールcreatevueのインストール


<details>
  <summary>注意事項</summary>
  
```
従来、VueではVueCLIと呼ばれるコマンドラインツールが提供されていましたが、現在ではメンテナンスモードの扱いとなっています。つまり、今後は新たな機能は追加されず、不具合の修正だけが行われます。新しい開発では、原則としてcreatevueを優先して利用するようにしてください。createvueは、内部的にはViteというツールをベースにしています。

```

</details>

 
# これからやるのは開発ツールのインストール
## 用語
・独自の.vueファイルを使って、コンポーネントを作成する
・開発環境なら、ブラウザ上で「.vueファイル」実行可能

# 準備
今回はnodeのバージョンは16.14.2を使う
```
% nodenv install 16.14.2

# nodenvに認識させる
% nodenv rehash
 % nodenv versions
  system
* 15.14.0 (set by /Users/user/.nodenv/version)
  16.14.2←
  16.16.0
  20.5.1
 %  nodenv local 16.14.2
```

# createvue プロジェクトの作成
```
(作業ディレクトリで（プロジェクトを作成)
npm init vue@latest

createvueをインストールするかを訊かれるので、Yes（既定）のままで先に進む
プロジェクト名を「quickvue」とするほかは、とりあえず、No（既定）のままで。あとで追加する
Vue.js - The Progressive JavaScript Framework

✔ Project name: … quick-vue
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit Testing? … No / Yes
✔ Add an End-to-End Testing Solution? › No
✔ Add ESLint for code quality? … No / Yes

Scaffolding project in /Users/user/workspace/Vue.js_practice_project/vue_cli/quick-vue...

Done. Now run:

  cd quick-vue
  npm install
  npm run dev

```

```
 quick-vue % ls
README.md	index.html	jsconfig.json	package.json	public		src		vite.config.js

/srcフォルダー（特に、その配下の/componentsフォルダー）で、Vueアプリの本体を作成、編集していく

```

# ライブラリのインストール(最初の1回だけ)
```
npm install
```

# アプリの起動
```
npm run dev

  VITE v5.1.1  ready in 3607 ms

  ➜  Local:   http://127.0.0.1:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```
# http://127.0.0.1:5173/にアクセス
成功！
![スクリーンショット 2024-02-13 17 23 42](https://github.com/kb8864/Study-Notes/assets/128299525/d9cca6d7-6623-4d01-b199-9620c4553b24)


index.html→main.js→App.vue（コンポーネント）の流れ
各ファイルの説明
<details>
  <summary>HTML</summary>
  
```
<body>
<divid="app">
<!--コンポーネントを反映する領域>

</div>

<!--b.アプリをインポートする領域>
<scripttype="module"src="/src/main.js"></script>

</body>
```

</details>

<details>
  <summary>JS</summary>
  
```
//アプリを動作するためのVueライブラリのインポート
import {createApp} from 'vue'

//アプリ本体のインポート
import App from './App.vue'

//c.Vueアプリの起動終了を管理するインスタンス
createApp(App).mount('#app')

```

</details>

# 最上位のコンポーネント→App.vueの主な構成

```
<script>要素：コンポーネント定義（JavaScript）
<template>要素：コンポーネントの見た目（HTML）
<style>要素：コンポーネントに適用するスタイル（CSS）
 ```

ブラウザが開発サーバにアクセス後にindex.htmlを受け取りindex.htmlファイルに設定されているmain.jsファイルをダウンロードして、
ブラウザ上で実行することでdiv要素の間にコンテンツが挿入
つまり、1ファイルに3つの言語をまとめることで管理がしやすくなってるのがVueの特徴。超便利。
 こんな感じのファイル構成となる
 
![スクリーンショット 2024-02-13 17 55 09](https://github.com/kb8864/Study-Notes/assets/128299525/b3af8c83-6858-4e71-af59-2671492382e0)

 HelloWorld.vue・・・HelloWorld.vueコンポーネント
 App.vue・・・メインコンポーネント

 # HelloWorldするための各コードを書き換える
インストール当初は、ごちゃごちゃと書いてあるけど、書き換える。

<details>
  <summary>一番最初のApp.vue</summary>
  
```
<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>

```

</details>

# アイコンを表示し、緑色の文字で「HelloWorld」と表示
TheWelcome コンポーネントとかいう邪魔なコンポーネントが描かれているので削除
<details>
  <summary>修正後のApp.vue</summary>
  
```
<script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
    <HelloWorld />
  </header>
</template>

<style scoped>
header {
  line-height: 1.5;
  text-align: center; /* 中央揃えに修正 */
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    flex-direction: column; /* ロゴとテキストを縦に並べる */
    align-items: center; /* 中央揃えに修正 */
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin-bottom: 2rem; /* ロゴとテキストの間隔を調整 */
  }
}
</style>

```

</details>

<details>
  <summary>他のコンポーネント</summary>
  
```
<template>
  <!--HTMLを記載するタグです-->
  <div id="app">
    <p>{{ message }}</p>
  </div>
</template>

<script>
// スクリプトを記載するタグです
  export default {
    data: function() {
      return {
        message: 'Hello World'
      }
    },
  }
</script>

<style scoped>
/** CSSを記載するタグです */
  p {
    color: green;
  }
</style>


```

</details>

<details>
  <summary>JS</summary>
  
```
import './assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
// main.js によってアプリケーションが起動され、
// App.vue がマウントされると、Vueのロゴとともに「Hello World」のメッセージが緑色の文字で表示
```

</details>

npm run devで起動
http://localhost:5173/

<img width="249" alt="スクリーンショット 2024-02-13 20 04 06" src="https://github.com/kb8864/Study-Notes/assets/128299525/7404f5f3-afd4-49ec-a92a-f89efa13f230">

# Vueのロゴを削除する
App.vue」を編集し、Vueのロゴを削除
App.vueの以下<img>タグでロゴの画像を読み込んでいるのでコメントアウト

<details>
  <summary>App.vue</summary>
  
```

<template>
  <header>
    <!-- <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" /> -->

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

</template>

```

</details>

# 簡単なリアクティブシステムの作り方
## Counter.vue コンポーネントの作成
Counter.vue ファイルは、プロジェクトの src/components ディレクトリ内に手動で作成
```
cd src/components
touch Counter.vue
```
## コンポーネントの定義
Counter.vue のコード
```
<template>
  <div>
    <h1>{{ count }}</h1>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const count = ref(0);

function increment() {
  count.value++;
}
</script>

<style>
/* ここにスタイルを追加 */
</style>
```

## Counter.vue コンポーネントを作成した後、 App.vue でこのコンポーネントを使用
App.vue は通常、プロジェクトのエントリーポイントとして機能するコンポーネント
App.vue で Counter.vue コンポーネントを使用するには、まずインポートする必要があるのでやる
App.vue の <script> タグ内に以下のコードを追加
```
import Counter from './components/Counter.vue'
```
そして、コンポーネントの使用の使用。
App.vue のテンプレート内で <Counter /> タグを追加して、コンポーネントを使用
```
<template>
  <div id="app">
    <Counter />
  </div>
</template>

<script setup>
import Counter from './components/Counter.vue'
</script>

```
修正完了したApp.vue
```
<script setup>
import Counter from './components/Counter.vue'
</script>

<template>
  <main>
    <Counter />
  </main>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
</style>


```
アプリケーションは起動時にCounter コンポーネントのみを表示

