## "%tm月".formatted(today)をString.formatを使って書き換えてください

```
// 必要なクラスをインポート
/import java.time.LocalDate

// LocalDate オブジェクトを作成
LocalDate today = LocalDate.now();

// String.format を使用して今月を出力
String result = String.format("%1$tm月", today);
System.out.println(result);

```

