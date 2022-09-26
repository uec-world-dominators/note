## (ssh越しに)userモードのsystemctlを使う

```
XDG_RUNTIME_DIR=/run/user/$(id -u) systemctl --user status hoge.service
```

> XDG_RUNTIME_DIRを指定しないとエラー`Failed to connect to bus: No medium found`

## shebang

```
#!/bin/ls -l
```

- 実行ファイルフォーマットの一つとしてLinuxカーネルで解釈される（[ソースコード](https://github.com/torvalds/linux/blob/f76349cf41451c5c42a99f18a9163377e4b364ff/fs/binfmt_script.c#L34-L43)）
- `#!`という文字列はLinuxカーネルで規定されている
- shebangの解釈はLinuxカーネルが行い、シェルは起動されない。実行ファイルが直接起動される。
- shebangに与えることのできる引数は実行ファイル名の他に1つ以下である
  - 2つ以上渡そうとしても、実行ファイルパスに続く文字列が1つの引数として渡される
- 実行ファイルをインタプリタと呼ぶ
- インタプリタには`argv`として`[ 実行ファイルパス, 引数, スクリプトファイル名 ]`が渡される
