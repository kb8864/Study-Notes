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

