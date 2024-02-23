# props
## props: 親から子にデータを渡すこと

## 親コンポーネントの書き方
親コンポーネントのテンプレート内で、子コンポーネントのカスタム属性に、変数を渡す
```
<template>
  <div>
    ⭐️<Child :title="foo" :count="bar" />
⭐️:title="foo" は子コンポーネントの title というプロパティに foo 変数の中身（下の例の場合は abc）を渡す、という意味
  </div>
</template>

<script>
import { ref } from '@vue/composition-api'
import Child from './child.vue'

export default {
  components: { Child },
  setup () {
    const foo = ref('abc')
    const bar = ref(123)

    return { foo, bar }
  }
}
</script>
```

## 子コンポーネントで親からのデータを受け取る方法
一番シンプルなのは、コンポーネントオプションの props にプロパティ名の配列を指定する方法
```
<template>
  <div>
    title: {{ title }}<br />
    count: {{ count }}<br />
  </div>
</template>

<script>
export default {
  ⭐️props: ['title', 'count'] // プロパティ名の配列
  ⭐️別解：どのような値が渡ってくるのか分かりづらい・・・ので以下のようにするのもあり
  props: {
    title: {         // プロパティ名
      type: String,  // 型
      required: true // 必須かどうか
    },
    count: {
      type: Number,
      default: 0     // 親から値が渡されなければ 0 になる
    }
  }

}
</script>
```

## setup 関数で props を使う
setup 関数では、ひとつめの引数で props を取得できる
これは親コンポーネントで値が変わったら props にも反映される、というリアクティブな性質をもつオブジェクトであるという理解でOK
注意点は、setup関数内で使用する場合は computed や watch で監視する必要がある。



```
import { computed } from '@vue/composition-api'

export default {
  props: {
    title: {
      type: String,
      required: true
    },
    count: {
      type: Number,
      default: 0
    }
  },
⭐️  setup (props) {
    const doubleCount = computed(() => props.count * 2)

    return { doubleCount }
  }
}

```


## 親子のコンポーネントができたら子から親にイベントを発生させてデータを渡す。
props のように直接データを渡す方法は用意されていないので、イベントを通じてデータを送る（emitを使う）
### 子コンポーネントでイベントを発生させる方法は以下の通り。
setup 関数のふたつめの引数に context というオブジェクトが渡す
```

<template>
  <div>
    <button @click="handleClick">Click me!</button>
  </div>
</template>

<script>
export default {
  ⭐️setup (props, context) {
    const handleClick = () => {
      ⭕️context.emit('my-event')
    }

    return { handleClick }
  }
}
</script>
⭕️context.emit(イベント名) を実行することで、カスタムイベントを発生させることが可能

```

### 親コンポーネントで子のイベントを受け取る
 v-on ディレクティブを使う
 ```
<template>
  <div>
    <Child @my-event="handleEvent" />
  </div>
</template>

<script lang="ts">
import Child from './child.vue'

export default {
  components: { Child },
  setup () {
    const handleEvent = () => {
      alert('イベント発生！')
    }

    return { handleEvent }
  }
}
</script>

```

## 子に渡した親の変数を書き換えたい場合はイベントを使う
この時の注意点は、子コンポーネントの中で props の中身を直接変更してはいけない
子コンポーネントでイベントを発生させる→
その引数を（親コンポーネントの）イベントハンドラ内で代入する、という手順を踏むことで親コンポーネントの変数の値を変更
`子コンポーネント`
```
export default {
  props: {
    count: Number
  },
  setup (props, { emit }) {
    ⭐️// ボタンのイベントハンドラ
    const handleClick = () => {
      emit('my-event', props.count + 1)
    }

    return { handleClick }
  }
}

```
`親コンポーネント`

<template>
  <div>
    <Child :count="num" @my-event="num = $event" />
      </div>
</template>

<script>
import { ref } from '@vue/composition-api'
import Child from './child.vue'

export default {
  components: { Child },
  setup () {
    const num = ref(0)

    // イベントハンドラ内で num の値を更新
    const handleEvent = (newVal) => {
      num.value = newVal
    }

    return { num, handleEvent }
  }
}
</script>
