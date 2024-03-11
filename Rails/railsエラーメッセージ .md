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
 


 

