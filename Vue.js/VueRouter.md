# VueRouter
Vue3でSPAを作成する際に利用する記法

## ルーティングの設定
router/index.tsでルーティングの設定を記述する。

## crateRouter()
ルーティングのベースになっているのがcreateRouter()メソッドになる。
createRouter()の引数にはルーティングに関する設定であるhistoryとroutesをオブジェクトとして渡す。
historyはhistoryとhashの2種類があり、それぞれでURLの表示が変わる。
hashモードにするとURLに#が入り不自然になるため不採用として詳細の調査も行っていない。
routesはルーティング情報配列を指定するプロパティ


[参照記事](https://qiita.com/gone0021/items/6d91ae659ec6d58bce5c)
