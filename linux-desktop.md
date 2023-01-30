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

- gdmのサスペンド無効化
  ```
  sudo -u gdm -g gdm dbus-launch gsettings set org.gnome.desktop.session idle-delay 0
  sudo -u gdm -g gdm dbus-launch gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-type 'nothing'
  sudo -u gdm -g gdm dbus-launch gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-timeout 0
  sudo -u gdm -g gdm dbus-launch gsettings set org.gnome.settings-daemon.plugins.power idle-dim false
  ```

- Dash to PanelやDash to Dockで中クリックで新規インスタンスが起動しない

  - nemoとかが該当する
  - desktopファイルにnew-windowアクションを追加する

  ```
  [Desktop Entry]
  Actions=new-window;open-home;open-computer;open-trash;
  ...
  
  [Desktop Action new-window]
  Name=Nemo
  Exec=nemo
  ```

  > https://github.com/home-sweet-gnome/dash-to-panel/blob/17f6fdfde3f887e1e09e29c4303371816a14e4bd/appIcons.js#L991
  > https://github.com/micheleg/dash-to-dock/blob/53114b4e000482a753e8b42dfa10d6057c08d1c6/appIcons.js#L676

- フォント設定（`fontconfig`）
  `serif`/`sans-serif`/`monospace`に対応するフォントを設定
  ```xml
  <?xml version='1.0'?>
  <!DOCTYPE fontconfig SYSTEM 'fonts.dtd'>
  <fontconfig>
      <alias>
          <family>serif</family>
          <prefer>
              <family>Noto Serif CJK JP</family>
              <family>IPAexMincho</family>
          </prefer>
      </alias>
      <alias>
          <family>sans-serif</family>
          <prefer>
              <family>Noto Sans CJK JP</family>
              <family>IPAexGothic</family>
          </prefer>
      </alias>
      <alias>
          <family>monospace</family>
          <prefer>
              <family>Fira Code</family>
              <family>Noto Sans Mono CJK JP</family>
              <family>IPAexGothic</family>
          </prefer>
      </alias>
  </fontconfig>
  ```
