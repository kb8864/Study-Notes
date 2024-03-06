# 基本コマンド

```
irb(main):001> user = User.new

irb(main):003> user.save
=> false
irb(main):004> user.errors.full_messages
=> ["パスワードを入力してください", "名前を入力してください", "メールアドレスを入力してください"]
irb(main):005> I18n.t("activerecord.attributes.user")
=> {:name=>"名前", :email=>"メールアドレス", :password=>"パスワード", :activated=>"アクティブフラグ", :admin=>"管理者フラグ"}

```

## ja.ymlの中身
```
ja:
  activerecord:
    attributes:
      user:
        name: 名前
        email: メールアドレス
        password: パスワード
```

## 2パスワードが仮に小文字8文字以上じゃないよほぞんされないように設定した場合
```

irb(main):006> user.password = "ああああああああ"
=> "ああああああああ"
irb(main):007> user.password
=> "ああああああああ"
irb(main):008> user.save
=> false
irb(main):009> user.errors.full_messages
=> ["名前を入力してください", "メールアドレスを入力してください", "パスワードは半角英数字・ハイフン・アンターバーが使えます"]
こんなふうにバリデーションエラーメッセージが出てくる

```
