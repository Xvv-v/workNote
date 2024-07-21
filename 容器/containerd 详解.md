# Containerd 详解

containerd 是一个高级容器运行时，用于管理容器的生命周期

containerd 的各个组件：

* **/usr/lib/systemd/system/containerd.service：**systemd 的 unit 文件
* **/usr/bin/containerd：**containerd的二进制文件，在 containerd.service Unit 文件中通过 ExecStart=/usr/bin/containerd 调用，以启动 containerd 进程。
* **/etc/containerd/config.toml：**containerd 的配置文件
* **/usr/bin/containerd-shim：**containerd 的套件，主要目的是隔离 containerd 和容器。当 containerd 的进程收到 grpc 调用（比如来自 kubelet 或 docker 的）便会启动 /usr/bin/containerd-shim 套件
* **/usr/bin/containerd-shim-runc-v2：**containerd-shim启动后会去启动/usr/bin/containerd-shim-runc-v2，然后立即退出，此时containerd-shim-runc-v2的父进程就变成了systemd(1)，这样containerd-shim-runc-v2就和containerd脱离了关系，即便containerd退出也不会影响到容器（这也是containerd-shim套件的作用）。
* **/usr/bin/runc：**runc 是一个底层运行时，OCI标准的具体实现就是runc，真正创建和维护容器最终便是由runc来完成的。/usr/bin/containerd-shim-runc-v2会启动runc去create、start容器，然后runc立即退出，容器的父进程就变成了containerd-shim-runc-v2，这也是容器内部可以看到的PID=1的进程。
* **/usr/bin/ctr：**containerd 的 cli 命令

