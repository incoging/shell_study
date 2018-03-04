##6.变量和引用
###6.1 变量
>变量分为3种：   
* 本地变量 &nbsp;&nbsp;--在用户当前的Shell生命周期内有效，类似于C++，Java中的局部变量
* 环境变量 &nbsp;&nbsp;--在用户登陆到注销之前的所有编辑器、脚本、程序和应用中都有效。
* 位置变量 &nbsp;&nbsp;--用于向Shell脚本传递参数，是只读的。

####6.1.1 变量的赋值和替换
> 引用变量值就成为变量替换(个人理解就是把变量名变成变量所对应的值)，$符号是变量替换符号。
即 variable是变量名，$variable就是变量的值，同时也可以使用${variable}.

> 利用unset命令可以清除变量的值：
```bash
unset 变量名
eg: 
unset variable4    #清除variable4变量
echo $variable4    #将输出空白行，表示variable4变量未被初始化
```
>变量赋值的模式：   
variable=value &nbsp;&nbsp;将value值赋给variable   
variable+value &nbsp;&nbsp;对已赋值的variable，重设其值   
variable?value 或 variable:?value   &nbsp;&nbsp;对未赋值的variable，显示系统错误信息   
variable:=value &nbsp;&nbsp;对未赋值的variable,将value值赋给它   
variable:-value &nbsp;&nbsp;对未赋值的var11iable,将value值赋给它，但value值不存储到variable对应的地址空间

* 将变量设为只读
```bash
variable=value  # 先对一个变量进行赋值
readonly variable  #将variable变量设置为只读 
```
* 查询系统中所有的只读变量
```bash
readonly  # 直接输入readonly就可以
```
####6.1.2 无类型的Shell脚本变量
> Shell脚本变量是无需声明类型的，默认是字符型。字符型变量具有一个整型值，0.
Shell会根据上下文自动判断该变量的类型：若变量只包含数字，则Shell认定该变量为数值型，反之为字符串。

* Shell可以不预先定义变量而直接使用它

####6.1.3 环境变量
1.定义和清除环境变量
```bash
ENVIRON-VARIABLE=value  # 环境变量赋值
export ENVIRON-VARIABLE  # 声明环境变量  
```
> 环境变量名称一般为大写字母组成，也是无类型的变量，它的值适用于所有由登录进程所产生的子进程。

* 定义一个环境变量(一个例子)
```bash
APPSPATH=/usr/local  # 为APPSPATH赋值
export APPSPATH  # 将APPSPATH声明为环境变量
echo $APPSPATH
/usr/local
```
* 列出系统中所有的环境变量及清除环境变量
```bash
env  # 列出系统中所有的环境变量
unset variable  # 清除环境变量variable
```

2.重要的环境变量
* PWD和OLDPWD
> PWD：记录当前的目录路径   
OLDPWD: 记录旧的工作目录
```bash
echo $PWD
echo $OLDPWD
```

* PATH
```bash
echo $PATH  
/usr/local/sbin:/usr/local/bin  # PATH变量的内容

export PATH="/new directory":$PATH  #在PATH中添加新的目录
```
* HOME
> HOME记录当前用户的根目录，由/etc/passwd的倒数第2个域决定，HOME目录用于保存用户自己的文件，
一般来说，非根用户的HOME目录都存放在/home目录下，通常以用户名命名。

* SHELL
> SHELL变量保存默认的Shell，默认的值为/bin/bash，表示当前的Shell是bash Shell,
如果有必要用另一个Shell，则需要重置SHELL变量值。

* USER和UID
> USER表示已登录用户的名字，UID表示已登录用户的ID

* 