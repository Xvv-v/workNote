# yum

`yum` 是一种在基于 RPM（Red Hat Package Manager）的 Linux 发行版中用于管理软件包的工具。它通常用于在系统上安装、更新和删除软件包，以及解决软件包之间的依赖关系。

linux 中与 yum 有关的文件有：

1. **/etc/yum.conf**：主要的配置文件，用于配置 yum 的行为，例如软件源、代理设置等。
2. **/etc/yum.repos.d/** 目录：该目录包含了 yum 的软件源配置文件，每个文件对应一个软件源。这些文件以 `.repo` 为扩展名，其中包含了软件源的 URL、名称、镜像等信息。
3. **/var/cache/yum/** 目录：这是 yum 的缓存目录，包含了下载的软件包和元数据。
4. **/var/log/yum.log**：这是 yum 的日志文件，记录了每次 yum 操作的详细信息，例如安装、更新或删除软件包等操作。



解释一下 `/etc/yum/repos.d/CentOS-Base.repo` 文件内容：

```sh
[base]
name=CentOS-$releasever
enabled=1
failovermethod=priority
baseurl=http://mirrors.cloud.aliyuncs.com/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.cloud.aliyuncs.com/centos/RPM-GPG-KEY-CentOS-7

[updates]
name=CentOS-$releasever
enabled=1
failovermethod=priority
baseurl=http://mirrors.cloud.aliyuncs.com/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.cloud.aliyuncs.com/centos/RPM-GPG-KEY-CentOS-7

[extras]
name=CentOS-$releasever
enabled=1
failovermethod=priority
baseurl=http://mirrors.cloud.aliyuncs.com/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=http://mirrors.cloud.aliyuncs.com/centos/RPM-GPG-KEY-CentOS-7
```

这个文件是定义了三个 yum 软件源：

* base

  这个软件源包含了 CentOS 的基本软件包。`baseurl` 参数指定了基本软件包的下载地址，其中 `$releasever` 和 `$basearch` 是变量，分别代表系统的版本和体系结构。`gpgcheck` 参数指定是否检查软件包的 GPG 签名来验证其真实性。`gpgkey` 指定了用于验证签名的 GPG 密钥的下载地址。

* updates

  这个软件源包含了 CentOS 的更新软件包，用于修复漏洞、提供新特性等。与 `base` 类似，它也有相应的 `baseurl`、`gpgcheck` 和 `gpgkey` 参数。

* extras

  这个软件源包含了额外的 CentOS 软件包，它们通常不包含在基本的发行版中，但可能提供一些额外的功能或软件包。与前两个软件源类似，它也有相应的 `baseurl`、`gpgcheck` 和 `gpgkey` 参数。

这些软件源是通过 HTTP 协议访问的，地址指向了阿里云的镜像源。通过这些软件源，可以方便地使用 YUM 来安装、更新和管理 CentOS 系统中的软件包。