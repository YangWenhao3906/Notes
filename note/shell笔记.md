shell 脚本编程

[来自菜鸟教程](https://www.runoob.com/linux/linux-shell.html)

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

#  变量与参数

## 变量

### 变量赋值

赋值不使用标识符

### 变量使用-标识符`${val}`

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
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。 如"$@"用「"」括起来的情况、以`"$1" ` `"$2"` … `"$n" `的形式输出所有参数。 |
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

