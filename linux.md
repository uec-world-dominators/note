## (ssh越しに)userモードのsystemctlを使う

```
XDG_RUNTIME_DIR=/run/user/$(id -u) systemctl --user status hoge.service
```

> XDG_RUNTIME_DIRを指定しないとエラー`Failed to connect to bus: No medium found`
