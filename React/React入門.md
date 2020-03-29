# React入門
ProgateのReact入門コースIの学習内容

## ■Reactとは
JavaScriptのライブラリのひとつで、アプリの見た目を作ることができるもの  
(モバイル版はReact Native)

## ■文字を表示する
ほとんどHTMLっぽいがJSXというものだぞ
- App.js内のrenderメソッドreturn内に記述する。
  - return内に複数の要素があるとエラーとなるので一つの要素にまとめる。具体的には下記のように```<div>``でまとめてやるなどする
  - return内のみJSXで記述する必要がある（その外はJavaScript）
  - return内にJavaScriptを記述する場合は{}で囲む
- コメントアウトする場合は{/* */}で囲む

```
…
// JSではこのスラッシュ2本でコメントアウト
 return (
      <div>
        {/* JSXではこの記述でコメントアウト */}
        <h1>Hello World</h1>
        
        {/* JSXではこの記述でコメントアウト */}
        <p>一緒にReactを学びましょう！</p>
        
      </div>
…
```

## ■画像の表示
```<img src='画像のURL' />```のようにタグ終わりにスラッシュを書く。

## ■クリックと表示の切り替え
```<button>```タグで囲むことで文字列をボタンにすることができる
### イベント
「●●が起きたときに▲▲という処理を行う」という設定をする  
たとえば、ボタンが押されたときに画像がジャンプするなど  
```イベント名={() => { 処理 }}```という書き方

### state
ユーザーの動きに合わせて変わる値  

#### stateの定義
**オブジェクト**…```const user = {name: "おれ", age:29};```のようにnameなどのプロパティに文字列などの値をつけて管理するもの  
これを使って定義してゆく  
App.jsのconstructorの中で定義したオブジェクトがstateの初期値となる
```
constructor(props) {
    super(props);
    this.state = {name: "おいら"};    
  }
```
のような形で```this.state```に代するのだ。

#### stateの表示
```render```メソッド内で
```console.log(this.state)```をするとコンソール上に出力される
また、
```{this.state.name}```のようにプロパティの値を取得することも可能

#### stateの変更
```this.setState({プロパティ名: 変更する値})```  
とすることで、指定したプロパティに対するstateの値を変更することができる。
onClick処理に追加する例:  
"<button onClick={() => {this.setState({name:"文字列"})}}>ひつじ仙人</button>"

#### stateの変更をメソッド化
```メソッド名() { 処理 }```という記述でメソッド化し、クリックされたときにメソッドを呼び出す  
##### メソッドの定義
App.js内でメソッドを定義し、stateを変更する処理を追加
```
  handleClick(name){
    this.setState({name: name})
  }
```  
##### opnClickイベントでメソッドを呼び出す
```<button onClick={() => {this.setState({name: 'ぼく'})}}>ぼく</button>```の  
```this.setState({name: 'ぼく'})```の部分を
```this.handleClick((ぼく')```に変更できる  

## ■カウントアップ機能
「クリックと表示の切り替え」のときと同じく、イベントとstateを駆使する
### stateの定義
```
  constructor(props) {
    super(props);
    this.state = {count:0}
  }
  ```
### stateの表示
render()内で```{this.state.count}```

### stateの変更
stateのcountの値に1を足すhandleClickメソッドを定義し、ボタンを押すとそのメソッドを呼び出す(=値が+1される)ようにする
#### handleClickメソッドの定義
```
  handleClick() {
    this.setState({count:this.state.count + 1});
    
  }
```
#### onClickイベントの追加
```<button onClick = {()=>{this.handleClick()}}>+</button>```  
これで+ボタンを押したときに上記のhandleClickメソッドに変更(countに+1する)が加えられるですよ


