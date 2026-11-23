# Windows 上 git 的 TLS 报错

`git clone` / `git fetch` / `git push` 报下面这行时，是 Windows 的 schannel 拿不到客户端凭证，不是网络不通：

```
schannel: AcquireCredentialsHandle failed: SEC_E_NO_CREDENTIALS (0x8009030e)
```

同一个地址在浏览器里能打开、用 Node 的 `https` 也能连上，基本就属于这种情况。把 Git 的 HTTPS 后端换成 OpenSSL：

```bash
git config http.sslBackend openssl
git config http.sslCAInfo "C:/Program Files/Git/mingw64/etc/ssl/certs/ca-bundle.crt"
```

第二条指向 Git for Windows 自带的 CA 包，装在别处时改成实际路径。加 `--global` 对所有仓库生效，
只给当前仓库用就不加。

若改完后仍提示 `Ignoring unavailable SSL backend 'openssl'`，说明这个 Git 发行版没编译 OpenSSL 后端，
换成 Git for Windows，或者改用 SSH 方式的远程地址。
