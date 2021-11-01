---
title: "C/C++Windows-Mingw-Vscode-CMake"
date: 2021-11-1T20:16:47+08:00
draft: false
---
# C/C++ 环境配置 Windows-Mingw-Vscode
## 环境要求：
- 64位 Windows 10 或 Windows 11 电脑
- Vscode 
- Mingw
- 网络连接

## 目的
**在vscode上配置C/C++的编码、编译和调试环境，分别使用简单的GCC/G++方法和较为复杂但工程常用的CMake方法**  
&emsp;&emsp;学习和使用C/C++的时候需要进行程序的编写运行和调试，常规IDE的体积较大，下载不便。  
&emsp;&emsp;虽然IDE使用起来比较方便，但对于初学者来说，IDE中的很多功能都不需要，对于老手来说，很多功能可以自己配置，所以对于环境来说尽量要求轻便。  
&emsp;&emsp;我们可以选择使用更为轻便的vscode进行编码、编译与调试，通过一些配置可以方便的实现IDE的大部分功能。

## 步骤
1. 下载mingw压缩包，解压并设置环境变量
2. 下载cmake压缩包，解压并设置环境变量
3. 下载vscode 安装
4. 在vscode上安装C/C++、cmake、cmake tools、中文插件
5. 创建文件夹并编写代码
6. 设置使用gcc或g++编译、调试
7. 另创建文件夹并使用CMake:Quick start 快速创建测试工程
8. 设置使用cmake作为构建调试工具
9. 总结两种方法的优劣

