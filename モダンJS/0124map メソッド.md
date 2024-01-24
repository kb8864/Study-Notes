# map メソッド
```
// map メソッド

const numbers = [5, 3, 4];
// それぞれの値を長さとする正方形の面積を求めたい時map メソッドを使う
// map メソッドの引数は配列の要素を入る
const squaers = numbers.map((number) => {
  return number * number;
});

console.log(squaers);
```
[map メソッド](https://it-kyujin.jp/article/detail/272/)
-  mapメソッドは元の配列を変更しません。新しい配列を作成して返します。
-  mapメソッドが生成する新しい配列は、元の配列と同じ長さです。
-  使用場面の実用例は、データの変換: ユーザーデータの配列があり、各ユーザーのフルネームのみを抽出したい場合、mapを使用して新しい配列を作成できます。

