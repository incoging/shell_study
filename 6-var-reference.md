### 6.变量和引用
#### 6.1 变量
>变量分为3种：   
* 本地变量 &nbsp;&nbsp;--在用户当前的Shell生命周期内有效，类似于C++，Java中的局部变量
* 环境变量 &nbsp;&nbsp;--在用户登陆到注销之前的所有编辑器、脚本、程序和应用中都有效。
* 位置变量 &nbsp;&nbsp;--用于向Shell脚本传递参数，是只读的。

##### 6.1.1 变量的赋值和替换
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
##### 6.1.2 无类型的Shell脚本变量
> Shell脚本变量是无需声明类型的，默认是字符型。字符型变量具有一个整型值，0.
Shell会根据上下文自动判断该变量的类型：若变量只包含数字，则Shell认定该变量为数值型，反之为字符串。

* Shell可以不预先定义变量而直接使用它

##### 6.1.3 环境变量

1.定义和清除环境变量

1.1 控制台中：
```
$PATH="$PATH:/my_new_path"    （关闭shell，会还原PATH）
```
应用举例：
```
如果想把当前路径加入到环境变量中去，就可以这样做：
$  PATH ="$PATH:."
这样运行自己编写的shell脚本时就可以不输入./了
```
1.2 修改profile文件：
```
$sudo gedit /etc/profile
```
在里面加入:
```
exportPATH="$PATH:/my_new_path"
```

1.3 修改.bashrc文件：
```
$ sudo gedit /root/.bashrc
```
在里面加入：
```
export PATH="$PATH:/my_new_path"
```
后两种方法一般需要重新注销系统才能生效

* 上面的过程等价于：
```bash
ENVIRON-VARIABLE=value  # 环境变量赋值
export ENVIRON-VARIABLE  # 声明环境变量  
```
> 环境变量名称一般为大写字母组成，也是无类型的变量，它的值适用于所有由登录进程所产生的子进程。

在终端查看定义的环境变量：
```
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

##### 6.1.4 环境变量文件
环境变量配置文件分为两种：系统级文件和用户级文件

1.系统级文件

1.1 **/etc/profile**:在登录时,操作系统定制用户环境时使用的第一个文件，
为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行。 **是所有用户的环境变量**
并从/etc/profile.d目录的配置文件中搜集shell的设置。这个文件一般就是调用/etc/bash.bashrc文件。

1.2 **/etc/bash.bashrc**：系统级的bashrc文件，为每一个运行bash shell的用户执行此文件.当bash shell被打开时,
该文件被读取.

1.3 **/etc/environment**: 在登录时操作系统使用的第二个文件,系统在读取你自己的profile前,
设置环境文件的环境变量。**是系统的环境变量**

* 对整个系统而言，是**先执行/etc/environment，后执行/etc/profile。**

例子：
先将export LANG=zh_CN加入/etc/profile ,退出系统重新登录，登录提示显示英文。将/etc/profile中的export LANG=zh_CN删除，
将LNAG=zh_CN加入/etc/environment，退出系统重新登录，登录提示显示中文。
> 解析：
/etc/environment是设置整个系统的环境，而/etc/profile是设置所有用户的环境，前者与登录用户无关，后者与登录用户有关。
系统应用程序的执行与用户环境可以是无关的，但与系统环境是相关的，所以当你登录时，你看到的提示信息，比如日期、
时间信息的显示格式与系统环境的LANG是相关的，缺省LANG=en_US，如果系统环境LANG=zh_CN，则提示信息是中文的，
否则是英文的。

* 而对于登陆系统时shell读取的顺序是
/etc/profile ->/etc/enviroment -->$HOME/.profile-->$HOME/.env

> Notice: 如果同一个变量在用户环境(/etc/profile)和系统环境(/etc/environment) 有不同的值那应该是以用户环境为准.

2.用户级文件

2.1 **~/.profile**: 在登录时用到的第三个文件 是.profile文件,每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件。

2.2 **~/.bashrc**:该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。不推荐放到这儿，因为每开一个shell，这个文件会读取一次，效率 上讲不好。

2.3 **~/.bash_profile**：每个用户都可使用该文件输入专用于自己 使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件。~/.bash_profile 是交互式、login 方式进入 bash 运行的~/.bashrc是交互式 non-login 方式进入 bash 运行的通常二者设置大致相同，所以通常前者会调用后者。

2.4 **~./bash_login**:不推荐使用这个，这些不会影响图形界面。而且.bash_profile优先级比bash_login高。当它们存在时，登录shell启动时会读取它们。

2.5 **~/.bash_logout**:当每次退出系统(退出bash shell)时,执行该文件.

2.6 **~/.pam_environment**：用户级的环境变量设置文件。

另外,/etc/profile中设定的变量(全局)的可以作用于任何用户,而~/.bashrc等中设定的变量(局部)只能继承 /etc/profile中的变量,他们是"父子"关系。


__登录Linux时要执行文件的过程__
在刚登录Linux时，首先启动/etc/profile 文件，然后再启动用户目录下的 ~/.bash_profile、 ~/.bash_login或 ~/.profile文件中的其中一个，
执行的顺序为：~/.bash_profile、 ~/.bash_login、 ~/.profile。如果 ~/.bash_profile文件存在的话，一般还会执行 ~/.bashrc文件。
因为在 ~/.bash_profile文件中一般会有下面的代码：
```
if[ -f ~/.bashrc ] ; then
　../bashrc
fi
```

~/.bashrc中，一般还会有以下代码：
```
if[ -f /etc/bashrc ] ; then
　./bashrc
fi
```
执行顺序为：
/etc/profile -> (~/.bash_profile | ~/.bash_login | ~/.profile) -> ~/.bashrc-> /etc/bashrc -> ~/.bash_logout

##### 系统变量文件作用域

/etc/profile全局的，随系统启动设置【设置这个文件是一劳永逸的办法】

/root/.profile和/home/myname/.profile只对当前窗口有效。

/root/.bashrc和 /home/yourname/.bashrc随系统启动，设置用户的环境变量【平时设置这个文件就可以了】
