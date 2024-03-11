# 1 rails c　でuser.saveができなかった
```
例えばこんな感じ



irb(main):004> user = User.new(name: "test", email: email, password: "password")
<internal:/usr/local/lib/ruby/3.2.0/rubygems/core_ext/kernel_require.rb>:38:in `require': cannot load such file -- validator/validator/email_validator (LoadError)
irb(main):005> user.save
(irb):5:in `<main>': undefined method `save' for nil:NilClass (NoMethodError)

user.save
    ^^^^^
irb(main):006> user.errors.full_messages
(irb):6:in `<main>': undefined method `errors' for nil:NilClass (NoMethodError)

user.errors.full_messages


```
これはuser.saveでのNoMethodErrorであり、
 undefined method `save' for nil:NilClass (NoMethodError)と書かれているのでuserオブジェクトがnilであるため、saveメソッドが正しく呼び出されていない
 User.newで失敗し、だからnilになっている

 user.rbの中でタイポしていないか、パスの読み込みに何か間違いがないか確認してした
 
# ２　タイポしていないのにrails cでオブジェクトが作れない
```
irb(main):001> user = User.new(name: "test", email: email, password: "password")
(irb):1:in `<main>': undefined local variable or method `email' for main:Object (NameError)

user = User.new(name: "test", ⭐️email: email, password: "password")
```

なんのことはなく、⭐️のとこにメールアドレスを定義すればいいだけ

```
# 最初にemail変数にメールアドレスを割り当てる
email = "test@example.com"

# 次に、割り当てたemail変数を使用してUserオブジェクトを作成する
user = User.new(name: "test", email: email, password: "password")

> user
+----+------+------------------+--------------------------------------------------------+-----------+-------+------------+------------+
| id | name | email            | password_digest                                        | activated | admin | created_at | updated_at |
+----+------+------------------+--------------------------------------------------------+-----------+-------+------------+------------+
|    | test | test@example.com | $2a$12$88KTVmMiy6z.DuTHkv6gMemmluKDOKyjOPL/eaSZ4PBS... | false     | false |            |            |
+----+------+------------------+--------------------------------------------------------+-----------+-------+------------+------------+
1 row in set

できた
```
