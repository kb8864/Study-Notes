# Nuxt3の状態管理

## そもそも、状態管理とは
- アプリケーションが持っている情報をどのように管理するかを決定する仕組み
- アプリケーションの規模が大きくなると、状態管理が重要になる

## Nuxt2で主要機能となっていたグローバルな状態管理ライブラリであるVuexがバンドルされておらず、Nuxt3では代わりにuseStateというComposableを使うことで簡単に状態管理できるようになった

# useStateの定義
[公式サイト](https://nuxt.com/docs/api/composables/use-state#usage)

アプリケーションの状態（state）を管理するために使う

```
useState<T>(キーの値, () => 初期値): Ref<T>
```

- ref()の代替=refはコンポーネント内の状態しか保持できないが、useSateならページ遷移しても値が保持される
（useStateの返却値はリアクティブなので、ボタンクリックで値を更新できて、処理で値を変更すればそれが画面表示にも反映される。）->{{}}を使う
- useStateは、setup()内か、Lifecycle Hooksでしか利用できない

- 値は、サーバー側のレンダリング後（クライアント側のハイドレーション中）に保持され、一意のキーを使用してすべてのコンポーネント間で共有される

```
公式
key: データの取得がリクエスト間で適切に重複排除されることを保証する一意のキー。キーを指定しない場合は、ファイルに固有のキーと のインスタンスの行番号が生成されます。useState
init: 未起動時の状態の初期値を提供する関数。この関数は を返すこともできますRef。
```


以下は値の設定→値の取得の流れ
```値の設定
<script setup>
const title = useState("title", () => "useStateの使用");
</script>
<template>
  <div>{{ title }}</div>
</template>
```

```
値の取得
<script setup>
const test = useState("test");
</script>
```

<details>
  <summary>app.vue</summary>
  
```
<template>
  <div>
    {{ title }} <br>
    <button @click="updateTitle">タイトルを更新</button>
  </div>
</template>

<script setup>


// useStateのキーの値と初期値を設定
  const title = useState('title', ()=> "useStateの使用")

  function updateTitle (){
    title.value = "更新後のタイトル"
  }

</script>
```

</details>

## 一元管理するためのcomposablesディレクトリ
- グローバルで状態を共有したい場合は、composablesディレクトリにuseStateを定義してコンポーネントでインポートして利用する。
- auto-importが行われるディレクトリ
- composables/ディレクトリは以下に設定されたファイルが自動的にインポートされる
```
composables
└── index.ts
```

[グローバルな状態のComposableで一元管理](https://zenn.dev/pistachiyoda/scraps/bb53fa5f9b6b82)

[useStateで状態管理](https://note.com/taatn0te/n/n8c3bc521b3e7)

``` index.ts
//  以下の方法でuseTitleを自動的に呼び出せる。つまりapp.vueに
// 書いたキーと初期値設定はけす

export const useTitle = () => useState('title', ()=> "useStateを使用")
```
composablesディレクトリ内に追加したuseStateをapp.vueで呼び出して利用できる

```
app.vue

<template>
  <div>
    {{ title }} <br>
    <button @click="updateInit">初期値を更新</button>
  </div>
</template>

<script setup>


// useStateのキーの値と初期値を設定
  const title = useTitle()
  
  function updateInit (){
    title.value = "更新後の値"
  }

</script>
```

## リロードしたら消えるので状態を操作するアクション処理（＝ステートの値を更新）を実装後、状態を保持して画面に表示させる
リアクティブにするならrefを使う


## 上のコードをちょっと変更

### index.ts の改善
useState はNuxt3の特定の状態管理フックであり、ここでの使用法は適切ではない。
Vue3 の ref と混在して使われるべきではない。
Vue3のコンポジションAPIを利用したカスタムフックの作成とそのフックをコンポーネントでの使用する

<details>
  <summary>index.ts </summary>
  
```
//  以下の方法でuseTitleを自動的に呼び出せる。つまりapp.vueに
// 書いたキーと初期値設定はけす

// カプセル化の強化。Vueのrefとreadonlyをインポート
import { ref, readonly } from 'vue';

export const useTitle = () => {
    //ref の使用: Vueのリアクティブシステム内でプリミティブ値をリアクティブに扱うため
      // これにより、アプリケーションのどの部分からでもこの状態を参照し、更新することが可能
      // 修正前const title = useState<string>('title', ()=> "useStateを使用した初期値")
    // refを使用してリアクティブなデータを作成
    const title = ref('useStateを使用した初期値');



  // title 状態の値を更新するために使用
  // 引数として受け取った value を title 状態に代入して更新
  const changeTitle =  (newValue: string) => {
    title.value = newValue
  }

  //useTitleで返却する値を記述
  // titleを読み取り専用にして外部からの直接変更を防ぎ、
  // changeTitle関数を通じてのみ更新を許可
  return{ 
    title: readonly(title),
    // 修正前changeTitle: changeTitle(title)
    changeTitle
  }
}


```

</details>
app.vue の変更

// 'composables' ディレクトリ内の 'index.ts' をインポート

イベントハンドリングの改善: $event => を削除し、直接 changeTitle 関数を呼び出すことで、コードの明瞭さを向上

<details>
  <summary>app.vue</summary>
  
```
<template>
  <div>
    <h1>{{ title }}</h1>
    <!-- 修正前<button @click="$event =>titleState.changeTitle('useStateを更新した')">useStateを更新</button> -->
    <!-- ボタンクリックでtitleの値を更新 -->
    <button @click="changeTitle('useStateを更新した')">useStateを更新</button>
  </div>
</template>

<script setup>

// 'composables' ディレクトリ内の 'index.ts' をインポートする。これは app.vue ファイルから相対的に composables ディレクトリ内の index.ts ファイルを見つけるため。
import { useTitle } from './composables/index';

// useStateのキーの値と初期値を設定->削除
// index.tsで記述した自動的にインポートされるcomposables/ディレクトリを呼び出す処理を記述
  // const titleState = useTitle()
  // const {title} = titleState
  // useTitleからtitleとchangeTitleを展開
const { title, changeTitle } = useTitle();



</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  
}

h1 {
  color: #e762a2;
  margin-bottom: 20px;
}

button {
  background-color: #ff3700;
  color: #655d5dba;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0ee67d;
}
</style>

```

</details>























[VueのComposition APIを使用して、Vuexのstoreのように状態を一元管理する](https://developer.mamezou-tech.com/nuxt/nuxt3-state-management/#usestate%E3%81%AE%E6%A6%82%E8%A6%81)





