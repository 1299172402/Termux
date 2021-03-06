# Termux

本文主要介绍如何简单使用Termux  

## 简介
 
Termux是一款在Android上模拟运行Linux的软件（个人理解），可以让你的手机作为一个服务器或者其他的什么东西。  
他的Github页面在 [此处](https://github.com/termux) 你可以详细的访问它。

## 下载方式

- （推荐）F-Droid [下载地址](https://f-droid.org/zh_Hans/packages/com.termux/)  目前（2021.03.06）版本最新为0.108，更新日期：2021-02-24，大小85MiB  
  此网页并没有被限制访问，下载速度尚可。  
  如遇下载缓慢，可尝试使用迅雷下载（这也是迅雷现在唯一的优点了，下载国外的安装包。。。）  
  或者本人之前在官网下载的安装包 [蓝奏云](https://zhiyuyu.lanzous.com/ie0lcml9d2h) 2021.02.24日下载，版本 0.107  
  
- 谷歌应用商店  
  不过现在官方不建议在上面下载，未来也可能停止在谷歌市场的更新。  
  另：谷歌提供的是apks的安装方式，应用可能不能正常导出为apk，敬请留意。（解决办法是使用软件 SAI）  
  
- 酷安  
  并不是指到酷安下载他提供的软件包，而是从这款软件的评论区获取此软件。  
  
## 开始使用

### 更换默认源

  因为服务器在国外的原因，termux的软件下载和更新超级慢。所以推荐使用[清华大学的termux镜像源](https://mirrors.tuna.tsinghua.edu.cn/help/termux/)。在命令行中输入如下内容  
```termux
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list
sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list
sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list
apt update && apt upgrade
```
其他更换镜像源的方法也可以在其网站中找到  

### 允许访问内部储存空间

使用`termux-setup-storage`命令以获取权限，这样子你就可以从termux访问手机上的文件了。

### 使用软连接

如下命令即可在当前目录中创建一个名为coding，指向/sdcard/coding的软连接（类似于快捷方式）
```
ln -s /sdcard/coding coding
```  

### 安装Vim

有点遗憾的是termux并没有内置提供vim编辑器，所以需要通过如下指令安装。  
```
pkg install vim
```
此步骤亦可以帮助你了解termux的软件包安装指令  

### 安装Python

```
pkg install python
```
请注意，python安装完成后也同样有默认软件源访问过慢的问题，请参照如下命令更换软件源。  
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
更多信息可以访问 [清华大学pipy镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)

附：现在的python似乎并不能运行多进程的程序，不过多线程可以。（2020.11，华为手机）

### 安装Clang

Clang是C语言的一个编译器  
注：另一个常见的编译器是GCC，termux同样支持，但笔者更偏爱clang的一些特性，故于此记录  

```
pkg install clang
```

因为笔者习惯于直接编译并运行，故附上下列方法以简便操作  

1. 将此shell脚本保存至 `~/c.sh`
```bash
#!/bin/bash

file=$(basename $1)
fn="${file%.*}"
fnout="$fn.out"

clang $1 -o ~/$fnout
echo -e "\e[35m----------Compile Finished----------\e[0m"
~/$fnout
echo -e "\e[35m--------------Run Finished----------\e[0m"
rm ~/$fnout

```
2. 使用alias简化命令

将如下内容保存至`~/.bashrc`文件中
```
alias c="~/c.sh"
```

3. 重启termux

4. 如下使用即可
```
c /sdcard/coding/hello.c
```

效果如下
```linux
~ $
~ $ c /sdcard/coding/hello.c
----------Compile Finished----------
Hello, world!
C program run succeed
--------------Run Finished----------
~ $
```
