# Shell

[TOC]

## 一、第一个Shell脚本

```shell
#!/bin/bash
echo "Hello world!"
```

* 执行：
  * chmod +x shell.sh       ./shell.sh
  * sh shell.sh
  * source shell.sh





## 二、Shell变量

* **常用方式：**

```shell
#!/bin/bash
# 1.变量名和等号之间不能有空格
param="myname"
echo ${param}
echo $param
# 2.使用语句给变量赋值
for file in `ls /etc`
# 或
# for file in $(ls /etc)
do 
echo ${file}
done
# 3.只读变量
myUrl="http://www.google.com"
readonly myUrl
myUrl="http://www.runoob.com"
# 4.删除变量，不能删除只读变量
unset myUrl
```

* **变量类型：**局部变量，环境变量和shell变量。



### 2.1 字符串

>  定义字符串：分单引号和双引号。

**双引号的优点：**

- 双引号里可以有变量
- 双引号里可以出现转义字符

```shell
your_name='runoob'
# 1. 单引号：
str='this is a string'
# 1.1 单引号使用变量：
str='hello, '$your_name' !'
# 2. 双引号：
str="this is a string"
# 2.1 双引号转义：
str="Hello, I know you are \"$your_name\"! \n"
echo -e $str
# Hello, I know you are "runoob"! 
```



#### 2.1.1 拼接字符串

```shell
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```

输出结果为：

```
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```



#### 2.1.2 获取字符串长度

```shell
string="abcd"
echo ${#string} #输出 4
```



#### 2.1.3 截取子字符串

1. 以下实例从字符串第 **2** 个字符开始截取 **4** 个字符：

```shell
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

**注意**：第一个字符的索引值为 **0**。

```shell
echo ${var:7}
# 从右边计算第7个，截取3个
echo ${var:0-7:3}
# 注：（左边的第一个字符是用 0 表示，右边的第一个字符用 0-1 表示）
```

2. **其他截取：**

```shell
str="www.runoob.com/linux/linux-shell-variable.html"
echo "str    : ${str}"
echo "str#*/    : ${str#*/}"   # 从 字符串开头 删除到 左数第一个'/'
echo "str##*/    : ${str##*/}"  # 从 字符串开头 删除到 左数最后一个'/'
echo "str%/*    : ${str%/*}"   # 从 字符串末尾 删除到 右数第一个'/'
echo "str%%/*    : ${str%%/*}"  # 从 字符串末尾 删除到 右数最后一个'/'
echo
echo "str#/*    : ${str#/*}"   # 无效果
echo "str##/*    : ${str##/*}"  # 无效果
echo "str%*/    : ${str%*/}"   # 无效果
echo "str%%*/    : ${str%%*/}"  # 无效果
```

输出结果：

```shell
str     : www.runoob.com/linux/linux-shell-variable.html
str#*/  : linux/linux-shell-variable.html
str##*/ : linux-shell-variable.html
str%/*  : www.runoob.com/linux
str%%/* : www.runoob.com
```

3. 设置默认值

```shell
${file:-my.file.txt}  :若$file没有设定或为空值，则使用my.file.txt作返回值。(非空值时不作处理)
```





#### 2.1.4 查找子字符串

查找字符 **i** 或 **o** 的位置(哪个字母先出现就计算哪个)：

```shell
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```

**注意：** 以上脚本中 **`** 是反引号，而不是单引号 **'**，不要看错了哦。



#### 2.1.5 读取键盘输入

```shell
read -p "input a val:" a 
```



### 2.2 数组

> 数组的值也可以写入变量。

#### 2.2.1 定义数组

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开：

```shell
数组名=(值1 值2 ... 值n)
```

还可以单独定义数组的各个分量：

```shell
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

可以不使用连续的下标，而且下标的范围没有限制。



#### 2.2.2 读取数组

读取数组元素值的一般格式是：

```shell
${数组名[下标]}
```

```shell
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```



### 2.3 注释

1. **单行：#**

2. **多行：EOF可以替换其他字符**

   ```shell
   :<<EOF
   注释内容...
   注释内容...
   注释内容...
   EOF
   ```





## 三、传递参数

> 可以在执行命令的时候传递参数

```shell
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

```linux
$ ./test.sh 1 2 3
第一个参数为：1
第二个参数为：2
第三个参数为：3
```

**$* 与 $@ 区别：**

- 相同点：都是引用所有参数。
- 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。





## 四、shell运算符

### 4.1 算术运算符

```shell
#!/bin/bash
val=`expr 2 + 2`
echo "两数之和为 : $val"
```

```
两数之和为 : 4
```

**两点注意：**

- 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2。
- 完整的表达式要被 **` `** 包含，注意这个字符不是常用的单引号，在 Esc 键下边。

包含：

```shell
# + - * / & = == != 这些运算符
a=10
b=20
echo `expr $a = $b`
```





## 五、shell符号含义

### 1. $() 和 ``  （命令替换）

在 bash shell 中，$( ) 与`` (反引号) 都是用来做**命令替换**用(commandsubstitution)的。

* version=$(uname -r)和version=`uname -r`都可以是version得到内核的版本号

* 优缺点：

  1. ``基本上可用在全部的 unix shell 中使用，若写成 shell script ，其移植性比较高。但反单引号容易打错或看错。

  2. $()并不是所有shell都支持。



### 2. ${ } 和 $    （变量替换）

${ }用于变量替换。一般情况下，$var 与${var} 并没有啥不一样。但是用 **${ } 会精确的界定变量名称的范围**



### 3. $[] $(())     （数学运算）

```shell
a=10
b=20
# 方式1
c=`expr ${a} + ${b}`
# 方式2
c=$[a + b]
# 方式3
c=$((a + b))
```



### 4. [ ] 	test命令

但要注意许多：

1.你必须在左括号的右侧和右括号的左侧各加一个空格，否则会报错。

2.test命令使用标准的数学比较符号来表示字符串的比较，而用文本符号来表示数值的比较。很多人会记反了。使用反了，shell可能得不到正确的结果。

3.大于符号或小于符号必须要转义，否则会被理解成重定向。



### 5. (( ))及[[ ]] :    逻辑运算符和字符串比较

* **分别是[ ]的针对数学比较表达式(())和字符串表达式[[]]的加强版。**
* (()) : 提供了<   >    ||  等对比符号