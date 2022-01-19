shell 脚本编程教程

[来自菜鸟教程](https://www.runoob.com/linux/linux-shell.html)

感觉菜鸟教程写的很乱

[来自阮一峰blog](https://wangdoc.com/bash/)

# 准备工作

## 解释器: 采用bash

```shell
#!/bin/bash
```

## shell脚本运行

### 方式1: 可执行

```shell
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```

### 方式2:解释器参数

```shell
/bin/sh test.sh
/bin/php test.php
```

## 基本语法

### 换行

```shell
$ echo foo bar

# 等同于
$ echo foo \
bar
```

### 分号

分号（`;`）是命令的结束符，使得一行可以放置多个命令，上一个命令执行结束后，再执行第二个命令。

```shell
$ clear; ls
```

上面例子中，Bash 先执行`clear`命令，执行完成后，再执行`ls`命令。

**注意，使用分号时，第二个命令总是接着第一个命令执行，不管第一个命令执行成功或失败。**

## 快捷键

- `Ctrl + L`：清除屏幕并将当前行移到页面顶部。
- `Ctrl + C`：中止当前正在执行的命令。
- `Shift + PageUp`：向上滚动。
- `Shift + PageDown`：向下滚动。
- `Ctrl + U`：从光标位置删除到行首。
- `Ctrl + K`：从光标位置删除到行尾。
- `Ctrl + D`：关闭 Shell 会话。
- `↑`，`↓`：浏览已执行命令的历史记录。

除了上面的快捷键，Bash 还具有自动补全功能。命令输入到一半的时候，可以按下 Tab 键，Bash 会自动完成剩下的部分。比如，输入`tou`，然后按一下 Tab 键，Bash 会自动补上`ch`。****

# 模式扩展

## 原理

Shell 接收到用户输入的命令以后，会根据空格将用户的输入，拆分成一个个`词元`（token）。然后，Shell 会扩展词元里面的特殊字符，扩展完成后才会调用相应的命令。

这种特殊字符的扩展，称为模式扩展（globbing）。其中有些用到通配符，又称为通配符扩展（wildcard expansion）。Bash 一共提供八种扩展。

- 波浪线扩展
- `?` 字符扩展
- `*` 字符扩展
- 方括号扩展
- 大括号扩展
- 变量扩展
- 子命令扩展
- 算术扩展

## 波浪线扩展

波浪线`~`会自动扩展成当前用户的主目录。

```shell
$ echo ~
/home/me
```

`~/dir`表示扩展成主目录的某个子目录，`dir`是主目录里面的一个子目录名。

```shell
# 进入 /home/yang/foo 目录
$ cd ~/foo
```

`~user`表示扩展成用户`user`的主目录。

```shell
$ echo ~foo
/home/foo

$ echo ~root
/root
```

## ?字符扩展

如果匹配多个字符，就需要多个`?`连用。

```shell
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls ??.txt
ab.txt
```

上面命令中，`??`匹配了两个字符。

`?` 字符扩展属于文件名扩展，只有文件确实存在的前提下，才会发生扩展。如果文件不存在，扩展就不会发生。

```shell
# 当前目录有 a.txt 文件
$ echo ?.txt
a.txt

# 当前目录为空目录
$ echo ?.txt
?.txt
```

## *字符扩展

### 不匹配子目录

### 不存在->原样输出

注意，`*`字符扩展属于文件名扩展，只有文件确实存在的前提下才会扩展。如果文件不存在，就会原样输出。

```shell
# 当前目录不存在 c 开头的文件
$ echo c*.txt
c*.txt
```

上面例子中，当前目录里面没有`c`开头的文件，导致`c*.txt`会原样输出



#  变量与参数

## 变量

### 变量赋值

赋值不使用标识符

### 变量使用-标识符`${val}`

**习惯: 用大括号{}包括**

```shell
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
```

### 变量删除

```shell
unset variable_name
```

### 变量类型

- **1) 局部变量** 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
- **2) 环境变量** 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
- **3) shell变量** shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

