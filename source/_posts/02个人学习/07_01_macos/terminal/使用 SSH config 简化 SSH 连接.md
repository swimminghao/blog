---
title: 使用 SSH config 简化 SSH 连接
tags:
  - mac
categories: 工具
abbrlink: 67da8aa2
date: 2025-09-15 06:06:00
---

# 使用 SSH config 简化 SSH 连接

如果你有很多的服务器要连接，如果对你来说记住那些服务器的地址、端口实在是一种痛苦，如果你一直在寻找一种能够简化在命令行下连接 SSH 服务器的办法，那么，本文将给你提供一种解决问题的思路，那就是，使用 SSH 的 config 文件。

## SSH config 文件是什么

Open SSH 客户端配置文件，允许你以配置项的形式，记录各个服务器的连接信息，并允许你使用一个定义好的别名来代替其对应的 ssh 命令参数。

## SSH config 文件该怎么用

### 创建 SSH config 文件

通常来说，该文件会出现在两个地方，一个是`/etc/ssh/ssh_config`，一个是`~/.ssh/config`。

`/etc/ssh/ssh_config`文件通常用来定义全局范围上的 SSH 客户端参数，而`~/.ssh/config`则被用来定义每个用户自己的 SSH 客户端的配置。我们将要修改的，就是位于用户目录下的 config 文件。

如果`~/.ssh/config`文件不存在，那么也不用着急，这是正常的，只需要执行如下命令，即可新建一个空白的 config 文件

```
# 创建 .ssh 目录（如果已存在，会提示没关系）
mkdir -p ~/.ssh

# 设置正确的权限，SSH 对文件权限非常严格
chmod 700 ~/.ssh

# 创建 config 文件（或编辑已存在的）
touch ~/.ssh/config

# 设置 config 文件的权限
chmod 600 ~/.ssh/config
```

### 在本地电脑上生成一个 **RSA** 类型的密钥对

生成一个 **RSA** 类型的密钥对，包含一个私钥（`id_rsa`.centos）和一个公钥（`id_rsa.centos.pub`）

```bash
ssh-keygen -t rsa -b 2048 -C "centos" -f ~/.ssh/id_rsa.centos
```

执行后会询问你三个问题：

1. **密钥保存路径**（默认为 `~/.ssh/`）
2. **密钥的密码（Passphrase）**（推荐设置，可为空直接回车）
3. **确认密码**

**生成成功**后，你会看到类似下面的输出：

```bash
Your identification has been saved in /home/your_user/.ssh/id_ed25519_github
Your public key has been saved in /home/your_user/.ssh/id_ed25519_github.pub
The key fingerprint is:
SHA256:2fLsxGsdHPrqF1qC0Wkj2sSfKkLwHnTZ1pVXyMzA1Bc user@host
The key's randomart image is:
+--[ED25519 256]--+
|         .o      |
|        . o .    |
|       . o o .   |
|        o o o . .|
|       .S o o o.o|
|      . .+ . .+*+|
|       .+.o.+o=*+|
|      .o+o+.o+o+E|
|      .oo++...oo.|
+----[SHA256]-----+
```

### 生成后的文件与管理

执行上述命令后，在你的 `~/.ssh/` 目录下会生成两个文件：

- `id_rsa.centos`：这是**私钥**。
  - **必须像保护密码一样保护它！**
  - 权限必须是 `600`（`-rw-------`）。SSH 客户端会拒绝权限过宽的私钥。
- `id_rsa.centos.pub`：这是**公钥**。
  - 这个文件**可以随便分发**，它的内容需要上传到你需要连接的远程服务器上。

**确保私钥的权限正确：**

```bash
chmod 600 ~/.ssh/id_rsa.centos
```

### 分发公钥

将公钥文件（`.pub` 后缀）的内容复制粘贴添加到远程服务器的 `~/.ssh/authorized_keys` 文件的末尾。

**确保远程服务器上的文件权限正确：**

