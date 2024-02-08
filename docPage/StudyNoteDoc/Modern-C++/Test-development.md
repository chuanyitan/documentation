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

$ 如果本地没有cmake或版本低，可以用以下方式操作；
$ cd cmake-3.28.2
$ ./bootstrap
$ make -j12
$ sudo make install

$ cmake --version

#最好直接使用最新cmake版本 	cmake-3.28.2.tar.gz
$ wget https://github.com/Kitware/CMake/releases/download/v3.28.2/cmake-3.28.2.tar.gz

### update python3.8
$ sudo apt install -y build-essential python3-pip python3.8  doxygen
# 下面内容可以暂时不用安装  
$ python3.8 -m pip install --upgrade pip
$ python3.8 -m pip install cmake Jinja2~=3.1.2 antlr4-python3-runtime~=4.13.0
$ sudo apt install -y default-jre-headless graphviz git libncurses-dev libncursesw5-dev

# ubuntu里安装了python2和python3
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 100
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 150
# 选择默认版本
$ sudo update-alternatives --config python

# 多个版本python3, 来选择默认的版本
$ sudo update-alternatives --install /usr/bin/python python3 /usr/bin/python3.6 150
$ sudo update-alternatives --install /usr/bin/python python3 /usr/bin/python3.8 100
$ sudo update-alternatives --config python3

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

gtest sample 参考 https://blog.csdn.net/wei_y0117/article/details/127922331

   https://blog.csdn.net/xb_2015/article/details/124361154
