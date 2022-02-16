- ランチャーアイコンの定義ファイルを列挙する

  ```
  echo $XDG_DATA_DIRS | tr ':' '\n' | xargs -I% find % -name '*.desktop'
  ```

  > https://specifications.freedesktop.org/menu-spec/latest/ar01s02.html


- `Fcitx`の「外観」タブがない！

    「アドオンタブ」->「UIセクション」->「クラシックユーザーインターフェースの歯車アイコン」->「テーマ」

- `dconf`の設定変更をwatchする

  ```
  dconf watch /
  ```
  
- `dconf`の設定をdumpする

  ```
  dconf dump /
  ```

- システムでインストールされたdesktopファイル（ランチャーのアイコン）を上書きする

  `~/.local/share/applications/`に同名のファイルを作成する。
  
  非表示にするには
  ```
  [Desktop Entry]
  NoDisplay=true
  ```

- デスクトップアイコンのリロード

  ```
  gtk-update-icon-cache
  ```

- ユーザディレクトリの日本語 <-> 英語

  ```
  LANG=C xdg-user-dirs-update
  LANG=ja_JP.UTF-8 xdg-user-dirs-update
  ```
