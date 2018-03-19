#### 玖 变量的高级用法
##### 9.1 内部变量
1.BASH
>BASH记录了bash Shell的路径，通常为/bin/bash，内部变量SHELL就是通过BASH的值确定当前Shell的类型。

2.BASH_SUBSHELL
>记录了子Shell的层次，在bash版本3之后才出现。

3.BASH_VERSINFO
>是一个数组，包含6个元素，表示bash的版本信息：
* BASH_VERSINFO[0] 表示bash Shell的主版本号
* BASH_VERSINFO[1] 表示bash Shell的次版本号
* BASH_VERSINFO[2] 表示bash Shell的补丁级别
* BASH_VERSINFO[3] 表示bash Shell的编译版本
* BASH_VERSINFO[4] 表示bash Shell的发行状态
* BASH_VERSINFO[5] 表示bash Shell的硬件架构

4.BASH_VERSION
>相当于BASH_VERSINFO的前5个信息。

5.DIRSTACK
>记录了栈顶目录值，Linux定义两个操作来维护目录栈
```bash
pushd 目录名  # 将此目录压入栈
popd 目录名  # 将此目录出栈
```

6.GLOBIGNORE(globignore)
>表示进行通配时忽略与GLOBIGNORE通配的文件。
```bash
ls a*
append.sed array.sh andlist.sh
GLOBIGNORE="ar*"
ls a*
append.sed andlist.sh  # 将不再显示以ar开头的文件
```

7.GROUPS
>GROUPS记录当前用户所属的群组，因一个用户可以包含在多个群组内，所以这个
GROUPS是一个数组，记录了当前用户所属的所有群组号。
>Linux管理用户组的文件是/etc/groups，每个群组对应文件中的一行，以冒号分割四个域：
```
群组名:加密后的组口令:群组号:组成员列表
```
eg:
```
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:alloy
```
可以看出，root用户同时属于root,bin,daemon,sys等群组.

8.HOSTNAME
>记录了主机名，主机名是网络配置时必须要设置的参数，一般在/etc/sysconfig/network文件中设置
/etc/hosts文件用于设定IP地址和主机名之间的对应关系，以利于快速从主机名查找到对应的IP地址。

9.HOSTTYPE和MACHTYPE
>两者都用于记录系统的硬件架构
```bash
echo $HOSTTYPE
x86_64
echo $MACHTYPE
x86_64-pc-linux-gnu
```

10.OSTYPE
>记录了操作系统的类型，在Linux中，$OSTYPE=linux

11.REPLY
>1.当read后面没有变量时，read后接收到的值将会存储在REPLY中。   
2.在select中，用来存储用户的选择。
```bash
#selectreply.sh
#!/bin/bash

echo "Pls, choose your profession?"
select var in "Worker" "Doctor" "Teacher" "Student" "Other"
do
echo "The \$REPLY is $REPLY."
echo "Your profession is $var."
break
done

chmod u+x selectreply.sh
./selectreply.sh
Pls, choose your profession?
1) Worker
2) Doctor
3) Teacher
4) Student
5) Other
#? 4  # select命令后，Shell提示符变为#?,该提示符由Shell提示符变量PS3进行设置。
The $REPLY is 4.
Your profession is Student.
```

12.SECONDS
>记录脚本从开始运行到结束所耗费的时间，以秒为单位。

13.SHELLOPTS
>记录处于"开"状态的Shell选项，它是一个只读变量。
* 一个Shell选项有"开"和"关"两种状态，set命令用于打开或关闭选项。
```bash
set -o optionname  # 打开名称为optionname的选项
set +o optionname  # 关闭名称为optionname的选项
```

14.SHLVL
>记录了bash Shell的嵌套层次，启动第一个Shell，$SHLVL=1,在这个Shell中执行脚本，
脚本中的$SHLVL=2，依次类推。

15.TMOUT
>用于设置Shell的过期时间，当TMOUT不为0时，Shell在TMOUT秒后自动注销。

####9.2 字符串处理
1.求取字符串长度
```bash
${#string}  # 方法一
expr length "$string"  # 方法二，若string中包含空格，则引号("")是必须的
```

2.expr index
```bash
expr index $string $substring
```
>在字符串string上匹配substring中的字符第一次出现在string中的位置。若无，返回0
```bash
string="Speeding up"
expr index "$string" dp  # 因为string有空格，所以加引号
2  # 尽管求取时d在前，但是string中p第一次出现，所以返回2
```

3.expr match
```bash
expr match $string $substring
```
>从string的开头，匹配substring，返回匹配到的substring字符串的长度。若匹配不到，返回0

4.抽取子串
```bash
#{string:position}
#{string:position:length}
```
> 第一行命令是从position位置开始抽取string的子串，(这里string从0开始计数，和上面求索引时不一样)   
第二行命令是取出从position位置开始长度为length的子串。

从右边开始计数求取子串
```bash
#{string: -position}  # 冒号和短横线符号之间有一个空格符
#{string:(position)}  # 冒号和左括号之间不必由空格符
```

expr substr求取子串
```bash
expr substr $string $position $length  # 这个方法string下标从1开始标号，且参数length不可少
```

5.删除子串
```bash
${string#substring}  # 删除string开头处与substring匹配最短子串
${string##substring}  # 删除string开头处与substring匹配最长子串
```
eg:
```bash
the_string="20180303 Reading Hadoop"
echo ${the_string#2*3}
03 Reading Hadoop
echo ${the_string##2*3}
 Reading Hadoop
```
从结尾处开始删除。
```bash
${string%substring}  # 删除string结尾处与substring匹配最短子串
${string%%substring}  # 删除string结尾处与substring匹配最长子串
```
eg:
```bash
the_string="20180303 Reading Hadoop"
echo ${the_string#a*p}
20180303 Reading H
echo ${the_string##a*p}
20180303 Re
```

6.替换子串
```bash
${string/substring/replacement}  # 仅替换第一次与substring相匹配的子串
${string//substring/replacement}  # 替换所有与substring相匹配的子串

${string/#substring/replacement}  # 替换string开头处与substring相匹配的子串
${string/%substring/replacement}  # 替换string结尾处与substring相匹配的子串
```
#### 9.3 有类型变量
declare和typeset是等价的命令来指定变量类型。
```bash
declare [选项] 变量名
```
*option:
> -r 将变量设置为只读   
-i  将变量定义为整数型   
-a  将变量定义为数组   
-f  显示此脚本前定义过的所有函数名及其内容   
-F  仅显示此脚本前定义过的所有函数名   
-x  将变量声明为环境变量

变量执行算数运算--双括号法
```bash
result=$((var1*var2))
```
##### 9.5.2 bc运算器
```bash
bc  # 启动bc运算器
quit  # 退出bc运算器

bc -q # 启动bc运算器,不显示bc运算器信息
```
在脚本中使用bc运算器
```bash
variable=`echo "options; expression" | bc`  # options一般用来设定scale，scale是精度设置
```
eg:
```bash
var1=20
var2=`echo "scale=5; $var1 ^ 2 " | bc`  # scale=5,若结果有小数，则结果将保留5位小数
```