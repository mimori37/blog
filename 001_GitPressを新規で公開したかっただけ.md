<title>GitPressを新規で公開したかっただけ</title>

# GitPressでブログを始める

さっさとGitPressを始めたい方向け

私がブログを開通する際に行った手順です

## 手順
### 1. Githubでリポジトリを作成
[Github公式サイト](https://github.com/) にアクセスし、ブログ用リポジトリを作成します

![新規リポジトリ作成](https://github.com/mimori37/blog/blob/main/picture/001-1.jpg)

### 2. GitPressにログイン
[https://gitpress.io/login](https://gitpress.io/login) の「Login with Github」から自身のGithubアカウントと連携します

![GitPress連携](https://github.com/mimori37/blog/blob/main/picture/001-2.png)

### 3. リポジトリを連携
Githubで作ったブログ用リポジトリを選択します

### 4. Webhookを許可
「Allow」してWebhookを許可します

### 5. ローカルリポジトリの準備
以下のコマンドを叩いてローカルリポジトリを準備します

```shell-session:cmd
$ mkdir blog 
$ cd blog
$ git init
$ git remote add origin https://github.com/mimori37/blog.git
```

### 6. Markdownファイルのコミットとプッシュ
記事になるMarkdawnを作ります
例として、以下のhello.mdを作ります
```markdawn:hello.md
# Hello

Hello, world!
```
Markdawnをコミットします
```shell-session:cmd
$ git add hello.md
$ git commit -m "hello.md added"
$ git branch -M main
```

### 6-1. 「Please tell me who you are」が返ってきた場合
続けてプッシュした際に.gitconfigがないと、以下のような応答が返ります

このような応答がされない方（正常）は「6-2. [.gitconfig]が作成済み」へスキップしてください
```shell-session:cmd
$ git push -u origin main
Author identity unknown

***Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'HOGEHOGE@HOGEsComputer.(note)')
```
configの設定を試みます

```shell-session:cmd
$ git config --global user.email "hogehoge@hoge.com"
error: could not lock config file /home/HOGE/.gitconfig: No such file or directory
$ git config --global user.name "mimori37"
error: could not lock config file /home/HOGE/.gitconfig: No such file or firectory
```
.gitconfigが消えているので、作り直します
完了したら「6-2」へ

```shell-session:cmd
$ mkdir home
$ cd home
$ mkdir HOGE
$ cd HOGE
$ git config user.name mimori37
$ git config user.email hogehoge@hoge.com
$ cd ..\..
```

### 6-2. [.gitconfig]が作成済み
コミットが成功しているときは大体以下のような応答が返っていたはずです
```shell-session:cmd
$ git commit -m "hello.md added"
[master (root-commit) xxxxxxx] hello.md added 
 1 file changed, 3 insertions(+)
 create mode xxxxxx hello.md
 ```
続けてプッシュします

```shell-session:cmd
$ git branch -M main
$ git push -u origin main
Username for 'https://github.com':    # <- mimori37
Password for 'https//mimori37@github.com':
Enumrating objects: 3, done.
Counting objects: 100% (3/3), done
Writing objects: 100% (3/3), 229bytes | 114.KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/mimori37/blog.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

### 7. 確認
ブログが完成してるはずなので、ブラウザから確認してみます
https://github.com/mimori37/blog

## 参考
開通する際に参考にした記事です
上の情報だけでは足りない方は、以下を参考にしてみてください

[GITPRESS 事始め](https://gitpress.io/u/1391/getting-started-with-gitpress)


[【git】error: failed to push some refs to "URL"のエラー対処法](https://qiita.com/chiaki-kjwr/items/118a5b3237c78d720582)


[Git Commitを実行したら、「Author identity unknown」エラー](https://yutaka-gakushu.com/tips/git/author-identity-unknown-error)


[githubからローカルにコピーするところまで](https://tenkoma.hatenablog.com/entry/20080906/1220728367)