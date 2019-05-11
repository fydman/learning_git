# git_setting notes

## 首先github上建立自己的respo

```bash
#首先进行本地SSH公钥的生成。打开git bash终端，键入：
ssh-keygen -t rsa -C "邮箱地址"
#这里的邮箱地址即为你的github账号邮箱。
#之后会本地生成shh key
 (/home/fydman/.ssh/id_rsa)
#打开浏览器登陆github，在自己的账户面板下找到SSH keys这一栏，打开后即会看到目前该账户下已进行过SSH认证的机器，
#选择Add SSH key之后，将前一步复制的内容粘贴至Key中，同时需要编辑一个Title来说明此Key认证的是哪一台机器，通常会使用计算机的名字。
ssh git@github.com
#成功认证
```

> learning_git** **git:(****master)** **✗**  ssh git@github.com
>
> The authenticity of host 'github.com (13.229.188.59)' can't be established.
> RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
> Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
> Warning: Permanently added 'github.com,13.229.188.59' (RSA) to the list of known hosts.
> PTY allocation request failed on channel 0
> Hi fydman! You've successfully authenticated, but GitHub does not provide shell access.
> Connection to github.com closed.

