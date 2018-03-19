### 7. 退出、测试、判断及操作符
#### 7.1 退出状态
```bash
ls    
echo $?    #0 运行成功。
abcjifw  
echo $?    #127 未找到要运行的命令
```
* 退出状态及意义：   
0 &nbsp;&nbsp;&nbsp;&nbsp;表示运行成功，未遇到任何问题   
1~125 &nbsp;&nbsp;表示运行失败，脚本命令、系统命令错误或参数传递错误   
126 &nbsp;&nbsp;找到了该命令但无法执行   
127 &nbsp;&nbsp;未找到要运行的命令   
\>128 &nbsp;&nbsp;命令被系统强制结束

#### 7.2 测试
##### 7.2.1 测试结构
>测试命令用于测试表达式的真假，   
条件为真：返回0值。   
条件为假：返回一个非0的整数值   
   
两种测试命令方式：
```bash
test expression
```
```bash
[ expression ]   #注意 [ 之后，和 ] 之前的空格不可少
```
##### 7.2.2 整数比较运算符
命令：
```bash
test "num1" numeric_operator "num2"
或者
[ "num1" numeric_operator "num2" ]
```
* option:   
num1 -eq num2 &nbsp;&nbsp;num1等于num2，测试结果为0   
num1 -ge num2 &nbsp;&nbsp;num1大于或等于num2，测试结果为0   
num1 -gt num2 &nbsp;&nbsp;&nbsp;num1大于num2，测试结果为0   
num1 -le num2 &nbsp;&nbsp;&nbsp;num1小于或等于num2，测试结果为0   
num1 -lt num2 &nbsp;&nbsp;&nbsp;&nbsp;num1小于num2，测试结果为0   
num1 -ne num2 &nbsp;&nbsp;num1不等于num2，测试结果为0   
```bash
num1 = 15
["$num1" -eq 15 ]   #测试num1是否等于15
echo $?              #0 退出状态为0，表示num1等于15
```
>Notice: 整数比较运算符不适合浮点数比较，若想比较浮点数，则需要特定的函数

##### 7.2.3 字符串运算符
命令：
```bash
test string
```
* option:   
string  &nbsp;&nbsp;测试字符串string是否不为空   
-n string &nbsp;&nbsp;测试字符串string是否不为空   
-z string &nbsp;&nbsp;测试字符串string是否为空   
string1 = string2 &nbsp;&nbsp;测试字符串string1是否与string2相同   
string1 !=string2 &nbsp;&nbsp;测试字符串string1是否与string2不相同
