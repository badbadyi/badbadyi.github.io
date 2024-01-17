---

title: Github SSH Timed Out
categories: [计算机, 问题排查]
tags: [速记, git, 问题排查]
authors: [badbadyi]

---

## 问题

远程连接 GitHub 时出现如下错误

```
ssh: connect to host github.com port 22: Connection timed out
```

## 排查

如下命令超时

```sh
ssh -T git@github.com
```

如下命令成功连接

```sh
ssh -T -p 443 git@ssh.github.com
```

## 解决

通过在 `.ssh` 文件夹下 `config` 文件新增如下内容覆盖设置解决该问题

```
Host github.com
  Hostname ssh.github.com
  Port 443
```

`.ssh` 文件夹位于用户文件夹下（通过 `cd ~` 切换）

没有 `config` 文件新建即可

## 参考

[Solution for 'ssh: connect to host github.com port 22: Connection timed out' error](https://gist.github.com/Tamal/1cc77f88ef3e900aeae65f0e5e504794)
