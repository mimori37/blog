# VSCodeでもnormal移行時に英数IMEに切り替えたい

長い間悩んでいたにも関わらず、案外あっさりと片付いてしまったので、ざっとご紹介<br>
ちなみに私はVSCode-Neovimを使ってますが、この方法ならVSCode-vimでも動きそうな気がします<br>
<br>

***
## 解決の糸口: zenhan
WindowsでIMEモードを切り替えるだけのシンプルなプログラムですが、これと.vimrcのnormal移行検知を組み合わせるだけでかなり便利になります<br>
作者には足を向けて寝られません<br>
<br>

***
## 実践
やることは以下の3つのみです
<br>

***
## zipファイルをダウンロードする
[iuchi氏の記事](https://qiita.com/iuchi/items/9ddcfb48063fc5ab626c)からzipファイルを適当な場所にダウンロードする<br>
ちなみに私がダウンロードした時は、実行ファイル名はspzenhan.exeになっていたので、実行ファイル名が「spzenhan.exe」だったとして話を進めます<br>
<br>

***
## 環境変数pathにzenhan.exeのあるフォルダのパスを追加する
アプリの検索バーに「システム」あたりまで入力すれば
「システム環境変数の編集」項目が出ることかと思います<br>

項目を選択したら「環境変数(n)...」をクリック<br>

ユーザー環境変数でもシステム環境変数でもお好きな方のPathをダブルクリック<br>
「新規(N)」を押して、spzenhan.exeのあるフォルダのパスを入力<br>

終わったら「OK」→「OK」でウィンドウを閉じる<br>


うまくいっていれば、コマンドプロンプト上で以下の動作が確認できるはず<br>

```shell-session
$spzenhan 0   # 英数IMEを使用
0             # IMEの変更を確認する

$spzenhan 1   # 日本語IMEを使用
0             # IMEの変更を確認する
```
<br>

***
## .vimrcにノーマル復帰時の処理を書いて保存する

自分の.vimrcを開いて、以下の処理を入力

```shell-session
" ime off
if executable('spzenhan')
autocmd InsertLeave * :call system('spzenhan 0')
autocmd CmdlineLeave * :call system('spzenhan 0')
endif
```
<br>

逆にインサートモードに入った際に英数IMEに切り替えたい方は、<br>
`if...`行から`end...`行の間に以下を挿入するのもアリかも

```shell-session
autocmd InsertEnter * :call system('spzenhan 0')
```
<br>

ちなみに('spzenhan 1')とすると、日本語IMEに切り替わります<br>
~~厳密に言えばIMEをONにしてるだけですが~~ <br>
<br>

***
## VSCodeで動作確認
設定は以上<br>
あとはVSCodeを開いて試してみてください<br>
<br>

***
## 参考にしたページ
[Win版の VS Code+VSCodeVim でノーマルモードに戻った時にIMEを半角英数入力にする](https://qiita.com/iuchi/items/9ddcfb48063fc5ab626c)<br>

[【キラーアプリ】VSCodeの新たなVim拡張はNeoVimがおすすめ！](https://wonwon-eater.com/vscode-nvim/#outline__4_1)<br>

[Vim のカスタマイズ 〜autocmd で自動処理の実例〜](https://vimblog.hatenablog.com/entry/vimrc_autocmd_examples)<br>