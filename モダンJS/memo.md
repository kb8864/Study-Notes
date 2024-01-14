# memo
[マークダウン記法チートシート](https://qiita.com/Qiita/items/c686397e4a0f4f11683d)

<details><summary>分割代入</summary>

```js
const myProfile = {
  name: '太郎 ',
  age: 20,
};
const { name, age } = myProfile;//今までは下のように変数の○○に見たいに書いていた
//const func4 = `名前は${myProfile.name}です。年齢は${myProfile.age}です。`;//修正前
const func4 = `名前は${name}です。年齢は${age}です。`;//修正後

console.log(func4);
```
</details>

<details><summary>分割代入の配列</summary>

```js
// ⭐️分割代入の配列記法
//オブジェクトの代入なので{}ではなく,[]を使う
//[]でどの配列から分割代入にするのか決める
const [name, age] = myProfile; //配列は順番しか持っていないので、分割代入では好きな変数を割り当てることができる
//上の霊では、０番目の値はnameという変数で受け取り、１番目はageで受け取る
const msg = `名前は${name}です。年齢は${age}です`;
console.log(msg);


```
</details>
