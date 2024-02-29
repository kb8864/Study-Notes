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
