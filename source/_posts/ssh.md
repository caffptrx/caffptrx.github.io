---
title: 使用 SSH 连接到 GitHub
date: 2021-11-04 18:50:42
author: caffx
tags:
    - SSH
categories:
    - 开发
---
<p style="font-weight: bold; color: #dc143c;">
注意！本教程的教学环境都基于 Windows 10，但过程大致相同，针对不同的终端，请使用不同的命令完成相同的操作。
</p>

### 生成新 SSH 密钥
在 Git Bash 下输入命令，**将 your_email@example.com 替换为你的邮件地址**。
```
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```
> 如果不支持 ED25519 算法，使用以下命令。
> ```
> $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
> ```

如果创建成功，将提示:
```
> Generating public/private algorithm key pair.
```

提示 ```Enter a file in which to save the key.``` (输入要保存密钥的文件) 时，按 *Enter* 键，将密钥创建在默认位置。

出现如下代码时，输入安全密码。
```
> Enter passphrase (empty for no passphrase):
> Enter same passphrase again:
```
连续按下两次 Enter 键将创建空密码。

<!-- more -->

### 将 SSH 密钥添加至 ssh-agent
确保 ssh-agent 正在运行，或者手动启动它:
```
$ eval "$(ssh-agent -s)"
```
将 SSH 私钥添加至 ssh-agent，如果你创建了或者需要添加不同名称的密钥，把命令中的 *id_ed22519* 替换为你的私钥文件的名称。
```
$ ssh-add ~/.ssh/id_ed22519
```
### 将 SSH 密钥添加至 GitHub 账户
1. 复制 SSH 公钥。
如果你的 SSH 公钥文件与以下代码不同，请修改文件名以匹配设置。在复制密钥时，请勿添加任何字符、新行或空格。
```
$ clip < ~/.ssh/id_ed22519.pub
```
如果 ```clip``` 不可用，找到隐藏的 ```.ssh``` 文件夹，在编辑器中打开你的公钥文件并将其里面的内容复制到剪贴板。


2. 进入 [GitHub 官网](https://github.com/)，在任何界面的右上角点击你的头像，然后点击 *Settings*。
3. 在用户设置侧边栏中，找到并点击 *SSH and GPG keys*。
4. 点击 *New SSH Key 或 Add SSH key*。
5. 在 *Title* 字段中，为新的密钥添加标签。
6. 将密钥粘贴到 *Key* 字段中。
7. 点击 *Add SSH Key*。
8. 如有提示，确认你的 GitHub 密码。

### 测试 SSH 连接
输入以下命令:
```
$ ssh -T git@github.com
```
> 可能会出现如下的警告
> ```
> > The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> > RSA key fingerprint is SHA256: (GitHub PUBLIC FINGERPRINT).
> > Are you sure you want to continue connecting (yes/no)?
> ```
> 验证所看到消息中的指纹是否与 [GitHub 的 RSA 公钥指纹](https://docs.github.com/cn/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints) 匹配，如果是，则输入 <code>yes</code>:

如果连接成功，会出现:
```
> Hi username! You've successfully authenticated, but GitHub does not
> provide shell access.
```
### 常见问题
1. Git Bash 错误 ```ssh: connect to host localhost port (PORT_NUMBER): Connection refused.```
以下是可能的原因
    - 端口未打开或被占用
        打开 CMD 终端，输入以下命令，**将 port_number 替换为错误消息中的端口号**:
        ```
        netstat -ano | findstr "port_number"
        ```
        找到正在监听对应端口的进程的 PID，输入以下命令查看对应进程的进程名，**将 processid 替换为对应进程的 PID**:
        ```
        tasklist | findstr "processid"
        ```
        如果结束此程序并无大碍，输入以下命令，**将 processid 替换为对应进程的 PID**:
        ```
        taskkill /pid "processid"
        ```
        随后重新尝试连接。

    - 本地防火墙拒绝
    - 本机无 SSH 服务
        1. 打开 *设置*，点击窗口中的 *应用* 图标。
        2. 在窗口中，点击左侧的 *应用和功能*，然后再在右侧窗口中点击 *管理可选功能* 快捷连接。
        3. 点击 *添加功能*，选择 *OpenSSH Client*，在弹出的窗口中点击 *安装*。
        随后重新尝试连接。