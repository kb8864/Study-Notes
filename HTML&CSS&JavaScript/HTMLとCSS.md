# CSS

[サニタイズCSS](https://csstools.github.io/sanitize.css/)

[CSSの優先順序](https://changeup.tech/article/css-selector/)

[よく使うCSS一覧](https://webukatu.com/wordpress/blog/6060/#CSS-6)

## divタグ
- divタグのメインの役割は、複数のタグを1つにまとめてグループ化できること
- divタグはブロックレベル要素
- 属性やcssを割り当ててデザイン変更に用いる
- そのページ内で何度も使うものはclass属性、一度しか使わないものはid属性を割り当て


[divタグの説明](https://gmotech.jp/semlabo/webmarketing/blog/div-meaning/)

[divタグにスタイル追加](https://skillhub.jp/courses/237/lessons/1877)

## 要素
marginとpaddingの違いは、外側の余白か内側の余白か

[marginとpaddingの違い](https://skillhub.jp/courses/237/lessons/1879)

## テキストフィールドにラベル
inout属性にidをつける
`<p><label for="mymail">メールアドレス</label><input id="mymail" type="email" name="mymail"></p>`

## 必須項目のラベル

![スクリーンショット 2024-03-19 16 30 06](https://github.com/kb8864/StudyNoteAndTIL/assets/128299525/83e3fb7b-499b-48d7-b9a2-d930dd090c10)

spanタグと class="required"を使う
`<span class="required">項目名</span>`

CSSには、.required {}で適応させる
（例）

<details>
  <summary>HTML</summary>
  
```
            <p><label for="mymail">メールアドレス<span class="required">必須</span></label><input id="mymail" type="email" name="mymail"></p>

```

</details>


<details>
  <summary>CSS</summary>
  
```.required {
    background-color: rgb(238, 246, 255);
    color: #f33;
    padding: 3px;
    font-size: .9em;
    font-weight: bold;
    margin-left: .3em;
}
```

</details>

[必須マーク](https://n2n-academy.com/required-mark-on-the-form/)

![スクリーンショット 2024-03-19 16 41 48](https://github.com/kb8864/StudyNoteAndTIL/assets/128299525/c5c4c680-ad44-4737-b1ed-eec6b3ef4762)

<details>
  <summary>HTML</summary>
  
```
<p><label for="mymail">メールアドレス<span class="required">必須</span></label><input id="mymail" type="email" name="mymail"></p>
 <p><label for="mymail2"><span class="required">必須</span>確認用メールアドレス</label><input id="mymail" type="email" name="mymail"></p>
```

</details>
