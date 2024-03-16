# java文法

コンパイルしてクラスファイルを作成する
`javac Main.java`

Javaのクラスファイルを指定して実行する
`java Main`

[変数宣言の記事](https://www.sejuku.net/blog/22805)
データ型名 変数名 = 値;

## キャスト
キャスト演算子を使うと一時的な型変換を行う
( 変換したい型 )変換したい変数名

## ≪文字⇔整数の型変換≫

文字列→整数：Integer.parseInt( int型として扱いたい文字列 )
```
String x = “10” ;
int y = Integer.parseInt( x ) ;
```
```

	public static void main (String[] args) {
		
		int a = 100 ;
		double b = 1.5 ;
		String c = "7" ;
		double d = 1.2 ;
		少数が整数にキャストされてる->a + b = 100+1.5==101.5ではなく101となる。-> Integer.parseIntなので"7"が数字の7になる
		
		System.out.println( Integer.parseInt( ( int )( a + b ) + c ) + d );
	

```
->1018.2

整数→文字列：String.valueOf( String型として扱いたい整数 )

```
int x = 10 ;
String y = String.valueOf( x ) ;
```

### 間違い文
<details>
  <summary>整数として使いたい</summary>
  
```
	short  calc3_1 = 7 ;                            
	String calc3_2 = "8" ;                           
	int    calc3_3 = 9 ;                            
	int    answer3 = calc3_1 + calc3_2 + calc3_3 ;   
	System.out.println( answer3 );                   

```

</details>

<details>
  <summary>正解</summary>
  
```
	short  calc3_1 = 7 ;                            
	String calc3_2 = "8" ;                           
	int    calc3_3 = 9 ;                            
	int    answer3 = calc3_1 + Integer.parseInt(calc3_2) + calc3_3 ;   
	System.out.println( answer3 );   
```

</details>
