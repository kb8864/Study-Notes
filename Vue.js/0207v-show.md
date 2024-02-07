# v-show
- 条件が成立したら要素を表示する
- 表示と非表示を頻繁に切り替えるコンテンツにはv-showディレクティブを使う
- 要素はDOMに出力される
- v-else、v-else-ifと組み合わせはできない
- 

<details>
  <summary>HTML</summary>
  
```
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>ダンスチーム検索・登録アプリ</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <button @click="showRegistrationForm = !showRegistrationForm">
        登録フォームを表示
      </button>
      <button @click="showSearchResults = !showSearchResults">
        検索結果を表示
      </button>

      <div v-show="showRegistrationForm">
        <h2>ダンスチーム登録</h2>
        <!-- 登録フォームの内容 -->
        <p>フォームをここに配置</p>
        <textarea v-model="memo" cols="30" rows="10"></textarea>
        <div v-bind:inner-html.prop="memo | nl2br"></div>
      </div>

      <div v-show="showSearchResults">
        <h2>検索結果</h2>
        <!-- 検索結果の表示 -->
        <p>検索結果をここに配置</p>
      </div>
    </div>

    <script>
      Vue.filter('nl2br', function (value) {
        if (typeof value !== 'string') {
          return value;
        }
        return value.replace(/\r?\n/g, '<br />');
      });

      new Vue({
        el: '#app',
        data: {
          showRegistrationForm: false,
          showSearchResults: false,
        },
      });
    </script>
  </body>
</html>

```

</details>

https://github.com/kb8864/Study-Notes/assets/128299525/a19367d3-2bd1-477f-80df-47235a89c60f

<details>
  <summary>HTML</summary>
  
```
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>sample</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <button @click="isActive = !isActive">切り替え</button>
      <div v-show="isActive" :style="{color: 'red'}">アクティブです!</div>
      <div v-show="!isActive" :style="{color: 'gray'}">...非アクティブ...</div>
    </div>
    <script>
      new Vue({
        el: '#app',
        data: {
          isActive: true,
        },
      });
    </script>
  </body>
</html>

```

</details>


https://github.com/kb8864/Study-Notes/assets/128299525/c7689fad-64b1-4121-89ad-8932a9a733fb

<details>
  <summary>HTML</summary>
  
```
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>sample</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <button class="button" v-on:click="toggle">切り替え</button>
      <!-- <button @click="isActive = !isActive">切り替え</button> -->
      <div v-show="isActive" :style="{color: 'red'}">アクティブです!</div>
      <div v-show="!isActive" :style="{color: 'gray'}">...非アクティブ...</div>
    </div>
    <script>
      new Vue({
        el: '#app',
        data: {
          isActive: true,
        },

        methods: {
          toggle: function () {
            this.isActive = !this.isActive;
          },
        },
      });
    </script>
  </body>
</html>

```

</details>


https://github.com/kb8864/Study-Notes/assets/128299525/a68f0d10-0e96-4754-b218-cf0b6dba75b2

# 特定の条件下でHTML要素の表示と非表示を行ってみた
- v-showディレクティブを各ボタン要素に適用して、isActiveがtrueの時のみ表示されるように変更
- isActiveがfalseの時は3つのボタンが非表示になる
- Vueインスタンスのdataオブジェクト内で定義されているisActiveというデータプロパティを使用して、条件を設定する
- v-showディレクティブは指定された条件が真（true）の場合にのみ、HTML要素を表示するので、trueの時に表示するボタンにv-show="isActive"をつけてあげる

<details>
  <summary>HTML</summary>
  
```
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>sample</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <button class="button" v-on:click="toggle">ボタン表示</button>
      <div v-show="isActive" :style="{color: 'red'}">ボタン表示中!</div>
      <div v-show="!isActive" :style="{color: 'gray'}"></div>
      <button class="button" v-on:click="logging('hoge')" v-show="isActive">
        hoge
      </button>
      <button class="button" v-on:click="logging('huga')" v-show="isActive">
        fuga
      </button>
      <button class="button" v-on:click="logging('pega')" v-show="isActive">
        pega
      </button>
    </div>
    <script>
      new Vue({
        el: '#app',
        data: {
          isActive: true,
        },

        methods: {
          toggle: function () {
            this.isActive = !this.isActive;
          },
          logging: function (param) {
            console.log('入力引数は' + param + 'です');
          },
        },
      });
    </script>
  </body>
</html>

```

</details>

https://github.com/kb8864/Study-Notes/assets/128299525/0ed850eb-d503-445a-887c-3734c39db057


