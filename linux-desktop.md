- ランチャーアイコンの定義ファイルを列挙する

  ```
  echo $XDG_DATA_DIRS | tr ':' '\n' | xargs -I% find % -name '*.desktop'
  ```

  > https://specifications.freedesktop.org/menu-spec/latest/ar01s02.html


- `Fcitx`の「外観」タブがない！

    「アドオンタブ」->「UIセクション」->「クラシックユーザーインターフェースの歯車アイコン」->「テーマ」
