# chmodコマンド覚え書き
参考
https://qiita.com/ntkgcj/items/6450e25c5564ccaa1b95

## chmocは権限設定のコマンド
```chmod 権限設定 ファイルパス```のように記述  
具体的には  
```chmod 755 text.txt```のような形
### 権限設定
それぞれ  
1つめの数字はuser  
2つめの数字はgroup  
3つめの数字はother  
に対応し、  

r = 読み取り = 4  
w = 書き込み = 2  
x = 実行 = 1  
の権限を与える  

したがって、最初の例```chmod 755 text.txt```では
```
userに7(4+2+1=rwx)
groupに5(4+1=rx)
otherに5(4+1=x)
```
の権限を与えることになる。
