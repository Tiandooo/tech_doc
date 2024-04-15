# 一、网络文件传输（ftp）

1. ## Ftp & vsftpd & lftp

"ftp" 和 "vsftpd" 都是用于在 Linux 系统上提供 FTP（文件传输协议）服务的软件，但它们是不同的软件包，具有一些区别。

1. vsftpd (Very Secure FTP Daemon):

   1. vsftpd 是一个轻量级、快速、安全的 FTP 服务器软件。

   2. 它的设计目标之一是提供最大的安全性。

   3. 默认情况下，vsftpd 的配置非常严格，仅允许本地用户进行 FTP 访问。

   4. 它是许多 Linux 发行版的默认 FTP 服务器选择。

   5.  vsftpd是一个服务，可以用`service vsftpd start/restart/stop`来控制

   6. > bug：invoke-rc.d: policy-rc.d denied execution of start.
      >
      > 原因：/usr/sbin/policy-rc.d的内容为：`#!/bin/sh  exit 101`
      >
      > 表明了它会拒绝执行所有的启动操作，因为脚本中的 `exit 101` 表示在执行完之后会以退出码 101 结束脚本的执行。
      >
      > 修改：`exit 0`

2. "ftp" 通常指的是 "File Transfer Protocol" 的客户端程序，用于在客户端与 FTP 服务器之间进行文件传输。它不能**上传目录**。

3. `lftp`是一个功能强大的文件传输工具，它支持递归地上传和下载目录。

1. ## 安装步骤

1. 首先安装vsftpd
   1. ```Bash
      apt-get install vsftpd
      ```
2. 安装lftp
   1. ```Bash
      apt-get install lftp
      ```
3. 登陆lftp
   1. ```Bash
      lftp 用户名:密码@ftp地址:传送端口（默认21）
      ```
4. 切换本地目录和远程目录
   1. ```Bash
      cd .. 
      lcd ..
      ```
5. 上传
   1. ```Bash
      # 上传文件
      put
      # 上传目录
      mirror -R 本地目录名
      ```
6. 下载
   1. ```Bash
      get
      mget -c *.pdf    
      #把所有的pdf文件以允许断点续传的方式下载。
      mirror aaa/      
      #将aaa目录整个的下载下来，子目录也会自动复制。
      pget -c -n 10 file.dat   
      #以最多10个线程以允许断点续传的方式下载file.dat，可以通过设置pget:default-n的值而使用默认值。
      ```

# 二、各种代理配置

## Git

设置代理：使用以下命令设置代理：

```Bash
git config --global http.proxy http://127.0.0.1:7890（端口号）
```

如果你需要使用 HTTPS 代理，也可以设置 HTTPS 代理：

```Bash
git config --global https.proxy http://127.0.0.1:7890
```

验证配置：你可以使用以下命令验证代理是否正确设置：

```Bash
bashCopy code
git config --global --get http.proxy
git config --global --get https.proxy
```

取消代理：如果需要取消代理，你可以使用以下命令：

```Bash
bashCopy code
git config --global --unset http.proxy
git config --global --unset https.proxy
```

# 三、软连接

在Linux中，你可以使用`ln`命令创建软链接（符号链接）。软链接是指向文件或目录的指针，它们允许你在文件系统中创建指向另一个文件或目录的链接。软链接类似于Windows中的快捷方式。

要创建软链接，可以使用以下命令格式：

```Bash
ln -s 源文件/目录 目标链接
```

例如，要将文件 `file.txt` 创建为指向 `/home/user/documents/file.txt` 的软链接，你可以运行以下命令：

```Bash
ln -s /home/user/documents/file.txt file.txt
```

如果要创建指向目录的软链接，则需要添加 `-d` 选项。例如：

```Bash
ln -s -d /path/to/source_directory /path/to/link_directory
```

这将在`/path/to/link_directory`创建一个指向`/path/to/source_directory`的软链接。

如：

```Python
ln -s /root/duruibin/mot/mmdetection3d/data/nuscenes_mini/ /root/duruibin/CenterPoint/data/nuscenes
```

# 四、screen和查看显存

```Bash
screen -X -S <会话ID或名称> quit
```

screen -X -S 3081193.ruibin.det2 quit

attach之后重新进入

```Bash
screen -d -r ruibin.mmtrack_box
```

持续查看显存

```Bash
watch -n 1 nvidia-smi
```

# 五、Tensorboard端口映射

本地访问server的tensorboard

```
ssh -L [local browser port]:127.0.0.1:[remote tensorboard port] root@[ip] -p [ssh port]
```

**注意：**`remote tensorboard port`和`local browser port`都不能过小，否则可能和现有的port冲突