# タイムゾーン
api/config/application.rb

```
    # Railsアプリデフォルトのタイムゾーン(default 'UTC')
    # TimeZoneList: http://api.rubyonrails.org/classes/ActiveSupport/TimeZone.html
    config.time_zone = ENV["TZ"]

```
config.time_zone ... Railsアプリのタイムゾーンを設定。デフォルトはUTC。
設定できる値は下記URLにあるオブジェクトの値で、key（Tokyo）, value（Asia/Tokyo）どちらでもOK
ENV["TZ"] ... RailsのDockerfileで設定した環境変数のタイムゾーンを取得

## データベースの読み書きに使用するタイムゾーン設定
api/config/application.rb
```
config.active_record.default_timezone = :utc
```
:local ... 地方時間=日本時間Time.localで読み込
Time.localはRubyのメソッドで、Rubyタイムゾーンを参照
```
irb(main):001> Time.local(2024,2,29,12)
=> 2024-02-29 12:00:00 +0900
irb(main):002> Time.now.zone
=> "JST"

```


```
api/config/application.rbに以下を追加
` config.time_zone = ENV["TZ"]`

```
irb(main):002> Time.current
=> Thu, 29 Feb 2024 13:10:11.828572064 JST +09:00
JSTになっているので変更完了
```

# 2データベースの読み書きに使用するタイムゾーン設定
api/config/application.rb
```
config.active_record.default_timezone = :utc

```
# 3日本語化ファイルの読み込み設定
api/config/application.rb
```
config.i18n.default_locale = :ja
```

# 4Zeitwerk（ツァイトベルク）の設定
Railsの$LOAD_PATHに自動読み込みパスを足すべきかどうかを指定
api/config/application.rb

```
    # $LOAD_PATHにautoload pathを追加しない(Zeitwerk有効時false推奨)
    config.add_autoload_paths_to_load_path = false

```
## $LOAD_PATHとは
ファイルを読み込むRubyのメソッド「load」や「require（リィクゥァィア）」が、ファイルを参照するときに使われるディレクトリパス群

## 自動読み込みパスとは
Railsが自動で読み込むディレクトリパスのこと
ここには「app」以下にある全てのサブディレクトリや、アプリが依存する可能性があるGemのパスが入ってる

Zeitwerkが有効であるかは下記コマンドで確認
```
irb(main):002> Rails.autoloaders.zeitwerk_enabled?
=> true
```
