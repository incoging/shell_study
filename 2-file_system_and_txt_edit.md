#Linux文件系统和文本编辑器
##2.1 用户和用户组管理
三类用户：
```
1.root用户：唯一，真实，可以登录系统，拥有最高权限
2.虚拟用户：伪用户，不能登录系统，系统自身拥有，eg:bin, daemon, adm, ftp, mail.
3.普通真实用户：能登录系统，只能操作根目录内容，部分权限，系统管理员自行添加。
```
###常用命令：
1.useradd  -- 用户账号添加
```
useradd [option] [username]
```
* option   
-m 若使用的目录不存在，则自动建立(实践中需要加这个选项在home中才有主目录)
>执行该命令后，会：  
* 在/etc/passwd文件中添加一行记录
* 在/home目录下创建新用户的主目录，并将/etc/skel目录中的文件复制到该目录中   

>Notice：此时用户还无法登陆，需要用passwd命令为其设置口令后才能登陆。
```
sudo tail -l /etc/passwd    查看新创建的用户和新创建的用户主目录
sudo tail -l /etc/shadow    查看创建用户的密码
```
2.usermod -- 修改用户账号   
```
usermod [option] [username]
```
* option   
-d [directory]  修改用户登陆时的目录   
-g [group]      修改用户所属的群组   
-p [password]   修改用户密码   
-l [login_name] 变更用户登陆时的名称
* username是需要修改的用户名

3.userdel -- 删除用户账号命令   
```
userdel [option] [username]
```
>只有一个可选命令-r   
当加上-r后在删除用户的同时也会一并删除存储在/home目录下的该用户目录文件

4.passwd -- 用户口令管理
```
passwd [option] [username]
```
* eg: sudo passwd username


###2.1.2 用户组管理命令
1.groupadd -- 用户组添加
```
groupadd [option] [groupname]          [groupname]是将要创建的用户组名
```
* option   
-g GID 除非使用-o参数，否则GID值必须是唯一且数值不可为负，预设值以/etc/login.defs为准   
-o 运行GID不唯一

>eg: sudo groupadd -g 666 wangyq      添加了一个GID为666的用户组wangyq
tail -l /etc/group  进行查看

2.groupmod -- 用户组修改
```
groupmod [option] [groupname]
```

3.groupdel -- 用户组删除命令
```
groupdel [groupname]
```
##文件和文件目录
###2.2.1文件操作常用命令
1.ls -- 文件清单命令   
```
ls [option] [directory]
```
* option   
-l 显示详细信息   

2.cp -- 文件复制
```
cp [option] [source] [destination]
```
* option   
-i 拷贝到目的文件夹中如果有相同名字，会提示，交互式复制   


3.mv -- 文件移动
````
mv [option] [source] [destination]
````
>Notice:mv是先复制，再将源文件删除，从而导致该文件的链接丢失。
mv 也可用于目录的重命名，mv abc ABC 将abc文件改名为ABC

4.rm -- 删除文件
```
rm [option] [filename or directoryName]
```
* option   
-f 忽略不存在的文件，从不给出提示   
-r 指示rm将目录中的全部文件及子目录全部删除   
-i 进行交互式删除

###2.2.2目录操作命令
1.mkdir -- 创建目录命令。
```
mkdir [option] [directoryName]
```
* option   
-m 对新建目录设置存取权限  
-p 一次建立多个目录   
-v 每次创建新目录都显示信息

>eg:mkdir -m 777 tsk 创建目录并指定权限(让所有用户都拥有读，写，执行权限)

2.rmdir -- 删除目录
```
rmdir [option] [directoryName]
```
* option   
-p 递归删除，当子目录删除后，其父目录为空时也会一同被删除。

3.cd -- 目录切换命令
```
cd [directoryName]
```
* option   
cd 返回登录目录   
cd ~ 返回登录目录

###2.2.3 文件和目录权限管理
```
drwxr-xr-x. 1 root root 225 03-15 13:14 abc.txt
```
> 3个是一组，分别是自己对文件的读写、执行权限，同组用户的读写、执行权限，系统其他用户的读写、执行权限

1.chmod -- 更改文件(目录)权限
```
chmod [userType] [signal] [type] [filename]
```
* option   
> u 表示用户，即文件或者目录的所有者   
g 表示同组，即文件属主的同组用户   
o 表示其他用户   
a 表示所有用户
>>  添加某个权限 +   
>>  取消某个权限 -  
>>  赋予给定权限并取消其他所有的权限 =  

>eg: chmod u+x, g+w test1

权限的数字设定法：
0表示没有权限，1表示可执行权限，2表示可写权限，4表示可读权限，然后将其相加。所以数字属性
的格式应为3个从0到7的八进制数，其顺序是u, g, o.
>eg: chmod u+x, g+w test1 等价于 chmod 764 test1  (因为可执行就伴随着可读可写，可写就伴随着可读)


##2.3文本编辑器
###2.3.1 vim编辑器
1.vim基本操作
```
vim [option] [filename...]
```
* option   
-c command 在对文件进行编辑前先执行command命令   
-r filename 恢复文件filename   
-R 以只读方式编辑文件

2.vim工作模式
* 一般模式
> 1.将用户输入看作命令   
2.允许用户移动光标   
3.最后一行显示文件名，文件包含的字符数和字节数   
4.此模式下按冒号，冒号后输入保存，退出等命令。   
>> w  --将编辑器的文本存储   
q  --离开vim文本编辑器   
q！  --修改过文本，但是不想保存修改，强制退出   
wq -- 存储并离开vim编辑器   
> * 一般模式下其他命令：   
h, j, k, l  将光标向左，下，上，右移动。   
{ ---将光标移动到段落开头   
} -- 将光标移动到段落末尾   
( -- 将光标移动到句子开头   
) -- 将光标移动到句子末尾   
:n -- 将光标移动到行n   
x -- 删除当前位置的字符   
dd -- 删除光标所在的整行文本   
d$ -- 删除当前光标位置到该行结束的所有文本
dw -- 从当前光标向前删除单词
* 插入模式
> 一般情况下按i, o, a等字母都可以进入编辑模式(此时下面会显示INSERT字样)。
按Esc键则从编辑模式退回到一般模式   
* 底行工作模式   
>底行模式下用户输入的任何字符串都会当做命令，按下反斜杠(/)，冒号(:),问号(?)都可进入底行工作模式。
>>该模式下可以实现搜索和替换功能，命令如下：   
/word  --  自当前光标位置向下搜索名字为word的字符串   
?word  --  自当前光标位置向上搜索名字为word的字符串
:n1,n2s/word1/word2/g -- 在n1和n2行之间搜索名字为word1的字符串，并全部替换为word2，如果不加最后的g
那么，只替换第一个。
:1,$s/word1/word2/g  -- 在第一行和最后一行之间搜索名字为word1的字符串，并全部替换为word2。   

3.vim的配置文件
> vimrc是vim的一个配置文件，是对vim的风格等进行说明，该目录位置可以通过在vim的的底行模式下输入echo $VIM命令来查看，
但通常在/usr/share/vim目录下，可以通过vim或者Gedit进行修改。其中重要的配置有：
* set showmatch  为文件中自动显示匹配的括号   
set nu    在文件中显示行号   
set autoindent  编辑时自动缩进。   
set cindent  编辑时按照C语言风格自动缩进，但是这个缩进会很大。   
set mouse    打开对鼠标的支持，滚轮和单击均可。



