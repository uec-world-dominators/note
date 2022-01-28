# SSH

## サーバ側

### `authorized_keys`

sshdの`StrictModes`が`no`でないとき、`~/.ssh/authorized_keys`, `~/.ssh/`, `~/`が他人から書き込めて(`(mode & 022) != 0`)はいけない。

> If this file, the ~/.ssh directory, or the user's home directory are writable by other users, then the file could be modified or replaced by unauthorized users. In this case, sshd will not allow it to be used unless the StrictModes option has been set to ''no''.
> https://linux.die.net/man/8/sshd

### ログレベルを下げる

サーバ側の`/etc/ssh/sshd_config`で

```
LogLevel DEBUG
```

のようにログレベルを下げる。デフォルトではINFOだがこれでは`authorized_keys`のパーミッションエラーなどの情報が出力されない。

### できることを制限する

```conf
# 実行するコマンドを制限
command="ls -lh" ssh-rsa ...
# 接続元を制限
from="192.168.0.0/24" ssh-rsa ...
# 制限モード(PortForwardingなどが禁止される)
restrict ssh-rsa ...

# 複数条件
command="ls -lh",restrict,from="192.168.0.0/24" ssh-rsa ...
```

## クライアント側

### 使用する公開鍵を指定する

```
-i id_rsa
```

### 公開鍵認証に限定する

```
-o PreferredAuthentications=publickey
```

### 使用する認証情報を限定する

```
-o IdentitiesOnly=yes
```

鍵を指定しないといろいろな認証情報を試行するので、公開鍵のみに限定する。
「Too many authentication failures」が出た時の原因の一つ。

### SSHホスト鍵の変更を無視する

> 意図しないサーバへの接続や中間車攻撃への対策のための機能
> 
> ```
> @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
> @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
> @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
> ```

```
-o StrictHostKeyChecking=no
-o UserKnownHostsFile=/dev/null
```
