# Vue3での変更点（Vueインスタンスの生成方法,Composition APIなど）

# Vueインスタンスの生成方法
グローバルAPIを使用する方法に変更
el:~がなくなった
<details>
  <summary>Vue2</summary>
  
```
new　Vue({
el:'#app',
  ...
})

```
</details>

<details>
  <summary>Vue3</summary>
  
```
Vue.createApp({
```
)}.mount('#app')

</details>

Vue 2ではdataをオブジェクトとして宣言できていましたが、 Vue 3からは関数での宣言に変わります。
Vue 3からはfillterが利用できなくなります。 実際のところ、computedでことたりるのであまり問題はない

[はじめに](https://b1san-blog.com/post/vue/vue-3-introduction/)

Vue２：Options APIが使われていた。オブジェクトプロパティとしてdataやmethodsなどの役割ごとにまとめて記述
Vue3：dataやmethodsなどの記述がなくなる。

Vue２：<script>
Vue3：<script setup>

[【Vue 3】Composition API の基本](https://b1san-blog.com/post/vue/vue-3-composition-api/)
