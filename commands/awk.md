# awk 的基本使用

awk 命令

awk 会使用一些数字来表示分割的部分：

* $0：表示整行
* $1：表示第一个字段
* $2：表示第二个字段（后边以此类推

除了数字还有一些变量：

* NF：表示当前有多少个字段，所以 $NF 可以表示当最后一个字段，以此类推 $(NF-1) 就是倒数第二个字段
* NR：输出当前处理的是第几行
* FILENAME：当前文件名
* FS：字段分隔符，默认是空格和制表符。
* RS：行分隔符，用于分割每一行，默认是换行符。
* OFS：输出字段的分隔符，用于打印时分隔字段，默认为空格。
* ORS：输出记录的分隔符，用于打印时分隔记录，默认为换行符。
* OFMT：数字输出的格式，默认为 `％.6g`

分割一行输入：

```shell
$ echo "hello world" | awk '{print $1}'
hello
```

还可以多个：

```shell
$ echo "hello linux world" | awk '{print $1 $3}'
helloworld
```

如果是处理文件中的内容应该是这样的格式：

```shell
$ awk '{print $xx}' 文件名
```

除了使用默认的空格分隔，还可以使用 `-F` 指定其他分隔符：

```shell
$ echo "hello:linux:world" | awk -F : '{print $1}'
hello
```

以上都是使用数字表示分割字段，变量的使用如下

在 awk 命令中，变量可以不加 $ 在花括号内直接使用：

```shell
$ echo "hello linux world" | awk '{print NF ")" $NF}'
3)world
```

上述命令就是先打印了 NF 即最后一个字段的序号，接着打印了一个括号（在 awk 命令中双引号内的内容会原样输出），接着打印了最后一个字段的内容。

> 注：awk 命令 {} 中空格无效，只是方便观看命令，如果想要输出结果中有空格，要加引号 {" "}，这样才有效

除此之外，awk 还有一些内置函数（此处只是举例，完整请看文档）

* toupper()：字符转为大写
* tolower()：字符转为小写。
* length()：返回字符串长度。
* substr()：返回子字符串。
* sin()：正弦。
* cos()：余弦。
* sqrt()：平方根。
* rand()：随机数。

函数使用方法和变量类似，不用加 $，可以直接使用：

```shell
$ echo "hello linux world" | awk '{print toupper($0)}'
HELLO LINUX WORLD
$ echo "hello linux world" | awk '{print toupper($1)}'
HELLO
```

awk 除了以上一些简单处理并输出，还能添加条件

格式如下：

```shell
$ awk '条件 动作' 文件名
```

条件必须要在动作之前

```shell
# 输出奇数行
$ awk -F ':' 'NR % 2 == 1 {print $1}' demo.txt
root
bin
sync
```

