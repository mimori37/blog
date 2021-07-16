# VSCodeでもnormal移行時に英数IMEに切り替えたいvimmerへ

長い間悩んでいたにも関わらず、案外あっさりと片付いてしまったので、ざっとご紹介

ちなみに私はVSCode-Neovimを使ってますが、この方法ならVSCode-vimでも動きそうな気がします


## 解決の糸口: zenhan
WindowsでIMEモードを切り替えるだけのシンプルなプログラムですが、これと.vimrcのnormal移行検知を組み合わせるだけでかなり便利になります
作者には足を向けて寝られません


## 実践
やることは以下の3つのみです


## zipファイルをダウンロードする
[iuchi氏の記事](https://qiita.com/iuchi/items/9ddcfb48063fc5ab626c)からzipファイルを適当な場所にダウンロードする

ちなみに私がダウンロードした時は、実行ファイル名はspzenhan.exeになっていたので、実行ファイル名が「spzenhan.exe」だったとして話を進めます


## 環境変数pathにzenhan.exeのあるフォルダのパスを追加する
アプリの検索バーに「システム」あたりまで入力すれば、
「システム環境変数の編集」項目が出ることかと思います

項目を選択したら「環境変数(n)...」をクリック

ユーザー環境変数でもシステム環境変数でもお好きな方のPathをダブルクリック
「新規(N)」を押して、spzenhan.exeのあるフォルダのパスを入力

終わったら「OK」→「OK」でウィンドウを閉じる


うまくいっていれば、コマンドプロンプト上で以下の動作が確認できるはず

```shell-session
$spzenhan 0   # 英数IMEを使用
0

$spzenhan 1   # 日本語IMEを使用
0
```

## .vimrcにノーマル復帰時の処理を書いて保存する

自分の.vimrcを開いて、以下の処理を入力

```shell-session
" ime off
if executable('spzenhan')
autocmd InsertLeave * :call system('spzenhan 0')
autocmd CmdlineLeave * :call system('spzenhan 0')
endif
```

逆にインサートモードに入った際に英数IMEに切り替えたい方は、

「if...」行から「end...」行の間に以下を挿入するのもアリかも

```shell-session
autocmd InsertEnter * :call system('spzenhan 0')
```

ちなみに('spzenhan 1')とすると、日本語IMEに切り替わります

~~厳密に言えばIMEをONにしてるだけですが~~

## VSCodeで動作確認

設定は以上

あとはVSCodeを開いて試してみてください


## 参考にしたページ
[Win版の VS Code+VSCodeVim でノーマルモードに戻った時にIMEを半角英数入力にする](https://qiita.com/iuchi/items/9ddcfb48063fc5ab626c)

[【キラーアプリ】VSCodeの新たなVim拡張はNeoVimがおすすめ！](https://wonwon-eater.com/vscode-nvim/#outline__4_1)

[Vim のカスタマイズ 〜autocmd で自動処理の実例〜](https://vimblog.hatenablog.com/entry/vimrc_autocmd_examples)