## 字符串

### 双引号

```shell
your_name = "runoob"
str = "Hello, I know you are \"$your_name\"! \n"
echo -e $str
```

输出结果为：

```shell
Hello, I know you are "runoob"! 
```

双引号的优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符

### (先略过)

## 数组

### 定义数组

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：

```shell
数组名=(值1 值2 ... 值n)
```

例如：

```shell
array_name=(value0 value1 value2 value3)
```

或者

```shell
array_name=(
value0
value1
value2
value3
)
```

还可以单独定义数组的各个分量：

```shell
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

### 读取数组

读取数组元素值的一般格式是：

```shell
${数组名[下标]}
```

例如：

```shell
valuen = ${array_name[n]}
```

使用 **@** 符号可以获取数组中的所有元素，例如：

```shell
echo ${array_name[@]}
```

### 获取数组的长度

获取数组长度的方法与获取字符串长度的方法相同，例如：

```shell
# 取得数组元素的个数
length = ${#array_name[@]}
# 或者
length = ${#array_name[*]}
# 取得数组单个元素的长度
lengthn = ${#array_name[n]}
```

## 注释

### 单行#

### 多行:<<'

```shell
:<<'
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!
```

## 参数$*

| 参数处理 | 说明                                                         |
| :------- | ------------------------------------------------------------ |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 以一个单字符串显示所有向脚本传递的参数。 如"$*"用「"」括起来的情况、以`"$1 $2 … $n"`的形式输出所有参数。 |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与\$*相同，但是使用时加引号，并在引号中返回每个参数。 如"$@"用「"」括起来的情况、以`"$1" ` `"$2"` … `"$n" `的形式输出所有参数。 |
| $-       | 显示Shell使用的当前选项，与[set命令](https://www.runoob.com/linux/linux-comm-set.html)功能相同。 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

# 运算符

## 文件测试运算符

| 操作符  | 说明                                                         | 举例                      |
| :------ | :----------------------------------------------------------- | :------------------------ |
| -b file | 检测文件是否是块设备文件block，如果是，则返回 true。         | [ -b $file ] 返回 false。 |
| -c file | 检测文件是否是字符设备文件char，如果是，则返回 true。        | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录dir，如果是，则返回 true。                 | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件file（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            | [ -g $file ] 返回 false。 |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  | [ -k $file ] 返回 false。 |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            | [ -u $file ] 返回 false。 |
| -r file | 检测文件是否可读`read`，如果是，则返回 true。                | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写`write`，如果是，则返回 true。               | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行`exe`，如果是，则返回 true。               | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在`exist`，如果是，则返回 true。   | [ -e $file ] 返回 true。  |

其他检查符：

- **-S**: 判断某文件是否 socket。
- **-L**: 检测文件是否存在并且是一个符号链接。

```shell
#!/bin/bash
file="/var/www/runoob/test.sh"
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi

if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi

if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi

if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi

if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi

if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi

if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

## test

## 算数运算符(略)

```shell
#!/bin/bash
a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```

## 关系运算符

| 运算符 | 说明                                                  | 举例                       |
| :----- | :---------------------------------------------------- | :------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [ $a -eq $b ] 返回 false。 |
| -ne    | 检测两个数是否不相等，不相等返回 true。               | [ $a -ne $b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ $a -gt $b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ $a -lt $b ] 返回 true。  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ $a -ge $b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |

# 输出相关

## echo

## printf

# 控制相关

## for 循环

与其他编程语言类似，Shell支持for循环。

for循环一般格式为：

```shell
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

输出数字

```shell
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

## while循环

while 循环用于不断执行一系列命令，也用于从输入文件中读取数据。其语法格式为：

```shell
while condition
do
    command
done
```

输出1到5

```shell
#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```



