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