```
# 在远程服务器上执行：
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

## 编写 config 条目

假如说，我们想连接到一台服务器，它的地址是 example.server.com，端口号是 2222，以用户 root 登陆，并使用~/.ssh/id_rsa_myserver 这个私钥验证身份。那么，我们需要在命令行里输入：

```
ssh root@example.server.com -p 2222 -i ~/.ssh/id_rsa_myserver
```

嗯好吧，-i 参数可以省略，但即使这样，命令还是很长，对吧？

那么我们把这个服务器的连接参数写到 config 文件里，就变成了这个样子：

```
# 此处我为了美观起见，给每个子条目都缩进了一层，实际使用时缩进不影响文件的效果。

Host example //起别名
    Hostname        example.server.com
    Port            2222
    User            admin
    Identityfile    ~/.ssh/id_rsa
```

嗯，在这里，它还有了一个新名字，叫`example`。

然后，我们只需要：

```
ssh example
```

就可以连接到这台主机了。

***

看到这里就能实现使用 SSH config连接主机了，以下内容为扩展内容。
## 创建通配符规则

有的时候，我们需要连接多台不同的主机，那难道我们需要针对每个主机都写一遍规则吗？

答案是不一定。如果你要连接的主机，它们的域名有一定规律可循，那么我们可以用通配符来匹配这一系列的主机。

比如，公司里针对开发和测试环境，各创建了一系列集群，同时各个集群中又根据负责的业务不同，有多台负载均衡的主机，那么我们可以这样写：

```
# 首先匹配所有主机，在这里配置好我们的用户名、私钥等参数
Host *
    User            boris1993
    IdentityFile    ~/.ssh/id_rsa

# 开发环境
Host dev-*
    HostName    %h.dev.mycompany.com

# 测试环境
Host staging-*
    HostName    %h.staging.mycompany.com
```

接下来，我们只需要执行类似`ssh dev-user`，就可以连接到开发环境的负责用户管理的服务器上了。

而实现这个操作的重点，我想你已经注意到了，一个是`Host`配置中的`*`，另一个就是配置文件里面的`%h`。星号我们都知道，是个通配符，放在这里就意味着它会匹配所有以`dev-*`开头的主机名；而`%h`是一个占位符，它会把你在`SSH`命令中`destination`部分的输入取出来放在这里，所以当我们执行`ssh dev-user`的时候，实际上命令会被展开成`ssh dev-user.dev.mycompany.com`。

## 配置通过跳板机连接

如果公司规定所有服务器都需要先`SSH`到跳板机，然后才能连接到具体的服务器，那该怎么办？把`ssh_config`文件放在跳板机上吗？

确实这是一个解决方案，但不是唯一解。因为我们还有一个配置参数叫`ProxyJump`。它，就是用来指定，在连接这个服务器之前，需要通过哪个跳板机。

写起来，是这样的：

```
# 其他部分略

# 跳板机
Host bastion
    HostName    bastion.mycompany.com

# 服务器
Host dev-*
    HostName    %h.dev.mycompany.com 
    ProxyJump   bastion
```

如果你的`SSH`版本早于 7.3，那么很可惜它是不支持`ProxyJump`这条配置的。但是不要灰心，它支持另一条配置，`ProxyCommand`。

```
# 其他部分略

# 跳板机
Host bastion
    HostName    bastion.mycompany.com

# 服务器
Host dev-*
    HostName        %h.dev.mycompany.com 
    ProxyCommand    ssh -W %h:%p bastion 
```

然后，我们继续执行`ssh dev-user`就可以了，`SSH`会自动先连接到跳板机，然后在跳板机中再连接到我们要去的服务器。

如果你们公司安全做的非常好，需要通过数个跳板机来连接服务器，那么只需要给每个跳板机都配置上更外层的跳板就好了，像这样：

```
# 其他部分略

# 最外层跳板机
Host bastion-outer
    HostName    bastion-outer.mycompany.com

# 下一层跳板机
Host bastion-inner
    HostName    bastion-inner.mycompany.com
    ProxyJump   bastion-outer

# 服务器
Host dev-*
    HostName    %h.dev.mycompany.com 
    ProxyJump   bastion-inner
```

这样，`SSH`就会先连接到`bastion-outer`，然后连到`bastion-inner`，然后连到`dev-user`。理论上，你可以在这里无限套娃。
