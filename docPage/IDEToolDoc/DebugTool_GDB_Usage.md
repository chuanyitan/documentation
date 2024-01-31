# GDB

# 1.Prepare

A GDB class video(zhanghaiyang):
https://edu.csdn.net/course/detail/28981?spm=1003.2449.3001.8295.7

The source code sample:
https://github.com/SimpleSoft-2020/gdbdebug

GDB user guide:
https://sourceware.org/gdb/
https://sourceware.org/gdb/download/onlinedocs/gdb.pdf

An online debug link:
https://www.onlinegdb.com/

另外，可以参考《C/C++代码调试的艺术》 主要讲怎么进行调试代码


# 2.Usage

## 2.1 GDB usage 
demo 参考 section1 和 section2  
### 2.1.1 启动调试并传入参数
demo section1
有三种调用方法：
* gdb --arg <exe>
* gdb 之后 set args <args>
* gdb 之后 r <args>

### 2.1.2 附加到进程
适用于进程已启动，没办再用gdb再去启动，那样是启动一个新的进程
* gdb attach <pid>
* gdb --pid <pid>

pa aux 查找已经跑起来的进程的pid. 执行上面的指令后，就可以正常的打断点，查变量信息等等


### 2.1.3 单步执行
 **step-over, 遇到函数跳过函数，不会进到函数里面单步执行**
* next
* n

查看变量值，比如 p number  打印出变量的值

### 2.1.3 逐语句执行
 **step-into, 遇到函数进入到函数内，一行行执行**
* step
* s

### 2.1.4 退出当前函数
使用s命令进入了函数，但想跳过，使用finsh

* finish

使用l, 可以查看源码

### 2.1.5 退出调试
Note: * 如果使用的attach进入调试，再使用detach退出，并不会影响原先程序的正常运行
      * 如果是用gdb <exe> ，q指令就退出调试，结束程序（kill），
      * 也可以在gdb <exe>, 之后使用detach退出，只是结束了调试。从调试中分离出去，进程还在后台运行。
* detach
* quit
* q

## 2.2 Debug method

### 2.2.1 设置断点
demo section3
* break/b 文件名：行号； 在源码某一行设置断点
* b 函数名； 为函数设置断点（存在函数的多次调用，以及虚函数继承后，同名不同参数的函数，这样会把所有同名的函数都打断点。同名函数怎么处理）
* rb 正则表达式； 为满足正则表达式的函数设置断点
* b 断点 条件; 设置断点条件
* tb 断点； 设置临时断点
* delete； 删除所有的断点

```bash
$ gdb section3

(gdb) list

(gdb) b main.cpp:38

(gdb) b testfun
Breakpoint 1 at 0x400ea4: testfun.(2 locations) 两次调用

(gdb) i b 显示打了哪些断点

(gdb) rb work 只有含有work的函数都打了断点

(gdb) b main.cpp:14 if i==90 则设置断点，当i=90时才停下来，i为此处的某个变量值

(gdb) p i 打印变量i的值

(gdb) delete

(gdb) tb main.cpp:14 临时断点只会命中一次，如果是循环，下次就不会在停下来了

```

指令 i b ;查询当前打了哪些断点  
r c的指令的区别？？

### 2.2.2 管理断点

* i b; 查看所有断点
* disable/enable 断点编号； 禁用、启用断点
* delete 断点； 删除断点

```bash

(gdb) info breakpoints (info break)
(gdb) i b 查出的前面的数字列表即断点编号
(gdb）delete 5
(gdb) disable 1断点还在，直接此处断点被设置成disabale,i b可以查看
(gdb）delete 删除所有断点

```
### 2.2.3 查看/修改变量
demo section4

查看变量
* info args; 查看函数参数
* print 变量名
* p     变量名
* set print null-stop ; 设置字符串显示规则，字符串后面的'\0'就不在显示
* set print pretty ; 优化显示结构体，一行行显示
* set print arry on ; 优化显示数组，一行行显示
* gdb的内嵌函数，例如： sizeof, strlen, strcmp等

修改变量
* p 变量=新值

```bash
(gdb) p argc
$1 = 1
(gdb) p argv
$2 = (char **) 0x222222
(gdb) p argv[0]
(gdb) info args (i args)
一般使用n,往下单步，但不进入函数
(gdb) n
(gdb) p sizeof(int)
(gdb) p sizeof(long)
(gdb) p strlen(name)   变量的长度
(gdb) p sizeof(test)   获取结构体的大小 

(gdb) p i=10 直接改了变量的值成10
(gdb) p strcpy(test.name, "soft")
(gdb) p test

如果是要调试某个函数调用的参数
(gdb) b test_work  函数打断点
(gdb) list
(gdb) next
(gdb) info args  会把带的参数全打出来
然后这里就可以直接
(gdb) print arg1=xxx  对传进来的参数直接修改，或者在执行某步骤后，开始用次指令修改变量值
```

### 2.2.4 查看/修改内存
demo section5

此方法的使用场景？ 怎么使用？
会涉及大小端的问题

* x /选项 内存地址
eg:
x /s str
x /d
x /4d
x /16s 结构体变量地址

关于大小端：
https://blog.csdn.net/zhanghaiyang9999/article/details/110957728




