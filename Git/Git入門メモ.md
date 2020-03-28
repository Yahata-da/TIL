# Git入門メモ
Progateで学んだGitのことを自分のために記しておきます
## ■まずスタート
```git init```  
- gitのリポジトリを作成  
- 叩いたディレクトリ上のファイルをgitで管理できるようになる  

## ■共有したいファイルを選択
```git add 共有したいファイル名```  
```git add -A```だとすべてのファイルをadd状態にできる  

## ■選択したファイルを記録(=コミット)する
その際、そのコミットがどんな内容か分かるようなメッセージ(=コミットメッセージ)を付ける  
```git commit -m "Create index.html"```  
index.htmlを作成したんだなと分かるですね。  

## ■ファイルをアップロード／ダウンロードできる場所(=リモート)を用意
リモートの登録  
```git remote add リモート名 リモートのURL```  
リモートの名前は一般的にはoriginであることが多い  

## ■リモートにファイルをアップロード(=プッシュ)する
```git push リモート名 master```  
リモート名はoriginが使われるのが通例のようです  

## ■リモートからファイルをダウンロード(=プル)する
```git pull リモート名 master```  

## ■把握
自分が行った変更を把握して、その変更の中で相手に共有すべき部分を選択できるようになることが大事であり、  
その把握と選択の仕方をやっていく  
```git status```

### なんもないときに```git status```
以下のようにが表示される  
```
git status
On branch master
nothing to commit, working tree clean
```
### ファイルに変更を加えた後に```git status```
変更箇所が<font color="Red">赤字</font>で表示される  
```
git status
On branch masterChanges not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)


        modified:   index.html
        modified:   stylesheet.css


no changes added to commit (use "git add" and/or "git commit -a")
```
## ■変更内容を把握
```git diff```  
変更した部分が赤字で表示される  

## ■addしたかどうか
```
git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        modified:   index.html        #addされているファイルは緑


Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)


        modified:   stylesheet.css        #addされていないファイルは赤
```
## ■コミットの確認
```
git log        #コミットを確認
git log -p    #変更内容も確認
```
※Qキーで表示終了
