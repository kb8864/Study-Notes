# memo
[マークダウン記法チートシート](https://qiita.com/Qiita/items/c686397e4a0f4f11683d)

<details><summary>分割代入</summary>

```rb
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
