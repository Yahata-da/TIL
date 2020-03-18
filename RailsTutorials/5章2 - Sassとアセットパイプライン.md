## Sassとアセットパイプライン
### ■アセットパイプライン
CSS、JavaScript、画像などの管理に有用  
アセットディレクトリ、マニフェストファイル、プリプロセッサエンジンの3つによって構成される

#### アセットディレクトリ
- app/assets: 現在のアプリケーション固有のアセット
- lib/assets: 開発チームによって作成されたライブラリ用のアセット
- vendor/assets: サードパーティのアセット

#### マニフェストファイル
Rilasの特筆すべき機能その1
アセットディレクトリに配置されたアセット(静的ファイル)を、Sprocker gemによってどのように1つのファイルにまとめるのかをRailsに指示する
CSSとJavaScriptには適用されるが画像には適用されない

アプリケーション固有のCSS用マニフェストファイルは
app/assets/stylesheets/application.cssで、

- ```*= require_tree .```はapp/assets/stylesheetsディレクトリのすべてのCSSファイルがアプリケーションＣＳＳに含まれるようにしている


- ```*= require_self```はCSSの読み込みシーケンスの中で、application.css自身もその対象に含めている。

#### プリプロセッサエンジン
まとめられた後、ファイルの拡張子によってどのプリプロセッサを使うのかをRailsが判断する

#### アセットパイプラインまとめ
最小化されていないCSSやJavaScriptを多数に分割するとページの読み込みが遅くなるので、いくらプログラマにとって都合がよくてもそれじゃあ駄目！  
↓  
開発環境は機能を個別のファイルに分割しておくプログラマにとって読みやすい状態をキープ  
本番環境ではAsset Pipelineによってファイルが最小化されるのでオッケー(すべてのCCSファイルをapplication.cssにまとめてくれるし、すべてのJavaScriptファイルをjavascripts.jsにまとめてくれる)
+おまけに不要な空白やインデントを取り除く処理を行い、ファイルサイズを最小化してくれる  

### ■Sass
スタイルシートを記述するための言語であるSass  
特徴としては

#### ネスト
##### CSSクラスのネスト
```
.center {
  text-align: center;
}

.center h1 {
  margin-bottom: 10px;
}
```
これが  
↓  
こうなるぜ！(ネストの内側にあるh1は.centerのルールを継承している)  
```
.center {
  text-align: center;
  h1 {
    margin-bottom: 10px;
  }
```
##### idのネスト 
```
#logo {
  float: left;
...
}
#logo:hover {
  color: #fff;
...
}
```
↓  
```
#logo {
  float: left;
　&:hover {
    color: #fff;
...
  }
}
```

#### 変数
カラーコードに変数名を与える
```$light-gray: #777;```  
こうしておけばその後出てくる#777を全部$light-grayに置き換えられるのでパッと見わかりやすい


### ■RailsのルートURL
#### 
get  'URL', to: 'コントローラ名#アクション名' という文法にしたがって
```get 'static_pages/help'```は
```get  '/help', to: 'static_pages#help'```となります

getルールを使うとGETリクエストが/helpに送信されたときStaticPagesコントローラのhelpアクションを呼び出す。同時に、
```
help_path -> '/help'
help_url  -> 'http://www.example.com/help'
```
上記のような名前付きルートも使える

#### テストの変更
```
  test "should get home" do
   get static_pages_home_url
  ...
  end
```

rails testを実行するとNAME ERRORで上記の get static_pages_home_urlがないですよと出るので、 get static_pages_home_url

#### 名前付きルート
getルールによって名前付きルートが使えるようになったので仮リンクの'#'を置き換えましょうね
たとえばhelpページへのリンクはhelp_pathと記述できる
```<li><%= link_to "Help",    help_path %></li>```


### ■リンクのテスト
rails generate integration_test site_layout
integration_testを生成、ファイル名はsite_layout

#### assert_templateメソッドを使って、Homeページが正しいビューを描画しているかどうか確かめる

この時生成されたにはsite_layout_test.rb内に以下を記述
```
test "layout links" do
    get root_path
    assert_template 'static_pages/home'
    assert_select "a[href=?]", root_path, count: 2
 ...
end
```
- "?"は自動的にroot_pathなどの名前付きルートに変換される
- ルートURLへのリンクは2つ置いてあるので、count: 2と書くことでリンクの個数も調べる

#### 追加したコードが通るかのテスト
- 統合テスト```rails test:integration```

### ■Usersコントローラ～ユーザー登録用URL
#### コントローラーの生成
```rails g controlller users new```することで以下のものが主に生成される
- users_controller.rb
- new.html.erb
- users_controller_test.rb

#### controllerの書き換え
users_controller_test.rbの    
```test "should get new" do```を  
```test "signup_path" do```に変更…するとtestでエラーが出る

#### routeの書き換え
routes.rbに```get  '/signup',  to: 'users#new'```を追加  
これで名前付きルートが使えるようになる。

