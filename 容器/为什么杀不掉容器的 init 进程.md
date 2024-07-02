# 容器的 init 进程

一个 Linux 系统打开电源后，首先会执行 BIOS/boot-loader，由 boot-loader 负责加载 Linux 内核。**内核完成各种初始化操作后就会执行第一个用户态进程，也就是 init 进程，又称 1 号进程。**目前主流的 Linux 发行版都会把 /sbin/init 作为符号连接指向 Systemd，Systemd 是目前最流行的 init 进程。

init 进程的作用（之一）就是创建出系统中的其他进程并管理它们。给 init 进程下个定义就是：**init 号进程是第一个用户态的进程，由它直接或者间接创建了 Namespace 中的其他进程。**

Linux 中定义了 64 个信号：

```shell
[root@trustbe-1 ~]# kill -l
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX
```

没个信号都有三种处理方法：

* 忽略
* 捕获
* 缺省

注意：SIGKILL 和 SIGSTOP 除外，这两个信号不能被捕获和处理，因为它们的主要作用是为 Linux kernel 和超级用户提供删除任意进程的特权。