# Dependency Injection

[今さら聞けないSpring BootのDI](https://tech-blog.yayoi-kk.co.jp/entry/2022/12/09/000000)

[依存性の注入](https://qiita.com/makiuchiatisols/items/39daaa5fcfc4d09f2868)

[SpringのDIコンテナの動作イメージ(雰囲気)を掴もう](https://qiita.com/kazuki43zoo/items/7a0e96573e930ac934ed)

## Dependency Injectionの失敗例
DIのステップはBean登録と取得
DIは使いたいオブジェクトを直接入れず外部から渡してもらうデザインパターン、DI経由で渡されるオブジェクトはBean
## DIとは依存性の注入→必要なインスタンスを。自動的に代入する
→外のオブジェクトを代入すること。（これで、、単体テストなどの時に  インスタンスの差し替えが可能になる）


## @Autowiredについて
- 代入の目印と思えばOK（引数に欲しいBeanを指定するやつ）
- 
[@Autowiredについて](https://qiita.com/yuto-hatano/items/69d01343f710117e4243)

失敗パターン
１　Beanの登録もれ→@Serviceをつける
こんなエラーが出る
```
Description:

Parameter 0 of constructor in com.example.todo.controller.task.TaskController required a bean of type 'com.example.todo.service.task.TaskService' that could not be found.
TaskServiceをspringが見つけることができない
TaskServiceをBean登録するためアノテーションをつけてあげないといいけない

Action:

Consider defining a bean of type 'com.example.todo.service.task.TaskService' in your configuration.

```

２　Beanの取得もれ→final修飾子をつける

