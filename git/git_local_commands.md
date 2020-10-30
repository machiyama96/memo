cmd+shift+vでプレビュー表示

#はじめに
##本ドキュメントの目的
知識が0の状態から、なるはやでGitを使えるようにする。
そのために、理論より実際にコマンドを打鍵することに重点を置く。
※Git初心者向けのハンズオンもどき。

##記載内容

Gitの初期設定~ローカルリポジトリへのコミットまでの流れ。
　→リモートリポジトリに関わる操作、ブランチの操作などは別の記事で(たぶん)紹介する。
##前提

- GitHubへのアカウント登録を終えていること
- [リンク(Git公式のインストール手順)]{https://git-scm.com/book/ja/v2/%E4%BD%BF%E3%81%84%E5%A7%8B%E3%82%81%E3%82%8B-Git%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB}などを参照しgitのインストールを終えていること
- Linuxファイル操作の基本的なコマンド知識があること

###その他(検証環境について)
ローカル環境:MacOS
なお、2020/10/22時点でMac歴３日のため操作が未だおぼつかない。。
Alt+Tabでウィンドウ切り替えができないと知ったときは絶望したが、Shiftltを無事にインストールし正気を取り戻した。

```
#Mac OSの状態
$sw_vers
ProductName:	Mac OS X
ProductVersion:	10.15.7
BuildVersion:	19H2

#gitのバージョン
$git --version
git version 2.28.0
```

#本文

###コメント
なお、これからの作業は、特にディレクトリ指定がない場合、上記にて作成した~/gitworkフォルダで行うものとする


##ファイル作成〜Add〜Commitまでの流れでよく使うコマンド類

###gitの管理を行わないファイルを作りたい
パスワードファイルやログファイルなどはgitの管理に適したファイルではない。
リモートリポジトリにAWSの鍵ファイルなどを転送してしまった結果、
莫大な請求がされる事件あとを絶たない。

そのような事故(事件？)を防ぐため、git管理を行わないことをおすすめする。
具体的な方法は「.gitignore」ファイルを作成、編集することである。
[.gitignore の書き方]{https://qiita.com/inabe49/items/16ee3d9d1ce68daa9fff}

```
$touch password.txt

$git add --dry-run .
add 'password.txt'

#.gitignore作成
$echo "password.txt" >>.gitignore

#password.txtがgitの管理外のファイルとなっていることを確認する
$git add --dry-run .
add '.gitignore'

```




###変更した際の差分を確認したい
- ワークツリーとローカルリポジトリの差分を確認するコマンド
- ステージングとローカルリポジトリの差分を確認する際はgit diff --stagedのオプションを付ける

```
$git diff #変更前なので出力/差分はなし

#ファイルの追記
$echo "<p>git diff</p>">>index.html 

$git diff
diff --git a/index.html b/index.html
index 159202e..346b715 100644
--- a/index.html
+++ b/index.html
@@ -1 +1,2 @@
 <h1>Hello world</h1>
+<p>git diff</p>

#Addを実行
$git add index.html 

$git diff --staged
diff --git a/index.html b/index.html
index 159202e..346b715 100644
--- a/index.html
+++ b/index.html
@@ -1 +1,2 @@
 <h1>Hello world</h1>
+<p>git diff</p>

#Commitを実行
$git commit -m "index.html -> Add:git diff"
[master 38a59bd] index.html -> Add:git diff
 1 file changed, 1 insertion(+)

$git diff ;git diff --staged #差分がなくなったため出力なし

```

###ファイルの削除をステージに記録する
- gitリポジトリから削除するとともにワークツリーのファイルを削除するgit rm
- リポジトリから削除するがワークツリーのファイルは削除しないgit rm --cached
　管理を外したい場合、.gitignore に追加すればよい

```
#ファイルの作成
$echo rmfile >>rmfile.txt 
$echo rmfile >>rmfile.txt
$git add .
$git commit -m "Created:rmfile.txt"
[master fa022c6] Created:rmfile.txt
 1 file changed, 1 insertion(+)
 create mode 100644 rmfile.txt

$git rm rmfile.txt 
rm 'rmfile.txt'
$ls
index.html	password.txt

$git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	deleted:    rmfile.txt

$git add . 
$git commit -m "Deleted:rmfile.txt"
[master fd35b1f] Deleted:rmfile.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 rmfile.txt

$git status
On branch master
nothing to commit, working tree clean

$echo "rm_cached">>rm_cached.txt
$git add .
$git commit -m "Created:rm_cached.txt"
[master 52b7373] Created:rm_cached.txt
 1 file changed, 1 insertion(+)
 create mode 100644 rm_cached.txt
$git rm --cached rm_cached.txt 
rm 'rm_cached.txt'
$ls
index.html	password.txt	rm_cached.txt
$git status -s
D  rm_cached.txt
?? rm_cached.txt
$echo "rm_cached.txt">> .gitignore
$git add .
$git commit
[master 235ae23] Deleted:rm_chached.txt / Add:.gitignore
 2 files changed, 1 insertion(+), 1 deletion(-)
 delete mode 100644 rm_cached.txt

```


#終わりに
##参考(本文などにリンクが添付されているものは除く)
 [サル先生のGit入門]{https://backlog.com/ja/git-tutorial/intro/01/}