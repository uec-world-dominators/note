# デスクトップ環境の構成要素

![](https://i.stack.imgur.com/LZGBJ.png)

## ディスプレイサーバ

実際の描画、マウス、キーボードからの入力の受取(X Window System)

## ディスプレイマネージャ

ログイン画面を表示し、セッションを開始する。(gdm)

画面描画のためログイン画面表示前にディスプレイサーバを起動させる

## ウィンドウマネージャ

ウィンドウの枠、サイズ変更などを扱う（なくても画面描画はできる。ウィンドウを動かせない）
(Mutter)

## ユーザーインターフェース

タスクバー、通知領域、時刻表示、アプリケーションランチャー
(GNOME Shell)

# その他

## freedesktop.org (XDG)

フリーなデスクトップ基盤を作る目的の団体。XDGはfreedesktop.orgの旧称。GUIアプリケーションで乱立する仕様をまとめる

- 「ドキュメント」や「ダウンロード」などのフォルダ名の定義
- フォントAPI(fontconfig)

## GTK

GUIフレームワーク(他の種類にはQt, Tk, Windows Forms, WPFなどがある)

- ウィジェット

## dbus

プロセス間通信。

## ibus

インプットメソッドフレームワーク。ブラウザやGUIアプリケーションと連携するためにdbusを使用

## dconf/gsettings

- dconfは設定管理ツール。Windowsのレジストリみたいなもの。GNOMEがよく使ってる。
- gsettingsはdconfのフロントエンドツール

# 参考文献

- https://uhoho.hatenablog.jp/entry/2020/12/24/125056
- https://xtech.nikkei.com/it/article/COLUMN/20121017/430545/
- https://qiita.com/ai56go/items/1b8bfeede2b467ac0667
- https://magcius.github.io/xplain/article/x-basics.html
- https://ja.wikipedia.org/wiki/Freedesktop.org
- https://ja.wikipedia.org/wiki/IBus
