### shell

**$**

```
$# : 获得脚本传入参数的个数。
$? :  获得上面函数或脚本执行之后的返回值（或者叫退出码）。（默认成功执行之后返回0）
$$ :  获得当前所在脚本的进程ID号。（通常会被作为生成唯一标识的一种手段）
$! : 获得最后一个后台进程PID。（通常这个可以用来结合后台任务& 来完成一定时间内运行任务的需求：cmd & (sleep 10; kill -9 $! 2>/dev/null)）
$() : 相当于``，也用作命令替换，即先执行括号中的命令，然后返回命令执行的结果。
$*/$@ : 获取传入脚本的全部参数。但两者在加上引号之后的行为有些差别，后者会把传入参数以空格为分割拆分成多个，前者则还是作为一个整体看待。
```



### sort

-u 去重

-b 忽略空格

-f 忽略大小写

```
➜  ~ sort -fbu a
a
a.go
Applications
  Backup
code
Desktop
Documents
Downloads
IdeaProjects
Library
```



### awk

见 man awk

 awk [ -F fs ] [ -v var=value ] [ 'prog' | -f progfile ] [ file ...  ]



##### -F 命令支持正则表达式

![image-20190727120256539](/Users/lixia/Library/Application Support/typora-user-images/image-20190727120256539.png)



##### **内置函数**

length

```shell
➜  ~ ls |awk '{print $0,length}'|sort -nk 2
a.go 4
code 4
test 4
z.sh 4
Music 5
lixia 5
vimrc 5
Backup 6
Movies 6
Public 6
Desktop 7
Library 7
Postman 7
Pictures 8
Documents 9
Downloads 9
pip-1.5.4 9
solarized 9
Applications 12
IdeaProjects 12
vimrc.bundles 13
vim.backup.tgz 14
```



rand

```
➜  ~ awk '{print rand()}' a.go
0.924046
0.593909
0.306394
0.578941
0.740133
0.786926
0.43637
0.332195
0.77888
0.100887
0.785084
0.835159
0.761209
0.496077
0.426298
```





### egrep

**参数**

```
-v: 逆反模示, 只输出"不含" RE 字符串之句子.
-r: 递归模式, 可同时处理所有层级子目录里的文件.
-q: 静默模式, 不输出任何结果(stderr 除外. 常用以获取 return value, 符合为 true, 否则为false .)
-i: 忽略大小写.
-w: 整词比对, 类似 <word> .
-n: 同时输出行号.
-c: 只输出符合比对的行数.
-l: 只输出符合比对的文件名称.
-o: 只输出符合 RE 的字符串. (gnu 新版独有, 不见得所有版本都支持.)
-E: 切换为 egrep .
```

**正则表达式**

```
1） ^ ：例如 ^word 以word开头的内容
2）$ ：例如 word$ 以word结尾的内容
3）^$ ：空行
4）. ：表示且只能代表任意一个字符（当前目录，加载文件）
5）\ ：转移字符，让有着特殊身份的字符，变回原来的字符。
6）* ：重复0个或多个前面的一个字符，不代表所有。
7）.* ：匹配所有的字符。^.* 任意多个字符开头。
8）[abc] ：匹配字符集合内任意一个字符[a-z]
9）[^abc] ：^在中括号表示非，表示不包含a或者b或者c
10）{n,m} ：前一个字符，重复n到m次
		{n,} ：至少N次，多了不限。
		{n} ：N次
		{,m} ：最多m次，少了不限。
11） () 子表达式
注意：grep 要对{转义} \{\} ，egrep (grep -E )不需要转义
```

**egrep 与 grep 区别**

```
egrep 和grep -E一样
egrep 和 grep的功能几乎一样，但是使用的是拓展的正则表达式
拓展正则表达式没有.*了，然后就是少了使用\
比如说
？：0次或者1次                   在grep里头要写\?
+：其前面的字符至少出现1次          \+
{m}:其前的字符出现m次             \{m\}
然后加入了或的一个逻辑
a|b:a或者b
C|cat:C或者cat
(C|c)at:cat或者Cat
```

**位置锚定**

```
^:行首锚定：用于模式的最左侧
$:行尾锚定：用于模式的最右侧
^patten$:用patten来匹配整行
^$:空白行
^[[:space:]]*$:
\<或者\b:词首锚定，用于单词模式的左侧
\>或者\b：词尾锚定，用于单子的右侧
\<patten\>:匹配完整的单词
```

**字符类**

```
放在括号内的表达式，即包在 "[:" 和 ":]" 之间的字符类的名字，它表示的是属于此类的所有字符列表。标准的字符类名称如下：

[:alnum:] - 字母数字字符
[:alpha:] - 字母字符
[:blank:] - 空字符: 空格键符 和 制表符
[:digit:] - 数字: '0 1 2 3 4 5 6 7 8 9'
[:lower:] - 小写字母: 'a b c d e f g h i j k l m n o p q r s t u v w x y z'
[:space:] - 空格字符: 制表符、换行符、垂直制表符、换页符、回车符和空格键符
[:upper:] - 大写字母: 'A B C D E F G H I J K L M N O P Q R S T U V W X Y Z'
[:graph:] - 匹配一个看的见的字符，不包括空白字符
[:print:] - 匹配一个可以打印的字符
[:punct:] - 匹配一个标点符号
[:xdigit:] - 匹配一个十六进制数字，即0-9,a-f,A-F
```

**Example**

查找包含两到三位数字的行

```
➜  ~ echo "123asdf\nasdfas233sds123\n123\n asdfsad\nasdf22\n12345\n" |egrep '[0-9]{2,3}\b'
asdfas233sds123
123
asdf22
12345
➜  ~ echo "123asdf\n asdfas233sds123\n123\n asdfsad\nasdf22\n12345\n" |egrep '\b[0-9]{2,3}\b'
123
➜  ~ echo "123asdf\n asdfas233sds123\n123\n asdfsad\nasdf22\n12345\n" |egrep -w '[0-9]{2,3}'
123
➜  ~ echo "123asdf\n asdfas233sds123\n123\n asdfsad\nasdf22\n12345\n" |egrep '[0-9]{2,3}\b'
 asdfas233sds123
123
asdf22
12345
```

查找包含两位数字的行

```
➜  ~ echo "12\n123asdf\n asdfas233sds123\n123\n asdfsad\nasdf22\n12345\n" |egrep '\b[0-9]{2}\b'
12
```

**查找空行**

```
➜  ~ grep '^[[:space:]]*$' -n a
2:
3:
4:
5:
6:
```

**删除空行**

```
➜  ~ grep '^[[:space:]]*$' -v a
Applications
Backup
Desktop
Documents
Downloads
IdeaProjects
Library
Movies
Music
Pictures
Postman
Public
```

**实用正则**

```
#匹配QQ号码，第一位不能是0，5位以上的数字。
egrep "[1-9][0-9]{4,}" testfile

#匹配IP地址，共4组数字，用"."隔开
egrep "^([0-9]{1,2}|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.   #第一组数字
        ([0-9]{1,2}|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.   #第二组数字
        ([0-9]{1,2}|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.   #第三组数字
        ([0-9]{1,2}|1[0-9]{2}|2[0-4][0-9]|25[0-5])$"   #第四组数字
        testfile 

#匹配邮箱地址
egrep "^[a-z0-9]([a-z0-9]*[-_]?[a-z0-9]+)*@
       ([a-z0-9]*[-_]?[a-z0-9]+)+[.][a-z]{2,3}([.][a-z]{2})?$"
```
