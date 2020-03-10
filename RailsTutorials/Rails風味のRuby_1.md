# 4章
## ヘルパー
### 組み込みヘルパー
app/views/layouts/application.html.erb内に書かれている
```
    <%= stylesheet_link_tag    'application', media: 'all',
                               'data-turbolinks-track': 'reload' %>
```
これはstylesheet_link_tagを使ってapplication.cssをすべてのメディアタイプで使えるようにしてくれているもの。  
### カスタムヘルパー
前項と同じくapp/views/layouts/application.html.erb内に書かれている
```
<%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
```
この行は頁タイトルの定義に依存している。  

ビューファイルでは  
```<% provide(:title, "Home") %>```  
このように書かれていれば「Home | Ruby on Rails Tutorial Sample App」がタイトルとなるが、もしタイトルが与えられていなければ
  「| Ruby on Rails Tutorial Sample App」がタイトルとなる。でもこれ | がちょっと余計だな～  
  ↓　そこで

#### full_titleヘルパーを定義する
app/helpers/application_helper.rbに、ページごとの完全なタイトルを返すためのヘルパーを定義する

```
module ApplicationHelper

  def full_title(page_title = '')
    base_title = "Ruby on Rails Tutorial Sample App"
    if page_title.empty?
      base_title
    else
      page_title + " | " + base_title
    end
  end
end
```
意味としては「page_titleが空欄のときのfull_titleを定義しますよ」  
変数base_titleが"Ruby on Rails Tutorial Sample App"であるとして、  
page_titleが.emptyであればbase_titleである"Ruby on Rails Tutorial Sample App"を、 
そうでなければpage_title + " | " + base_titleを表示（Home | Ruby on Rails Tutorial Sample Appなど）する。  

そうするとこのようにできる  
```<title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>```

↓

```<title><%= full_title(yield(:title)) %></title>```

## 4.2 コメント～メソッド
RailsコンソールでRailsアプリケーションを対話的に操作する
```rails c```を実行  

### 変数と出力
- 変数prefectureに県名を、変数cityに市を代入してみる
```
prefecture = "福岡県"
city = "北九州市"
```
- 出力してみる
```
puts "#{prefecture}" + "#{city}"
=> 福岡県北九州市
```
- 改行ではなくタブにしてみる
```
puts "#{prefecture}" + "\t" + "#{city}"
=> 福岡県　北九州市
```
- タブの特殊文字を''で囲ってみる
```
puts "#{prefecture}" + '\t' + "#{city}"
=> 福岡県\t北九州市
```
シングルクォーテーションならばそのまま

### メッセージ
- lengthメソッド
```
"foobar".length        # 文字列に "length" というメッセージを送る
=> 6
```
lengthメソッドは文字列の文字数を返す。

- empty?メソッド
```
if "foobar".empty?
 "The string is empty"
else
 "The string is noempty"
end
=> 
```
emptyメソッドの末尾に?があるif分

- nil?メソッド
nilかどうかを調べる
例
```
>> "foo".nil?
=> false
>> "".nil?
=> false
>> nil.nil?
=> true
```

### メソッドチェーン
```
>> nil.empty?
NoMethodError: undefined method `empty?' for nil:NilClass
>> nil.to_s.empty?      # メソッドチェーンの例
=> true
```

to_sメソッドはオブジェクトを文字列に変換する。
nilオブジェクト自身はempty?メソッドには応答しないにもかかわらず、nil.to_sとすると応答する


### 後続if、後続unless
```
puts "x is not empty" if !x.empty?
puts "The string '#{string}' is nonempty." unless string.empty?
```
のように、if文やunless文が真であるならば式が実行されるという感じ

### nilは特別
オブジェクトそのものの論理値がfalseになるのはfalse自身とnilの2つのみ

### 強制的に論理値に変換
!!(bang bang)
```
>> !!nil
=> false
```


## メソッドの定義
### string_messageというメソッドを定義する。
```
def string_message(str = '')
if str.empty?
    "It's an empty string!"
else
    "The string is nonempty."
end
```  

- このときif文で引数が空白ならば"It's an empty string!"が、そうでなければ"The string is nonempty."が出力されるようにしている
- デフォルトで空の文字列を指定しているので
puts string_messageを実行する(メソッドの引数を省略する)とputs string_message("")と同義になる。

### メソッド内で最後に評価された式の値が自動的に返される
```
def string_message(str = '')
  return "It's an empty string!" if str.empty?
  return "The string is nonempty."
end
```
これは前述のメソッドと同じ結果を返す。  
(2行目if str.empty?でstrが空かどうかに応じて返す値を決める文になっている)  

とりあえずこの辺まで～

