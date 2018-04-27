# CentOS 7 安装 gcc 4.1.2

## 0. 参考

[在centOS7.2上编译gcc4.1.2](http://www.cnblogs.com/tianjiqx/p/6224126.html)



## 1. 安装了编译所需

```shell
yum groupinstall "Development Tools"
yum install wget bzip2 texinfo glibc-devel glibc-static glibc binutils vim
```

出现collect2: error: ld returned 1 exit status多为编译库没有安装，要安装上glibc-devel。



## 2.下载源码 

```shell
wget http://mirrors.ustc.edu.cn/gnu/gcc/gcc-4.1.2/gcc-4.1.2.tar.bz2
tar jxvf gcc-4.1.2.tar.bz2
```



## 3. 修改代码

```c
struct rt_sigframe {
    int sig;
    struct siginfo *pinfo;
    void *puc;
    struct siginfo info;
} *rt_ = context->cfa;
```

改为

```c
struct rt_sigframe {
    int sig;
    siginfo_t *pinfo;
    void *puc;
    siginfo_t info;
    struct ucontext uc;
} *rt_ = context->cfa;
```

否则会有类似以下的编辑错误

```shell
It has a compiling error when you build gcc4.1.2:
../gcc/config/i386/linux-unwind.h:138:17: error: field 'info' has incomplete type " 
```



## 4. 编译

```shell
cd gcc-4.1.2
./configure --prefix=/usr/local/gcc --enable-languages=c,c++
make
```



## 5. 替换现有的gcc

```shell
cd /usr/bin/
mv gcc gcc-4.8.5 #4.8.5为现版本
mv g++ g++-4.8.5 #4.8.5为现版本
ln -s /usr/local/gcc-4.1.2/bin/gcc /usr/bin/gcc
ln -s /usr/local/gcc-4.1.2/bin/g++ /usr/bin/g++
```

检查是gcc g++版本已经替换

```shell
gcc -v

Using built-in specs.
Target: x86_64-unknown-linux-gnu
Configured with: ./configure --prefix=/usr/local/gcc-4.1.2 --enable-languages=c,c++
Thread model: posix
gcc version 4.1.2

```

