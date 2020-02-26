# Rails Tutorials 1.4～ Gitによるバージョン管理 
## バージョン管理システム導入のメリット
- プロジェクトのコードの履歴を追う
- うっかり削除してしまったファイルを復旧 (ロールバック) する
- 自分の作成したコードを他の開発者と簡単に共有する
- 最初の章で作成したアプリケーションを本番サーバーへデプロイしたりする
こともできるようだ。
今回使っているAWS9ではデフォルトでgitが入っているのでインストールは今回なし  

## ファイルをgit commitするまで
#### 1.gitへの登録
```git config --global user.name "ユーザー名"```  
```git config --global user.email メールアドレス```  
で登録が可能。  
  
※なお、現在登録されているかを確認するにはそれぞれ以下のコードでOK  
```git config user.name```  
```git config user.email```  
登録されていればユーザー名やメールアドレスが表示されるし、されていなければ何も表示されない。
#### 2. アプリのルートディレクトリに移動後、新しいリポジトリの初期化
今回の場合だと**カレントディレクトリが```hello_app```である**ことを確認してから
```git init```  を実行
#### 3. 現在のディレクトリにあるファイルをすべて追加する
(.gitignoreに記載されているパターンにファイル名がマッチする場合、そのファイルは追加されない)
```git add -A```  
#### 4. ステージング (Staging) という一種の待機用リポジトリに置かれているファイルの状態を知る
```git status```
実行すると100はゆうに超えるファイル名が表示されたぞ
#### 5. リポジトリに反映(コミット)するにはcommitコマンドを使用
```git commit -m "Initialize repository"```  
-m のあとの文字列にはコミットメッセージを入れる  
どんなcommitだったか分かるようにするため。今回の場合だとリポジトリをイニシャライズしますよという意味。
#### (その他)コミットメッセージの履歴はlogコマンドで参照できる。
```git log```を叩く


## Bitbucket
#### BitbucketはGitリポジトリのホスティングと共有に特化したサイトである(GitHubのようなもの)
- ソースコード (とそのすべての変更履歴) の完全なバックアップの作成ができる
- 他の開発者との共同作業をより簡単に行うことができる

### bitbucketのアカウント取得と公開鍵の作成
https://git-scm.com/book/ja/v2/Git%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC-SSH-%E5%85%AC%E9%96%8B%E9%8D%B5%E3%81%AE%E4%BD%9C%E6%88%90
を参照しつつ、```ssh-keygen```を実行し作成

### Branch
- ブランチは基本的にはリポジトリのコピー
- ブランチ上では元のファイルを触らずに新しいコードを書くなど、自由に変更や実験を試すことができます。
- 通常、親リポジトリはmasterブランチと呼ばれ、トピックブランチ (短期間だけ使う一時的なブランチ) はcheckoutと-bフラグを使って作成できる。

```git checkout -b modify-README```  
を実行。ブランチの新規作成とそのブランチへの切り替えが同時に行われる。  
これ以降カレントディレクトリが``` ec2-user:~/environment/hello_app2 (modify-README)```のようにトピックブランチ込みで表示される
  
```git branch```  
を実行。すべてのローカルブランチを一覧表示  
トピックブランチでどんなぐっちゃぐっちゃな変更をしてしまって狂気の地獄の魔界の沙汰になってしまっても、masterブランチに影響を及ぼさない。  
masterブランチをチェックアウトしてトピックブランチを削除すれば変更が破棄できる。  

### Edit
README.mdを編集してみる。  
今この変更はトピックブランチで行われているよ  

### Commit
```git status```  
を実行し、ブランチの状態を確認  

```git commit -a -m "Improve the README file"```を実行する。 -aフラグは現存するすべてのファイル (git mvで作成したファイルも含む) への変更を一括でコミットする  
- コミットメッセージは現在形かつ命令形で書く (英語で書く場合。日本語であれば「〜を追加」などの体言止め)。  
- コミットメッセージを書くときには、そのコミットが「何をしたのか」と過去形の履歴スタイルで書くよりも「何をする」ためのものなのかを現在形かつ命令形で書く方が、後から見返したときにわかりやすくなるため。  

### Merge
さっきREADME.mdに加えていた変更はトピックブランチに対する変更なので、  
こんどはmasterブランチにこの変更を反映する(=merge)する  
```git checkout master```  
を実行し、ブランチをmasterに切り替える  
```git merge modify-README```  
を実行しmodify-READMEブランチで行った変更をmasterブランチにmergeだ！  
また、必須ではないもののトピックブランチを削除するには  
```git branch -d modify-README```  
でmodify-READMEブランチを削除できるぞ。  

### Push
```git push```でオーケー
すでにgit pushを行っているのでorigin masterは省略できる場合が多い

## 遭遇したエラー
#### ```cat ~/.ssh/id_rsa.pub```  実行時
→No such file or directory  
【原因】```ssh-keygen``` を実行した際にファイル名をid_rsa.pub以外の名前で作成してしまっていたため。  
【解決】```cat ~/.ssh/正しいファイル名.pub``` とすれば解決。

#### ```git remote add origin git@bitbucket.org:ユーザー名/hello_app.git```実行時
→fatal: remote origin already exists.  
【原因】originがすでに存在してしまっていること？  
【解決】```git remote rm origin``` でoriginを削除の上、再実行で解決。

#### ```git push -u origin --all```実行時
```
Warning: Permanently added the RSA host key for IP address '18.205.93.2' to the list of known hosts.  
Permission denied (publickey).  
fatal: Could not read from remote repository.  
  
Please make sure you have the correct access rights  
and the repository exists.
```  

【原因】  
- SSH認証の際に~/.ssh/config内にどのホストのどのユーザーにどの鍵で認証してあげるかという事が書いてないとPermission denied (publickey).になってしまう
- SSH鍵作成時に(意図せずですが)id_rsa.pub以外の名称で作ってしまったためconfigファイルが生成されていない
上記2つが原因だったようです。
【解決】touch ~/.ssh/configでconfigファイルを作成  
↓  
configファイル内に  

```Host bitbucket.org
    HostName bitbucket.org
    User bitbucketのユーザー名
    IdentityFile 秘密鍵へのパス
    Port 22
    TCPKeepAlive yes
    IdentitiesOnly yes
```  
    
を書いて保存  
↓  
```chmod 700 ~/.ssh/config```  
を実行してuserに読み取り・書き込み・実行の3つの権限総てを許可する  
↓  
```git push -u origin --all``` を実行し、成功

#### 

```git checkout master```実行時

```error: Your local changes to the following files would be overwritten by checkout:
        .bash_history
Please commit your changes or stash them before you switch branches.
Aborting``` 
【原因】
- .bash_historyに変更が加えられるのでcommitするかstash(一時的に退避)するかしてねという事ですが、.bash_histryには綱に実行したコマンドの履歴が更新されるので解決できない。そもそもmasterにチェックアウトしてトピックブランチを削除しようにもそのチェックアウトができないわけで…  
- そもそもgitのリモートリポジトリに.bash_historyが含まれてるのがおかしいわけで、そうなった原因はアプリのルートディレクトリではなくその上のenvironmentで```git add```を叩いていたから  
【解決】
→にっちもさっちも行かなくなったのでrails newからやり直しました…。



