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
##Gitについて
###そもそもGitとは？
バージョン管理を目的としたツール。
使いこなすことで、以下の点が明確になるため、開発を行う際の混乱を防ぐことができる。
また、バージョンを戻したりすることも可能になる。
- ファイルの最新版はどれか 
-  誰が、いつ、どのような更新をしたのか

###Gitの用語と概念
本記事の前提知識として最低限必要な用語のみを記載する。
「なるはやでGitを使う」ために、詳細で正確な内容は各自インプットいただきたい。

ローカルリポジトリ
　ファイルの履歴を貯蔵する領域
ステージング
　ワークツリーの変更内容をローカルリポジトリに登録する内容を準備するための領域
ワークツリー
　作業を行うディレクトリ


##初期設定
- git config XXX コマンドを利用することで設定の登録を行う
- ホームディレクトリに作成される.gitconfigの中に設定が記載される
- 最低限必要なのは、GitHubに登録したユーザ名とメールアドレス
```
#手順として使い回しやすいように設定項目を変数に格納している

#ユーザ
$read USR
$git config --global user.name $USR

#メールアドレス
$read MAIL
$git config --global user.email $MAIL

#設定確認
$git config --list
user.name=ユーザ名
user.email=メールアドレス

```
コマンド実行後には~/.gitconfigが作成されている。
```
$cat ~/.gitconfig
(個人情報などが記載されているため内容は非表示) 
```
git config XXX　のコマンドで、様々な設定が可能。
以下のリンクや様々なサイトを参考にして設定することで効率が上がるので、ぜひとも設定することをおすすめする。
[Gitを使い始めたら一番最初にやりたい git config設定メモ]{https://blog.katsubemakito.net/git/git-config-1st}



##ローカルリポジトリの作成
- git initコマンドにて作成を行う
- .gitディレクトリが保存される
-

```
#作業用ディレクトリの作成と移動
$mkdir ~/gitwork && cd $_ && pwd 
~/gitwork
#ローカルリポジトリの作成
$git init
Initialized empty Git repository in ~/gitwork/.git/
#作成確認
$ls -A
.git
```

###コメント
なお、これからの作業は、特にディレクトリ指定がない場合、上記にて作成した~/gitworkフォルダで行うものとする

##基本的なコミットまでの流れ
以下の流れを必要とする。

1. ワークツリーでのファイル編集
1. ステージングに追加(Add)
1. ローカルリポジトリに登録(Commit)


###1.ワークツリーでのファイル編集
特記事項なし。
gitの管理下にあるディレクトリ(git initを行ったディレクトリ)で作業を行う。
```
$echo "<h1>Hello world</h1>" > index.html

$ls
index.html

$cat index.html 
<h1>Hello world</h1>
```


###2.ステージングに追加(Add)
- git add ファイル名　のコマンドで行う
- git add . で、現在のディレクトリのファイルをすべて登録する
```
#WorktreeとStagingの差分確認
$git status

On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	index.html
nothing added to commit but untracked files present (use "git add" to track)


#git addコマンドを実行した際にステージングに登録されるファイルを確認
$git add --dry-run .

add 'index.html

#Stagingに追加
$git add index.html

#Stagingに登録されたファイルを確認
$git ls-files
index.html

#
$git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   index.html
```


###3.ローカルリポジトリに登録(Commit)
- git commitコマンドにて実行する(git configで登録したエディタが立ち上がる)
- git commit -m "XXXXX"のコマンドを利用することで、エディターを立ち上げることなくメッセージの記載が可能

```
#StagingとLocal Repoの差分を確認する
$git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   index.html

#Stagingの内容をLocal Repoに登録する（コミット）
$git commit -m "Created:index.html" 
[master (root-commit) 3683311] Created:index.html
 1 file changed, 1 insertion(+)
 create mode 100644 index.html

#直前にコミットした内容を確認する
#git showでも情報取得可能　
$git log -1 

commit 3683311f3172a54981bd13ba2f841341b77985a7
Author: (個人情報流出防止の為編集)
Date:   Thu Oct 22 23:40:59 2020 +0900

    Created:index.html
```
gitをインストールして初のコミットの場合はメールアドレスとユーザ名の確認がされたような気も。。。
(もし、求められたら打鍵すること)
```
#git statusを再実行して、意図せぬ差異が生じていないことを確認する。
$git status
On branch master
nothing to commit, working tree clean

```
###git statusコマンドについて
git status -sオプションをつけると出力が短くなる。
複数ファイルの編集を行うと出力が長くなるため使用すると便利
[git status -sでちょっと幸せになれる]{https://qiita.com/tommy_aka_jps/items/af536a7c20747f99aa42}


#終わりに
##参考(本文などにリンクが添付されているものは除く)
 [サル先生のGit入門]{https://backlog.com/ja/git-tutorial/intro/01/}