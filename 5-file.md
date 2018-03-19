### 5.文件的排序、合并和分割
#### 5.1 sort命令
```
sort [选项] [输入文件]
```
* 选项内容：   
-c &nbsp;&nbsp; 测试文件是否已经被排序   
-k &nbsp;&nbsp; 指定排序的域   
-m &nbsp;&nbsp; 合并两个已排序的文件   
-n &nbsp;&nbsp; 根据数字大小进行排序   
-o [输出文件] &nbsp;&nbsp; 将输出写到指定文件，相当于将输出重定向到指定的文件   
-r &nbsp;&nbsp; 将排序的结果逆向显示   
-t &nbsp;&nbsp; 改变域分隔符(默认是空格)  
-u &nbsp;&nbsp; 去除结果中的重复行

1.-t &nbsp;选项
```
sort -t: CARGO.db
```
> 假设CARGO.db文件中有Acer:Taiwan:8000:2010:PT210 此时指定了以:作为分割域，这条记录将会有5个域，
若不指定:,以默认的空格为域分隔符的话，这条记录将会只是有1个域，即整个记录为一个域

2.-k &nbsp;选项
```
sort -t: -k3 CARGO.db
```
> sort排序默认是以第一域进行排序，上面的例子将会按照第3域进行排序。

3.-n &nbsp;选项
```
sort -t: -k3n CARGO.db   #根据第3域的数字大小进行排序
```
4.-o &nbsp;选项
```
sort -t: -k3n -o sort_CARGO.db CARGO.db   #sort_CARGO.db 为重定向到的文件
```
5.-m &nbsp;选项
```
sort -t: -m abc.db asd.db 
```
> -m是将两个已排好序的文件进行合并，这两个文件必须是已经排好序的，不然这个命令无效。

#### 5.2 uniq命令
```
uniq [选项] [文件]
```
> uniq用于去除文本中的重复行，和sort -u 的命令类似，区别是：
uniq去除文本中连续的重复行，若隔行后还有重复的，将不会被去除
sort -u [文件] 去除文本中所有的重复行。最后只保留一个。

* 选项及其意义：   
-c &nbsp;&nbsp;打印每行在文本中重复出现的次数。   
-d &nbsp;&nbsp;只显示有重复的记录，每个重复记录只出现一次   
-u &nbsp;&nbsp;只显示没有重复的记录。和-d是对立的。

#### 5.3 jion命令
> 用于实现两个文件中记录的连接操作，将两个文件中具有相同域的记录选择出来，
再将这些记录所有的域放到一行(包含来自两个文件的所有域)
```
file A:
Liu:Shanghai Jiaotong University
file B:
Liu:Tea

jion -t: A B
Liu:Shanghai Jiaotong University:Tea
```
pass--------------------------------

#### 5.4 cut命令
> cut用于从标准输入或者文本文件中按域或者行提取文件。
```
cut [选项] [文件]
```
* 选项及意义：
-c &nbsp;&nbsp;指定提取的字符数，或字符范围   
-f &nbsp;&nbsp;指定提取的域数，或域范围
-d &nbsp;&nbsp;改变域分隔符
> eg:   
cut -c3 TEACHR.db     #提取TEACHR.db的第3个字符   
cut -c1-5 TEACHR.db     #提取TEACHR.db的第1-5个字符   
cut -d: -f1 TEACHR.db     #提取TEACHR.db的第1个域内的内容
cut -d: -f1,4 TEACHR.db     #提取TEACHR.db的第1和第4个域内的内容

#### 5.5 paste命令

#### 5.6 split命令

#### 5.7 tr命令

#### 5.8 tar命令
```
tar [选项] 文件名或目录名
```
* option   
-c &nbsp;&nbsp;创建新的包   
-r &nbsp;&nbsp;为包添加新的文件   
-t &nbsp;&nbsp;列出包的内容   
-u &nbsp;&nbsp;更新包中的文件，若包中无此文件，则将该文件添加到包中   
-x &nbsp;&nbsp;解压缩文件   
-f &nbsp;&nbsp;使用压缩文件或设备，通常是必选的
-v &nbsp;&nbsp;详细报告tar处理文件的信息   
-z&nbsp;&nbsp;用gzip压缩和解压缩文件，若加上此选项创建压缩包，那么解压缩时也需要加上此选项

```
tar -cf db.all *.db   #将所有.db结尾的文件放入压缩包
tar -tf db.all        #查看db.all压缩包的内容
tar -rf db.all log*   #将以log开头的文件添加到db.all中
```
> 两个Linux系统下加压的通用命令
```
tar -xvf 压缩包名称    #解压非gzip格式的压缩包
tar -zxvf 压缩包名称   #解压gzip格式的压缩包
```

> tar -cf 命令创建的包实际上是将多个文件放到一起，此时文件并没有被压缩。
可以使用gzip命令对文件进行压缩。
```
gzip db.all            #压缩db.all包， 此后db.all变成db.all.gz
tar -zxvf db.all.gz    #解压缩db.all.gz包
```
> gzip也可以将db.all.gz还原到db.all文件，只需要加上-d选项：
```
gzip -d db.all.gz    #将db.all.gz解压缩，此时得到的是包。而 tar -zxvf是直接解压缩得到文件。
```