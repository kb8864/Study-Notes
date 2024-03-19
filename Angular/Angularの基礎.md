# Angularの基礎
何かしらファイルを作る場合３セット（html,css,type Script）を作る必要がある

[1ヶ月でアンギュラー](https://qiita.com/seteen/items/87b6caf746a65b0397ee)

[公式](https://angular.jp/tutorial/first-app)

[Angularチュートリアル](https://developer.mozilla.org/ja/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Angular_getting_started)

<details>
  <summary>product-list.component.html</summary>
  
```
<h1>Products</h1>
<hr />
<h2>iPhoneリスト</h2>
<h3>在庫：{{ product.stock }}</h3>
<p>おすすめ商品は{{ product.name }}</p>
<p>{{ product.discription }}</p>
<hr />
<h2>Androidリスト</h2>
<h3>在庫：{{ product2.stock }}</h3>
<p>おすすめ商品は{{ product2.name }}</p>
<p>コメント：{{ product2.discription }}</p>

```

</details>

<details>
  <summary>product-list.component.ts</summary>
  
```
import { Component } from '@angular/core';

import { products } from '../products';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.css'],
})
export class ProductListComponent {
  products = [...products];
  product = {
    name: 'iPhone14',
    stock: 5,
    discription: 'サンプルテキスト',
  };

  product2 = {
    name: 'ギャラクシー',
    stock: 12,
    discription: '在庫残りわずか',
  };

  share() {
    window.alert('The product has been shared!');
  }
}

/*
Copyright Google LLC. All Rights Reserved.
Use of this source code is governed by an MIT-style license that
can be found in the LICENSE file at https://angular.io/license
*/

```

</details>

![スクリーンショット 2024-03-18 21 07 52](https://github.com/kb8864/Study-Notes/assets/128299525/98440004-52bc-40cb-80a9-490ba3067bb7)


## オブジェクトとその情報を配列にまとめる

```変更前
  products = [...products];
  product = {
    name: 'iPhone14',
    stock: 5,
    discription: 'サンプルテキスト',
  };

変更後
  products = [
    { name: 'iPhone14', stock: 5, discription: 'サンプルテキスト' },
  ];

html側も変更する
<h2>iPhoneリスト</h2>
<h3>在庫：{{ products[0].stock }}</h3>
<p>おすすめ商品は{{ products[0].name }}</p>
<p>{{ products[0].discription }}</p>

```

## ngforについて

[for文の例](https://angular.jp/start#%E5%95%86%E5%93%81%E3%83%AA%E3%82%B9%E3%83%88%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)

ngFor 構造ディレクティブをdivタグの中に作成

product-list.component.html
```
<h1>Products</h1>
<hr />

<h2>おすすめ商品</h2>
<div *ngFor="let product of products">
  <h3>{{ product.name }}</h3>
</div>
これで、product-list.component.tsに記載したオブジェクトを配列で使えうｒ。（今回は、　product.nameだけだけどね）
```





