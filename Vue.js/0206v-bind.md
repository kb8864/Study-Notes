# v-bind

```
v-bind:href='プロパティ名'
これでリンクのhref属性に反映される

<p><a v-bind:href="book.url">{{book.title}}</a></p>

```

<details>
  <summary>HTML</summary>
  
```
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>
    <style type="text/css">
      ul {
        list-style-type: none;
      }
    </style>

    <title>Document</title>
  </head>
  <body>
    <div id="app" class="container">
      <ul>
        <li>
          <img
            :id="images[0].id"
            :src="images[0].src"
            class="image"
            :alt="images[0].alt"
            :width="images[0].width"
          />
        </li>
        <li>
          <img
            :id="images[1].id"
            :src="images[1].src"
            class="image"
            :alt="images[1].alt"
            :width="images[1].width"
          />
        </li>
        <li>
          <img
            :id="images[2].id"
            :src="images[2].src"
            class="image"
            :alt="images[2].alt"
            :width="images[2].width"
          />
        </li>
      </ul>

      <p><a v-bind:href="book.url">{{book.title}}</a></p>
      <p></p>
      <p><span>{{book.author}}</span></p>
    </div>
    <script>
      new Vue({
        el: '#app',
        data: {
          book: {
            title: 'vueのホームページ',
            author: '作者',
            url: 'https://ja.vuejs.org/',
          },

          images: [
            {
              id: 'image1',
              src: './img/sample1.png',
              alt: '画像1です',
              width: '200',
            },
            {
              id: 'image2',
              src: './img/sample2.png',
              alt: '画像2です',
              width: '100',
            },
            {
              id: 'image3',
              src: './img/sample3.png',
              alt: '画像3です',
              width: '50',
            },
          ],
        },
      });
    </script>
  </body>
</html>

```

</details>

<details>
  <summary>単一ファイルコンポーネント（.vueファイル）</summary>
  
```
<template>
  <div class="container">
    <ul>
      <li v-for="(image, index) in images" :key="index">
        <img
          :id="image.id"
          :src="image.src"
          class="image"
          :alt="image.alt"
          :width="image.width"
        />
      </li>
    </ul>

    <p><a :href="book.url">{{ book.title }}</a></p>
    <p>{{ book.author }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      book: {
        title: 'vueのホームページ',
        author: '作者',
        url: 'https://ja.vuejs.org/',
      },
      images: [
        {
          id: 'image1',
          src: './img/sample1.png',
          alt: '画像1です',
          width: '200',
        },
        {
          id: 'image2',
          src: './img/sample2.png',
          alt: '画像2です',
          width: '100',
        },
        {
          id: 'image3',
          src: './img/sample3.png',
          alt: '画像3です',
          width: '50',
        },
      ],
    };
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
}
</style>

```

</details>
v-forディレクティブを使って、images配列の各要素に対して<li>要素を動的に生成し,
  style scoped>タグ：コンポーネント専用のスタイルを定義します。scoped属性により、このスタイルがこのコンポーネントにのみ適用される
  
<img width="279" alt="スクリーンショット 2024-02-06 23 16 22" src="https://github.com/kb8864/Study-Notes/assets/128299525/06d567c1-6f44-4977-b928-fb75a1f1b56d">


# v-bind:class
- あらかじめ用意されたスタイルクラスを要素の割り当てるた目のディレクティブ
<details>
  <summary>HTML</summary>
  
```
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>Vue.js</title>
  </head>
  <body>
    <div id="app">
      <div class="small" v-bind:class="{ color, frame: isChange }">
        皆さん、こんにちは！
      </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>
    <script>
      new Vue({
        el: '#app',
        data: {
          color: true,
          isChange: true,
        },
      });
    </script>
  </body>
</html>
****
```

</details>

<details>
  <summary>単一ファイルコンポーネント（.vueファイル）</summary>
  
```
<template>
  <div class="small" :class="{ color, frame: isChange }">
    皆さん、こんにちは！
  </div>
</template>

<script>
export default {
  data() {
    return {
      color: true,
      isChange: true,
    };
  },
};
</script>

<style>
/* ここに必要なスタイルを追加します。 */
.color {
  /* colorクラスがtrueのときのスタイルを定義 */
  color: red ;
}

.frame {
  /* frameクラスがtrueのときのスタイルを定義 */
}

.small {
  /* smallクラスのスタイルを定義 */
}
</style>

```

</details>

<template>タグ：コンポーネントのHTMLマークアップを含みます。ここでは、class属性にバインディングを使用して、colorとisChangeデータプロパティに基づいてクラスを動的に適用していま
<style>タグ：コンポーネント専用のスタイルを定義します。必要に応じて、.color、.frame、.smallクラスのスタイルをここに追加

# v-bind:style
```
v-bind:style="{スタイル名:値,...}"
```
<details>
  <summary>HTML</summary>
  
```
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>Vue.js</title>
  </head>
  <body>
    <div id="app">
      <div
        class="small"
        v-bind:style="{ color: color, 'background-color': backgroundColor,
        fontsize: fontsize + 'px'}"
      >
        皆さん、こんにちは！
      </div>
    </div>

    <div id="app1">
      <div
        class="small"
        v-bind:style="{ color: color, 'background-color': backgroundColor,
        fontsize: fontsize + 'px'}"
      >
        皆さん、元気かい！
      </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.11/dist/vue.js"></script>
    <script>
      new Vue({
        el: '#app',
        data: {
          color: 'white',
          backgroundColor: 'blue',
          fontsize: 30,
        },
      });

      new Vue({
        el: '#app1',
        data: {
          color: 'white',
          backgroundColor: 'red',
          fontsize: 100,
        },
      });
    </script>
  </body>
</html>

```

</details>


<details>
  <summary>単一ファイルコンポーネント（.vueファイル）</summary>
  2つの異なるスタイルのテキストを表示する2つのコンポーネントを1つのファイルに統合していますが、通常はコンポーネントごとにファイルを分ける方が一般的
```
<template>
  <div>
    <div
      class="small"
      :style="{ color: color, 'background-color': backgroundColor, fontSize: fontSize + 'px' }"
    >
      皆さん、こんにちは！
    </div>

    <div
      class="large"
      :style="{ color: color1, 'background-color': backgroundColor1, fontSize: fontSize1 + 'px' }"
    >
      皆さん、元気かい！
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // コンポーネントの最初の部分のスタイル
      color: 'white',
      backgroundColor: 'blue',
      fontSize: 30,
      // コンポーネントの2番目の部分のスタイル
      color1: 'white',
      backgroundColor1: 'red',
      fontSize1: 100,
    };
  },
};
</script>

<style>
.small {
  /* smallクラスのスタイルを定義 */
}

.large {
  /* largeクラスのスタイルを定義、この例では.smallと同じスタイルを適用していますが、必要に応じて変更してください */
}
</style>

```

</details>

<img width="247" alt="スクリーンショット 2024-02-06 23 45 52" src="https://github.com/kb8864/Study-Notes/assets/128299525/9c6079f7-a38c-46d0-a4a8-72a4cccc92a8">


