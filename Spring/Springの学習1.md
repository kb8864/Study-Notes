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
