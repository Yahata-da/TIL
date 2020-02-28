# 1.5 デプロイする
## デプロイとは
配置、配備といった意味
> 頻繁に本番環境にデプロイすることによって、開発サイクルでの問題を早い段階で見つけることができます。  

> 開発環境のテストを繰り返すばかりで、いつまでも本番環境にデプロイしないままだと、  
アプリケーションを公開するぎりぎりの時になって思わぬ事態に遭遇する可能性が高まります  

とのことなので、山月記っぽいというかローカル環境だけに秘めて作業してないで本番環境に出せよという感じにも似てる

##  Herokuのセットアップ
バージョン管理にgitを使っていれば、PaaS（Platform as a Service）のひとつであるHerokuが使いやすいそうだ  
手順としては
- PostgreSQLデータベースを使うために本番環境にpg gemをインストールすることで、RailsがPostgreSQLと通信できるようにする
- サポートされていないSQLiteが本番環境に導入されないようにする  

以上のために、Gemfileリストを以下のように書き換える。  
```
group :development, :test do
gem 'sqlite3', '1.3.13'
end
```
と、  
```
group :production do
  gem 'pg', '0.20.0'
end
```
を追加。pg gemを追加したことそのものやRubyバージョンを指定したことをGemfile.lockに反映させないと、本番環境へのデプロイで失敗してしまうため  
```bundle install --without production```
を実行。--without productionフラグで本番環境用以外のgemをインストールできる。  

また、この変更をcommitしておきましょうネ  
```git commit -a -m "Update Gemfile for Heroku"、```

## Herokuのインストール
- Herokuga入っているかどうかは```heroku --version```で確認
- クラウドIDEなら```source <(curl -sL https://cdn.learnenough.com/heroku_install)```を実行でHerokuをインストール。アカウントを取得してメールアドレスとパスワードを登録しておくのを忘れずに。  
- ```heroku login --interactive``` を実行しemailとパスを入力してログイン
- ```heroku keys:add``` を実行しsshキーのパスを入力して認証
- ```heroku create```を実行しHerokuサーバーにアプリケーションの実行場所を作成

## Herokuにデプロイする
これは超簡単で  
```git push heroku master```  
でOKだよ
