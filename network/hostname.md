# hostname

首先查看服务器的 hostname 只需要输入 `hostname` 命令即可

```bash
[root@node-1] hostname
```

如果要修改 hostname，有几种方式：

1. 临时修改，执行 `hostname xxx` 即可完成修改，但重新打开终端就会失效

   ```
   [root@node-1] hostname NEW_HOSTNAME
   ```

2. 永久修改，有两个文件可以永久修改

   1. `/etc/hostname` 文件

      ```
      HOSTNAME=myhostname
      ```

   2. `/etc/sysconfig/network` 文件

      ```
      myhostname
      ```

   > 区别：
   >
   > /etc/sysconfig/network 是 Red Hat 系列 Linux 发行版特有的，用于存储网络配置信息，包括主机名
   >
   > /etc/hostname 是通用的 Linux 配置文件，用于专门指定主机名，优先级更高，如果两个文件有冲突，按照 /etc/hostname 来

另外还要区分 `/etc/hosts` 文件。`/etc/host` 文件用于将主机名解析为 IP 地址，如果你想用主机名访问某台机器，就需要将该机器的主机名和地址写入 `/etc/host` 文件中，文件的格式为：

```
IP_address hostname1 hostname2 ...
```

各部分含义为：

* IP_address：主机的 IP 地址
* hostname1、hostname2...：与 IP 对应的主机名，一个 IP 可以对应多个主机名，每个主机名都可以在不同的情况下使用