## 一、下载并设置mingw
### (1) 下载并解压
&emsp;&emsp;在[mingw-w64](https://sourceforge.net/projects/mingw-w64/files/)的下载页面找到**MinGW-W64 GCC-8.1.0**下的**x86_64-posix-seh**进行下载
&emsp;&emsp;下载后解压在一个可以长期保留的位置并记录下该文件夹下的bin文件路径  
**示例：**
```powershell
D:\app\x86_64-8.1.0-release-posix-seh-rt_v6-rev0\mingw64\bin
```
### (2) 设置环境变量
设置环境变量以便可以在系统全局找到mingw  
1. 使用<kbd>Windows</kbd> + <kbd>R</kbd>键 打开 Windows运行窗口，输入 **sysdm.cpl** 后选择 <kbd>高级</kbd> 选项卡中的 <kbd>环境变量</kbd>  
2. 在 <kbd>系统变量</kbd> 中找到 <kbd>Path</kbd> 变量 点击 <kbd>编辑</kbd> 按钮
3. 在变量的最后一行后双击并添加刚才的mingw路径 ,添加完成后点击 <kbd>确定</kbd>
4. 随后分别点击前几个页面的确定
5.  使用<kbd>Windows</kbd> + <kbd>R</kbd>键 打开 Windows运行窗口，输入 **cmd**， 后输入 gcc -v ,检查输出是否为刚才下载的mingw版本号  
**示例：**
```
C:\Users\WGB>gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=D:/app/x86_64-8.1.0-release-posix-seh-rt_v6-rev0/mingw64/bin/../libexec/gcc/x86_64-w64-mingw32/8.1.0/lto-wrapper.exe
Target: x86_64-w64-mingw32
Configured with: ../../../src/gcc-8.1.0/configure --host=x86_64-w64-mingw32 --build=x86_64-w64-mingw32 --target=x86_64-w64-mingw32 --prefix=/mingw64 --with-sysroot=/c/mingw810/x86_64-810-posix-seh-rt_v6-rev0/mingw64 --enable-shared --enable-static --disable-multilib --enable-languages=c,c++,fortran,lto --enable-libstdcxx-time=yes --enable-threads=posix --enable-libgomp --enable-libatomic --enable-lto --enable-graphite --enable-checking=release --enable-fully-dynamic-string --enable-version-specific-runtime-libs --disable-libstdcxx-pch --disable-libstdcxx-debug --enable-bootstrap --disable-rpath --disable-win32-registry --disable-nls --disable-werror --disable-symvers --with-gnu-as --with-gnu-ld --with-arch=nocona --with-tune=core2 --with-libiconv --with-system-zlib --with-gmp=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-mpfr=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-mpc=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-isl=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-pkgversion='x86_64-posix-seh-rev0, Built by MinGW-W64 project' --with-bugurl=https://sourceforge.net/projects/mingw-w64 CFLAGS='-O2 -pipe -fno-ident -I/c/mingw810/x86_64-810-posix-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' CXXFLAGS='-O2 -pipe -fno-ident -I/c/mingw810/x86_64-810-posix-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' CPPFLAGS=' -I/c/mingw810/x86_64-810-posix-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' LDFLAGS='-pipe -fno-ident -L/c/mingw810/x86_64-810-posix-seh-rt_v6-rev0/mingw64/opt/lib -L/c/mingw810/prerequisites/x86_64-zlib-static/lib -L/c/mingw810/prerequisites/x86_64-w64-mingw32-static/lib '
Thread model: posix
gcc version 8.1.0 (x86_64-posix-seh-rev0, Built by MinGW-W64 project)
```  
## 二、下载并设置cmake
### (1) 下载并解压
&emsp;&emsp;在[CMake官网](https://cmake.org/download/)中找到Windows x64 ZIP 并下载 
&emsp;&emsp;下载后解压在一个可以长期保留的位置并记录下该文件夹下的bin文件路径  
**示例：**
```powershell
D:\app\cmake-3.22.0-rc2-windows-x86_64\bin
```
### (2) 设置环境变量
设置环境变量以便可以在系统全局找到CMake  
1. 使用<kbd>Windows</kbd> + <kbd>R</kbd>键 打开 Windows运行窗口，输入 **sysdm.cpl** 后选择 <kbd>高级</kbd> 选项卡中的 <kbd>环境变量</kbd>  
2. 在 <kbd>系统变量</kbd> 中找到 <kbd>Path</kbd> 变量 点击 <kbd>编辑</kbd> 按钮
3. 在变量的最后一行后双击并添加刚才的CMake路径 ,添加完成后点击 <kbd>确定</kbd>
4. 随后分别点击前几个页面的确定
5.  使用<kbd>Windows</kbd> + <kbd>R</kbd>键 打开 Windows运行窗口，输入 **cmd**， 后输入 cmake --version ,检查输出是否为刚才下载的CMake版本号  

**示例：**
```
C:\Users\WGB>cmake --version
cmake version 3.22.0-rc2

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```
## 三、下载并解压vscode
### (1) 下载
&emsp;&emsp;在[vscode 官网](https://code.visualstudio.com/Download)进行下载，可以选择64bit zip包下载，可以免去安装过程
### (2) 解压
&emsp;&emsp;解压到可以长期保留的位置后对文件夹内的vscode.exe 创建快捷方式并拖到自己想要的地方，方便以后快捷打开

## 四、安装vscode插件
### (1) 打开vscode的插件页面
&emsp;&emsp;可以使用快捷键 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</Kbd> 打开插件页面搜索并安装如下几个插件
- C/C++                     C、C++支持
- Chinese                   中文支持
- CMake                     CMake支持
- CMake Tools               CMake工具套装
&emsp;&emsp;成功安装后再次打开时，整个vscode变为中文  

## 五、使用GCC、G++配置C/C++环境
使用GCC、G++配置环境比较适合较小的工程，一般作为验证程序的编写调试等等  

### (1) 新建文件夹和代码
1. 在电脑中新建一个文件夹并通过vscode的打开文件夹功能打开
2. 在文件夹中创建一个 hello.c 文件，并填入一些简单的代码
```
#include<stdio.h>
int main()
{
    printf("hello,world\n");
}
```
3. 创建后整个目录结构如下
```
.
└── hello.c
```

### (2) 配置mingw的路径
1. 通过<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</Kbd>调出选项栏后，输入C/C++:Edit Configurations (UI) 并选择 <kbd>C/C++:Edit Configurations (UI)</kbd>
2. 在编译器路径下的输入框中填入mingw64的gcc.exe的路径  
示例:
```
D:\app\x86_64-8.1.0-release-posix-seh-rt_v6-rev0\mingw64\bin\gcc.exe
```
3. 选择 IntelliSense 模式 为 windows-gcc-x64  


### (3) 配置构建任务
通过配置构建任务可以方便的进行构建生成可执行文件，对于构建选项可以在生成的构建任务文件(.vscode/tasks.json)中修改
1. 随后将页面选为编辑 .c 文件的页面点击 工具栏上的<kbd>终端</kbd>(快捷键 <kbd>Alt</kbd> + <kbd>T</kbd> )
2. 在弹出的菜单中选择  <kbd>配置默认生成任务</kbd> ，然后在出现的选项栏中选择刚才填入路径的 <kbd>gcc.exe 生成活动文件</kbd>
3. 点击工具栏上的 <kbd>终端</kbd>(快捷键 <kbd>Alt</kbd> + <kbd>T</kbd> )，再点击<kbd>运行生成任务</kbd>来构建可执行文件 ，或使用快捷键<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>B</kbd> 直接构建可执行文件  
   随后就可以看到在当前目录已经成功生成了执行文件hello.exe  
4. 可以在cmd命令行或者终端中通过命令 ./hello.exe 运行，并打印 Hello, world!


### (4) 配置调试
通过配置调试可以方便的打断点调试等  
1. 将页面选为编辑 .c 文件的页面并点击 工具栏上的<kbd>运行</kbd>(快捷键 <kbd>Alt</kbd> + <kbd>R</kbd> )
2. 在弹出的菜单中选择<kbd>添加配置</kbd>，随后在出现的选项栏中选择<kbd>C++(GDB/LLDB)</kbd>
3. 随后页面会切换到调试模式，并显示调试的配置文件，默认不需要更改，切换回.c文件并在Print语句前打上红色断点
4. 点击工具栏上的<kbd>运行</kbd>(快捷键 <kbd>Alt</kbd> + <kbd>R</kbd> )
5. 在随后的菜单中点击<kbd>启动调试</kbd>，或直接点击 <kbd>F5</kbd>按键进行调试
6. 随后页面跳转为调试模式并在断点处停止


## 六、使用CMake编译C/C++
在一般的工程中会存在许多的源文件，通过CMake可以方便的构建较大工程，同时具有一定的跨平台能力，更为高级的操作可以细致查看CMake插件文档和CMake官方文档

### (1) 使用CMake构建
1. 重新新建一个文件夹，并使用vscode进行打开操作
2. 通过<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</Kbd>调出选项栏后，输入CMake:Quick start 并选择 <kbd>CMake:Quick start</kbd>
3. 随后输入新建的工程名称，这里选择填写 hello 作为工程名称
4. 随后选择生成的文件类型，选择 <kbd>Executable</kbd>
5. 这时在目录中自动创建CMakeLists.txt 和 main.c 文件
6. 可以使用<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</Kbd>调出选项栏后，输入CMake:Build 并选择 <kbd>CMake:Build</kbd>生成可执行文件，也可以通过快捷键<kbd>F7</kbd> 生成，生成的文件在./build/ 下 ，生成名称为第3步输入的工程名
7. 可以在cmd命令行或者终端中通过命令 ./hello.exe 运行，并打印 Hello, world!


### (2) 使用CMake调试
1. 可以使用<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</Kbd>调出选项栏后，输入CMake:Debug 并选择 <kbd>CMake:Debug</kbd>进入调试界面，也可以通过快捷键<kbd>Ctrl</kbd> + <kbd>F5</kbd> 调试，同样可以先打断点进行调试

## 五、总结
&emsp;&emsp;两种方法都可以对C/C++程序进行构建调试，其中GCC的方法适用于小型工程，对于稍大的工程使用CMake更为方便，但是CMake的功能对于初学者来说可能会造成困扰，不能很好的添加源文件，但是既然开始学习就应该选择更为广泛使用的工具，所以推荐大家使用CMake，