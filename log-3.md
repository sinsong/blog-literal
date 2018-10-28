---
title: log-3
date: 2018-09-01 21:37:55
tags:
- FreeBSD
- SSH
- PuTTY
---

PuTTY 公钥登陆的坑。

# 生成密钥对

PuTTY的工具 PUTTYGEN.EXE
生成密钥

！不要使用 Save public key按钮

！！！在上面的 Public key for pasting into OpenSSH authorized_keys file
然后复制那里的文本。
保存到一个文件
scp过去，加入authorized_keys。
（就是这里坑了我了）

# 配置sshd

```conf
PasswordAuthentication no # 关闭密码认证
ChallengeResponseAuthentication no # 关闭其他认证 login.conf的那些。。。
# 可选
PermitEmptyPassworlds no # 禁用空密码
```

# 生成主机密钥

密钥名字参见sshd_config

```sh
# ssk-keygen
# 然后问密钥文件名，直接写 /etc/ssh/...
```

