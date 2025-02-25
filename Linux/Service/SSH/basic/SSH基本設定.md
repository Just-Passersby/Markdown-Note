# SSH基本設定

[toc]

# SSH安裝
Debian/Ubuntu安裝：
```bash
apt install ssh
```

Red Hat
```
dnf install ssh
```

# 比較常用的基本設定
設定檔路徑：`/etc/ssh/sshd_config`

基本設定數值：
```bash
Port [NUM]      # Port，預設22，可以寫多行Port開啟多個Port
AddressFamily inet(6)       # 監聽的IP版本，預設any，可以用inet或inet6指定IP版本
```

驗證類：
```bash
LoginGraceTime [TIME]       # 在驗證成功前的登入時長，在這時間內沒登入成功會被斷線，預設為2m(120s)
PermitRootLogin [OPTION]    # 可用的參數為yes/no/prohibit-password和forced-commands-only
                            # prohibit-password顧名思義，不允許密碼登入
                            # forced-commands-only允許公鑰登入，但必須提前指定好登入後執行的指令，直接ssh進來操作是不允許的（password驗證的部份有待商榷）

AyrhorizedkeysFile [PATH]   # SSH登入驗證的金鑰路徑

```

