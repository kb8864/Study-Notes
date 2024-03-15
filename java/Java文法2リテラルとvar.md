# リテラル
リテラルとは、ソースコード中に記述する値のことで、Javaでは、整数・浮動小数点数・論理値・文字・文字列のリテラルがある

# 接頭辞、接尾辞のリテラル
## 10進数（整数値）：普通に表記
int a = 267;

## 2進数：先頭0b
int a = 0b11111111;

## 8進数 :先頭0
int a = 0413;
8進数は0〜７の数字を表すのでint a = 0867みたいなのはNG

## 16進数:先頭0x
int a = 0x10B;

## リテラルのルール
「_」はリテラルの先頭と末尾には記述できない

「_」は　記号の前後には記述できない

リテラルのルールに従ってコンパイルエラーになるものとならないもの（NG）
1．int a = 123_456_789;OK

2. int b = 5______2;OK

3. int c = _123_456_789;（NG）

4. int d = 123_456_789_;（NG）

5. float e = 3_.1415F;（NG）

6. long f = 999_999_9999L;

7. byte g = 0b0_1;

8. int h = 0_52;

9. int i = 0x_52;（NG）

## char型の変数に代入可能なもの
''で囲った文字
''で囲った¥u方始まる文字
0~65535までの数字

String型をchar型に変換する方法→「toCharArray()」メソッド

  public static void main(String[] args) {
    String str = "あいうえおかきくけこ";
    char[] c = str.toCharArray();
    for (int i = 0; i < c.length; i++) {
      System.out.print(c[i]);
    }
  }


char型をString型に変換->「ValueOf()」メソッド

  public static void main(String[] args){
    char c = 'あ';
    String s = String.valueOf(c);
    System.out.println(s);
  }
}

## var型の変数宣言の時にコンパイルエラーとなるもの
何型の変数やインスタンスを生成すればいいか判断ができない場合にエラーが起きる

変数が初期化されていない
```
var num;

```
nullで初期化する

```
// nullでは何の型なのかわからないためコンパイルエラー
var v = nul
```

複数の変数をまとめて宣言するような場合には利用できない
```
var width = 100, height = 80;
```

メソッドの戻り値

```
	
var method(){ } //←ここでコンパイルエラー
```

ラムダ式
```
var = a () -> {}
```

配列の初期化式

```
var a  = {1,2,3}型の特定ができない
```

１度varで定義された変数に、別の型の値を再代入するとエラーになる

```
var s = "hogehoge";
// sはStringなので、intなど別の型の値は代入できない
s = 123;
```

## var型の変数宣言の時にコンパイルエラーにならないのは、ダイヤモンド演算子を使場合
ダイヤモンド演算子による型推論ではもしか他情報がなければObject型の型として与えられるため、型推論を行うことができる

```
var arry new ArrayList<>();
```

[ローカル変数型推論](https://tech.pjin.jp/blog/2019/09/22/java_11_use_var_with_care/)

## そもそもvarって何？
ローカル変数の宣言時、変数の型を指定する代わりに「var」と書くことで、型の指定を省略することができる
どんなに型名が長くても「var」と書くだけで済むため、書くのが楽になる

```
// 従来の書き方
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy/MM/dd");

// varを使った書き方
var dtf = DateTimeFormatter.ofPattern("yyyy/MM/dd");
```

## varを使うべき場合
右辺でインスタンス化している場合

```
// 右辺に型名がそのまま書いてあるので、何の型か一見してわかる
var date = new Date();

var scanner = new Scanner(System.in);

var list = new ArrayList<String>();
```

右辺がリテラルである場合
```
// 右辺が文字列リテラルであるため、String型であると一見してわかる
var s = "hogehoge";
// 右辺が整数リテラルであるため、int型であると一見してわかる
var n = 123;
```

右辺で戻り値の型を容易に推測できるメソッドを呼び出している場合
```
// メソッド名が型名を含んでいるのでわかる
var br = Files.newBufferedReader(Paths.get("filename"));

// 現在日時の取得だが、LocalDateTimeのstaticメソッドなのでLocalDateTime型だろうと推測できる
var date = LocalDateTime.now();

// Calendarのインスタンスを取得するわけなので当然Calendar型だろうと推測できる
var cal = Calendar.getInstance();
```

## varの注意点
変数宣言をするとき、varによる型推論はコンパイル時つまり、Mainクラスをコンパイルする段階で型が決まる
