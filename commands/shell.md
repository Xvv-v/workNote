# shell 变量使用

在 linux 终端上，可以进行变量的创建和使用

直接在终端上输入 varible=value，即可以创建一个变量：

```shell
➜  config variable="test"
```

输出这个变量的值就可以用 echo 命令：

```shell
➜  config echo {variable}
{variable}
➜  config echo $variable
test
➜  config echo $(variable)
zsh: command not found: variable
➜  config echo {variable}
{variable}
➜  config echo "$variable"
test
➜  config echo '$variable'
$variable
➜  config echo `variable`
zsh: command not found: variable
➜  config echo ${variable}
test
```

如上可以看到，echo 输出变量值大概分为以下三种方式：

* $varible
* "$varible"
* ${variable}

删除变量直接用 unset variable 即可：

```shell
➜  config unset variable
➜  config echo ${variable}

➜  config
```

除了以上的基本用法，还可以对字符串进行一些简单处理：

* 可以得到字符串长度

  ```shell
  ➜  config value=test
  ➜  config echo ${#value}
  4
  ```

* 可以输出变量的子字符串：

  格式为 ${count:offset:length}，offset 为从那个位置开始，length 为输出多长的字符串

  ```
  ➜  config count=frogfootman
  # 即为从第 4 个元素开始，往后数 4 个元素输出
  ➜  config echo ${count:4:4}
  foot
  ```

  