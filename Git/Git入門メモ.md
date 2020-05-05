# Git入門メモ
Progateで学んだGitのことを自分のために記しておきます

## ■pushまでのざっくり手順まとめ
1. ```git init```でGitの初期化  
2. コードを書くなど、ファイルに何らかの変更が加えられる
3. ```git add 共有したいファイル名```でファイルを**選択**
4. ```git commit -m "_"```で**コミット**
5. ```git remote add リモート名 リモートのURL```で**リモートリポジトリの登録**
6. ```git push リモート名 master```で**プッシュ**  
  
↓ くわしく見ていく↓  
  
## ■```git init```でGitの初期化
```git init```  
- 叩いたディレクトリ上のファイルをgitで管理できるようになる  

## ■共有したいファイルを選択
```git add 共有したいファイル名```  
```git add -A```だとすべてのファイルをadd状態にできる  

## ■選択したファイルを記録(=コミット)する
その際、そのコミットがどんな内容か分かるようなメッセージ(=コミットメッセージ)を付ける  
```git commit -m "Create index.html"```  
index.htmlを作成したんだなと分かるですね。  

## ■ファイルをアップロード／ダウンロードできる場所(=リモート)を用意し、登録する
リモートの登録  
```git remote add リモート名 リモートのURL```  
- リモート名はoriginが使われるのが通例のようなので```git remote add origin リモートのURL```となることが多い

## ■リモートにファイルをアップロード(=プッシュ)する
```git push リモート名 master```  
- リモート名はoriginが使われるのが通例のようなので```git push origin master```となることが多い

## ■リモートからファイルをダウンロード(=プル)する
```git pull リモート名 master```  

## ■自分が行った変更を把握 
### ```git status```
ただし変更内容の把握は出来ない  

#### なんの変更も加えていない状態だと以下のように表示される  
```
git status
On branch master
nothing to commit, working tree clean
```
#### ファイルに変更を加えた後だと以下のように表示される
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
### ```git diff```  
変更内容の把握をする。 
変更前のコードが赤色、変更後のコードが緑色で表示されるので、それを見てオッケーならaddしようぜということになる。

### ```git log``` コミットの確認
```
git log        #コミットを確認
git log -p    #変更内容も確認
```
※Qキーで表示終了
