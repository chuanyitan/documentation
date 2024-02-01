# Test-development


# 1.Reference

https://pragprog.com/titles/lotdd/modern-c-programming-with-test-driven-development/
  
《C++程序设计实践与技巧 测试驱动开发》
  
《Modern C++ Programming with Test-Driven Development》

test-driven development
测试驱动开发

行为驱动开发

主要学习如何写单元测试，以及培养软件开发的思维

# 2.环境搭建

环境为ubuntu 18.04，要升级c++到g++-9, cmake默认是3.10,需要升级
```bash
$ sudo vim /etc/apt/sources.list
 
## tail following content:
deb https://launchpad.proxy.ustclug.org/ubuntu-toolchain-r/test/ubuntu bionic main
deb-src https://launchpad.proxy.ustclug.org/ubuntu-toolchain-r/test/ubuntu bionic main
 
### install Signing Key
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 60C317803A41BA51845E371A1E9377A2BA9EF27F
 
### update gcc-9
$ sudo apt-get update && sudo apt-get upgrade
$ sudo apt install gcc-9 g++-9
 
#### set gcc-9 as default gcc
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 30 --slave /usr/bin/g++ g++ /usr/bin/g++-9
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 40 --slave /usr/bin/g++ g++ /usr/bin/g++-7
$ sudo update-alternatives --config gcc
## input 2 to select gcc-9


### update cmake

$sudo apt install cmake

$ wget https://github.com/Kitware/CMake/releases/download/v3.20.5/cmake-3.20.5.tar.gz
$ tar -zxvf cmake-3.20.5.tar.gz && cd cmake-3.20.5
$ mkdir build && cd build
$ cmake ..   #报错,需要安装openssl ，sudo apt-get install libssl-dev
             #或者cmake -DCMAKE_USE_OPENSSL=OFF <path_to_source>，CMake在构建过程中不使用 OpenSSL
$ make 
$ sudo make install
$ cmake --version


```

```bash
#所需要的测试框架的源码
#bdd:
https://github.com/michaelvlach/cppbdd
$ git clone https://github.com/michaelvlach/cppbdd.git

#googletest:
https://github.com/google/googletest
$ git clone https://github.com/google/googletest.git

#cmock:
https://github.com/hjagodzinski/C-Mock
$ git clone https://github.com/hjagodzinski/C-Mock.git

本例子的环境直接选择了固定版本
googletest: https://github.com/google/googletest/tree/v1.14.x

bdd: https://github.com/michaelvlach/cppbdd  main 1.0.0

c-mok: https://github.com/hjagodzinski/C-Mock/tree/v0.4.0
```