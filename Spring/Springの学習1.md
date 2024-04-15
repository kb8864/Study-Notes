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
アプリケーションの監視、管理、デバッグなどで役立ちます。
[jsug201908_SpringBootActuator.pdf](https://github.com/kb8864/StudyNoteAndTIL/files/14976182/jsug201908_SpringBootActuator.pdf)

[Spring Boot Actuatorとは？よく使うエンドポイントや使い方](https://camp.trainocate.co.jp/magazine/about-spring-boot-actuator/)

[Spring Boot Actuatorを使って開発効率を飛躍的に向上させる方法](https://www.issoh.co.jp/tech/details/1931/)

 - 認証と認可を扱う部分
- ロールヒエラルきー。この部分の認識があいまい
[ロールヒエラルきー](https://qiita.com/opengl-8080/items/032ed0fa27a239bdc1cc)
- 
