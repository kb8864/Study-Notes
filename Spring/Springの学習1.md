# Springの学習1

コンテナ=インスタンスの入れ物
Bean=コンテナで管理されたインスタンス

[本当のSpring入門.pdf](https://github.com/kb8864/StudyNoteAndTIL/files/14975155/Spring.pdf)

## そもそもBeanとは？
[Beanの簡単な説明](https://ti-tomo-knowledge.hatenablog.com/entry/2018/06/08/204447)

[つけないといけない理由](https://www.greptips.com/posts/1318/)

## Bean定義方法1 コンポーネントスキャン
コンポーネントスキャン  =指定されたパッケージから@Componentが付いたクラスを探す
 見つけたらインスタンス化→コンテナに保存

 ちなみに@ComponentがないとBeanにならない
 - Java Config(DIコンテナに読み込ませるコンフィグレーションをjavのクラスに記述するためのSprnngの機能)
 - 指定したパッケージのサブパッケージ以下も含む
 - Java Configクラスにメソッドを作成し、  @Beanを付加する 
→ メソッドの戻り値がBeanになる


 ## コンテナを作る・Beanを取得する
▸ Java Configクラスを指定して  ApplicationContext (=コンテナ)を作成
コンポーネントスキャン・Java Configどちらでも共通 ▸ getBean()でBeanを取得できる


# SpringのMVC
[図解で学ぶSpringMVC_20190828_訂正版.pdf](https://github.com/kb8864/StudyNoteAndTIL/files/14975646/SpringMVC_20190828_.pdf)

[基本概念：Spring MVCにおける設計のルール](https://sites.google.com/site/soracane/springnitsuite/spring-mvc/02-ji-ben-gai-nian-spring-mvcniokeru-she-jinoruru)

[Spring MVC についてまとめてみるよ！！！](https://qiita.com/PonPon3/items/76318ab3524c43630761)




# Spring Boot Actuator入門
認証、認可、セッション管理機能（Spring Securityでは、ログイン時にセッション IDという証明書のようなものが発行されます。これをAuthentication インターフェースというサーバー側の機能と、ブラウザ側の両方が持つことでセッションの整合性を保ちます。）で使う
実行中のSpring Boot アプリケーションの情報を取得するための機能と考えればいい。
ここを学習する前にいろんなエンドポイントがあることを頭に入れる。
アプリケーションの監視、管理、デバッグなどで役立ちます。

[5分で解決](https://camp.trainocate.co.jp/magazine/spring-security/)

各クラスについて
- FilterChainProxy(リクエストを最初に受け取るクラスであり、Webアプリケーションとユーザーとの処理の全体を制御)
- SecurityFilterChainこのクラスのおかげでリクエスト毎に異なるセキュリティ機能を適用することが可能)
- HttpFirewall

### 認可について

### 認証について、
ログインは、Authentication オブジェクトの作成。
ログイン済みかどうかは、Authentication オブジェクトがセッションに保持されているかをチェックすることで確認していると覚えておく

### 基本的な使いかた
WebSecurityConfigurerAdapterクラスを継承して定義を行い、既存のコントローラに何も記述せずにセキュリティ機能が追加でき容易する
[Spring BootでWebセキュリティを設定しよ](https://codezine.jp/article/detail/11703)

### Springの開発でで使用する場面
実際に読み込まれたプロパティが分からない→→env

Controllerのどのメソッドがどのエンドポイント
に対応しているか分からない⇒ mappiｇエンドポイントでControllerのメソッドとの対応関係を一覧で取得できるようにする

ログレベルを変えるのにいちいち再起動するのが面倒だ
⇒ loggers

[jsug201908_SpringBootActuator.pdf](https://github.com/kb8864/StudyNoteAndTIL/files/14976182/jsug201908_SpringBootActuator.pdf)

[Spring Boot Actuatorとは？よく使うエンドポイントや使い方](https://camp.trainocate.co.jp/magazine/about-spring-boot-actuator/)

[Spring Boot Actuatorを使って開発効率を飛躍的に向上させる方法](https://www.issoh.co.jp/tech/details/1931/)

[Spring Securityでログイン機能](https://qiita.com/yosuke_takeuchi/items/93f9155b5a4fa1976247#:~:text=%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3%E5%87%A6%E7%90%86%E3%81%B8%E3%81%AE%E9%81%93%E3%81%AE%E3%82%8A&text=%E3%83%BBSecurity%E3%81%A7%E6%8F%90%E4%BE%9B%E3%81%97%E3%81%A6,%E3%81%8C%E3%82%B5%E3%83%9D%E3%83%BC%E3%83%88%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%82%8B%E3%80%82)
