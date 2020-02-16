# Rails Tutorials 1.4～ Gitによるバージョン管理 
## バージョン管理システムを導入のメリット
- プロジェクトのコードの履歴を追う
- うっかり削除してしまったファイルを復旧 (ロールバック) する
- 自分の作成したコードを他の開発者と簡単に共有する
- 最初の章で作成したアプリケーションを本番サーバーへデプロイしたりする
こともできるようだ。
今回使っているAWS9ではデフォルトでgitが入っているのでインストールは今回なし  

## ファイルをgit commitするまで
#### 1.アプリのルートディレクトリに移動後、新しいリポジトリの初期化
```git init```
#### 2.現在のディレクトリにあるファイルをすべて追加する
(.gitignoreに記載されているパターンにファイル名がマッチする場合、そのファイルは追加されない)
```git add -A```
3.ステージング (Staging) という一種の待機用リポジトリに置かれているファイルの状態を知る
```git status```
実行すると100はゆうに超えるファイル名が表示されたぞ
#### 4.リポジトリに反映(コミット)するにはcommitコマンドを使用
```git commit -m "Initialize repository"```  
-m のあとの文字列にはコミットメッセージを入れる
#### 5.コミットメッセージの履歴はlogコマンドで参照できる。
```git log```


## Bitbucket
#### BitbucketはGitリポジトリのホスティングと共有に特化したサイトである(GitHubのようなもの)
- ソースコード (とそのすべての変更履歴) の完全なバックアップの作成ができる
- 他の開発者との共同作業をより簡単に行うことができる

#### bitbucketのアカウント取得と公開鍵の作成
https://git-scm.com/book/ja/v2/Git%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC-SSH-%E5%85%AC%E9%96%8B%E9%8D%B5%E3%81%AE%E4%BD%9C%E6%88%90
を参照する。
## 遭遇したエラー(現在未解決)
```cat ~/.ssh/id_rsa.pub```  
→No such file or directory  
```git remote add origin git@bitbucket.org:ユーザー名/hello_app.git```  
→fatal: remote origin already exists.  
```git push -u origin --all```  
→Warning: Permanently added the RSA host key for IP address '18.205.93.2' to the list of known hosts.  
Permission denied (publickey).  
fatal: Could not read from remote repository.  
  
Please make sure you have the correct access rights  
and the repository exists.  

これらはまだ解決してないです…
