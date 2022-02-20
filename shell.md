## コメント・空行を除いて表示

```
grep -vE '^(#|$)' < hoge.conf
```

`moreutils`の`sponge`を使うとIn-Placeで

```
function rmcomments() {
  cat $1 | grep -vE '^(#|$)' | sponge $1
}
```
