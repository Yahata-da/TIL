# 5章 レイアウト
## 構造の追加
- カスタムCSSルール
- Bootstrapフレームワークを組み込み
以上の2つを主にやっていく

### アプリケーションビューに記述を追加
#### HTML宣言
app/views/layouts/application.html.erb内で```<!DOCTYPE html>```と書いてHTML5であることを宣言するが、ブラウザのバージョンが古い場合に備えて下記のJavaScriptを記述しておく
```
<!--[if lt IE 9]>
  <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
  </script>
<![endif]-->
```
#### リンクを貼るlink_toヘルパー
埋め込みRubyコードを適用するために<%= %>で囲む
```<%= link_to "sample app", '#', id: "logo" %>```
- 第1引数はリンクテキスト
- 第2引数はURL(仮で#を置く)
```<li><a href="#">sample app</a></li>```みたいな意味になる

#### ナビゲーションリンク
navタグはその内側がナビゲーションリンクであることを示す  
- ulタグは順不同のリスト
- liタグはリストアイテム

#### image_tagヘルパー
引数として画像ファイルのパスと任意のオプションハッシュをとる。  
app/assets/images/ディレクトリに画像(この場合はrails.png)を置いておけばOK  
(例)
```
<%= link_to image_tag("rails.png", alt: "Rails logo"),
            'http://rubyonrails.org/' %>
```

### 実際にブラウザで見てみる
- src属性には "images" というディレクトリ名が含まれていない
 - Railsはassetsディレクトリ直下の画像をapp/assets/imagesディレクトリにある画像と紐付けてくれる。
  - ブラウザから見るとすべてのファイルが同じディレクトリにあるように見える
  - フラットなディレクトリ構成によって、ファイルをより高速にブラウザに渡すことができるようになる
- alt属性
 - 画像がない場合に代わりに表示される文字列で、スクリーンリーダーを使う場合にこれが読み上げられ、例えば視覚障害のある人はそこに画像がある事が分かる

## BootstrapとカスタムCSS

### Bootstrapで読み込まれるsassを利用できるようにする
gem 'bootstrap-sass', '3.3.7'をgemfileに追加  
### CSSを作成
```touch app/assets/stylesheets/custom.scss```  
拡張子がscssになってるのが重要。
### @importでBootstrap (とそれに関連するSprockets) を読み込む
app/assets/stylesheets/custom.scss内に記述
- ```@import "bootstrap-sprockets";```  
- ```@import "bootstrap";```  

#### レイアウト
```
body {
  padding-top: 60px;
}
```
ページ上部に60ピクセルの余白を追加
```
.center {
  text-align: center;
}
```

#### 画像を非表示にする方法
- 埋め込みRubyのコードをコメントアウトする
```<%#= image_tag("kitten.jpg", alt: "Kitten") %>```
ページのソースを表示するとソースコードも消えている

- すべての画像を非表示にするCSS
```
img {
  display: none;
}
```
ソースコードは残る。  

## パーシャル
app/views/layouts/application.html.erb内のコードを少しでも綺麗に、少ない行数で表したい！

### 古いIE用のJS
最初に書いた以下のコードについて
```
    <!--[if lt IE 9]>
      <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
      </script>
    <![endif]-->
```
app/views/layouts/_shim.html.erbというパーシャルを作成して上記のコードを転記し、
application.html.erb内では```<%= render 'layouts/shim' %>```に置換  

### ヘッダーとフッター
```
    <header class="navbar navbar-fixed-top navbar-inverse">
      <div class="container">
        <%= link_to "sample app", '#', id: "logo" %>
        <nav>
          <ul class="nav navbar-nav navbar-right">
            <li><%= link_to "Home",   '#' %></li>
            <li><%= link_to "Help",   '#' %></li>
            <li><%= link_to "Log in", '#' %></li>
          </ul>
        </nav>
      </div>
    </header>
```

apapp/views/layouts/_haeder.html.erbというパーシャルを作成して上記のコードを転記し、  
plication.html.erb内では```<%= render 'layouts/header' %>```に置換  
