## iptables-nftでMASQUERADEとかすると`nft list ruleset`の表示が変

Dockerは`iptables`を使うのでnftables環境で`iptables`フロントエンドが使われたときに起きる。

```
$ nft --version
nftables v1.0.1 (Fearless Fosdick #3)

$ iptables --version
iptables v1.8.7 (nf_tables)
```

### まずは普通に`nftables`でmasquerade

```sh
nft add rule nat POSTROUTING oifname eno1 masquerade
```

それぞれのツールで見る

```sh
# iptables-save
# Table `nat' is incompatible, use 'nft' tool.

# nft list ruleset
oifname "eno1" masquerade
```

nftでルールを追加した場合はiptablesでは表示できなかった。


### `iptables-nft`でmasquerade

```sh
iptables -t nat -A POSTROUTING -o eno1 -j MASQUERADE
```

```sh
# iptables-save
-A POSTROUTING -o eno1 -j MASQUERADE

# nft list ruleset
oifname "eno1" # xt_MASQUERADE
```

iptablesフロントエンドで追加した場合は`masquerade`ではなく`xt_MASQUERADE`って表示される。

### 理由

ソースコード見るとここで出力してるっぽい。

```c
void xt_stmt_xlate(...) {
    ...
    nft_print(octx, "# xt_%s", stmt->xt.name);
    ...
}
// https://git.netfilter.org/nftables/tree/src/xt.c?h=v1.0.2&id=5b364657a35f4e4cd5d220ba2a45303d729c8eca#n77
```

[`statement.c`](https://git.netfilter.org/nftables/tree/src/statement.c?h=v1.0.2&id=5b364657a35f4e4cd5d220ba2a45303d729c8eca)によると、`nftables`のmasqueradと、`iptables (xtables)`のmasqueradeは別の文として扱われてる。たぶん裏で使ってるエンジンが違ってて、完全な互換性がないからこういう表示になっているのでは
