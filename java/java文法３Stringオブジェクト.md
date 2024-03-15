# Stringオブジェクト
変数の宣言では特別なstringクラスをインスタンス化する
→java.lang.string　クラス
一度作成した文字列を変更できない、変更するには新規でインスタンスを生成する
replaceAll()という置換メソッドをつかっても、一度定義した値は変更できない


## string　クラスのインスタンスの生成方法
代表的なモン

```
 string str = "abcde"; //クラスのインスタンス化を明示しない方法(""で文字リテラルを記述する）
 string str = new string("abcd"); //クラスのインスタンス化を明示する方法(newを使う)
他にもString.valueOf(“文字列”) で作成がある
```
クラスのコンストラクタを使用する時以外は、new キーワードを使わない方がメモリ効率が良い

# String型の正体＝参照型
String型の正式な生成方法は以下

`String data= new String( “データ” );`

```
String s1 = new String("sample1");
String s2 = "sample1";

String s11 = new String("sample1");
String s21 = "sample1";

System.out.println(s1 == s11); // false
System.out.println(s2 == s21); // true
System.out.println(s1 == s2); // false
System.out.println(s1.equals(s2)); // true
System.out.println(s1.equals(s11)); //true
System.out.println(s2.equals(s21)); //true

equals() メソッド=すべてのクラスが暗黙的に Object クラスを継承するため、どのクラスでも使用することができます。
```

# java.lang.Stringクラスのメソッド
charAt() ->指定された文字列から、引数で指定された位置（0から始まる整数列）にある１文字を抜き出す。範囲外の番号を指定した場合は、例外がスロー
```
String str = "abcde"; 
System.out.print(str.charAt(4)); // e
```

indexOf() ->引数で指定された文字が文字列のどの位置のものか文字に割り振られた番号がす。範囲外の場合は-1が返されるメソッド。

```
String str = "abcde";

System.out.print(str.indexOf(abcdeｆ)); // −１
System.out.println(str2.indexOf("c")); // 2
System.out.println(str3.indexOf("bc")); // 1

```

## Stringの比較する時の注意点

『==』で比較しようとすると場所情報の比較となってしまうため、それぞれが保持している文字列データが
等しいかどうかという比較はできない
比較する場合は.equalsを使用する
name1.equals( name2 )



<details>
  <summary>コード</summary>
  
```
class java2_4 {
	public static void main (String[] args) {
		
		//参照型に格納されているのは場所情報→表示したらどうなる？？
		
		int[][] rooms = {{101, 102, 103}, {201, 202, 203}, {301, 302, 303}} ;
		
		System.out.println("▼参照型の変数roomsに格納されている値");
		System.out.println("rooms：" + rooms );
		
		
		//String型変数同士で関係演算子を用いる際の注意
		
		String  name   = "ポチ" ;
		
		boolean check1 = name == args[0] ;
		System.out.println("▼『==』を用いたString型の比較");
		System.out.println("check1：" + check1 );
		
		boolean check2 = name.equals( args[0] ) ;
		System.out.println("▼『equals』を用いたString型の比較");
		System.out.println("check2：" + check2 );
		
		
		//【注意】疑似プリミティブとして生成されたString型変数は扱いがややこしい
		
		String  nameOfficial1 = new String("ポチ") ;  //正式な作られ方をしたString型変数
		String  nameOfficial2 = new String("ポチ") ;  //正式な作られ方をしたString型変数
		String  nameGizi1   = "ポチ" ;                //疑似プリミティブな作られ方をしたString型変数
		String  nameGizi2   = "ポチ" ;                //疑似プリミティブな作られ方をしたString型変数
		
		boolean check3 = nameOfficial1 == nameOfficial2 ;
		System.out.println("▼『==』を用いた比較（正式⇔正式）");
		System.out.println("check3：" + check3 );
		
		boolean check4 = nameGizi1 == nameOfficial1 ;
		System.out.println("▼『==』を用いた比較（疑似⇔正式）");
		System.out.println("check4：" + check4 );
		
		boolean check5 = nameGizi1 == nameGizi2 ;
		System.out.println("▼『==』を用いた比較（疑似⇔疑似）※注目！");
		System.out.println("check5：" + check5 );
		
	}
}


```

</details>

<details>
  <summary>コンパイル結果</summary>
  
```
 % java java2_4 ポチ
▼参照型の変数roomsに格納されている値
rooms：[[I@1dbd16a6

▼『==』を用いたString型の比較
check1：false

▼『equals』を用いたString型の比較
check2：false

▼『==』を用いた比較（正式⇔正式）
check3：false

▼『==』を用いた比較（疑似⇔正式）
check4：false

▼『==』を用いた比較（疑似⇔疑似）※注目！
⭐️check5：true
```

</details>

[java.lang.Stringクラスのメソッド](https://itstudio.co/2023/06/11/11706/#:~:text=%E3%81%A7%E3%81%AF%E3%81%82%E3%82%8A%E3%81%BE%E3%81%9B%E3%82%93%E3%80%82-,java.lang.String%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%AE%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89,-java.lang.String)
