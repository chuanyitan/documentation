# GDB

# 1.Prepare

A GDB class video
https://edu.csdn.net/course/detail/28981?spm=1003.2449.3001.8295.7

The source code sample.
https://github.com/SimpleSoft-2020/gdbdebug

GDB user guide:
https://sourceware.org/gdb/
https://sourceware.org/gdb/download/onlinedocs/gdb.pdf

An online debug link:
https://www.onlinegdb.com/


# 2.Usage
demo 参考 section1 按到section1  
## 2.1 启动调试并传入参数
demo section1
有三种调用方法：
* gdb --arg <exe>
* gdb 之后 set args <args>
* gdb 之后 r <args>

## 2.2 附加到进程
适用于进程已启动，没办再用gdb再去启动，那样是启动一个新的进程
* gdb attach <pid>
* gdb --pid <pid>

pa aux 查找已经跑起来的进程的pid. 执行上面的指令后，就可以正常的打断点，查变量信息等等


## 2.3 单步执行
 **step-over, 遇到函数跳过函数，不会进到函数里面单步执行**
* next
* n

查看变量值，比如 p number  打印出变量的值

## 2.3 逐语句执行
 **step-into, 遇到函数进入到函数内，一行行执行**
* step
* s

## 退出当前函数
使用s命令进入了函数，但想跳过，使用finsh

* finish

使用l, 可以查看源码

## 2.5 退出调试
Note: * 如果使用的attach进入调试，再使用detach退出，并不会影响原先程序的正常运行
      * 如果是用gdb <exe> ，q指令就退出调试，结束程序（kill），
      * 也可以在gdb <exe>, 之后使用detach退出，只是结束了调试。从调试中分离出去，进程还在后台运行。
* detach
* quit
* q
