---
description: >-
  source: https://blog.csdn.net/hblhly/java/article/details/80740493
  https://blog.csdn.net/FX677588/java/article/details/53159019
---

# gcc, g++, make, cmake

文本程序到可执行文件生成无论在什么平台大致分为以下几个部分：

1. 用编辑器编写源代码，如.c文件。&#x20;
2. 用编译器编译代码生成目标文件，如.o。&#x20;
3. 用链接器连接目标代码生成可执行文件，如.exe。&#x20;

Linux平台下，.o文件一般是通过编译的但还未链接的目标文件，.out文件一般都是经过相应的链接产生的可执行文件（linux下）。当然这是一般情况下人们这么设置，而真正的，在linux中 .o通常保存的是可执行代码 ，至于可执行文件则没有规定扩展名，用的是文件属性位来决定的是否可执行。在chmod中设置。 我们知道编译和链接阶段是靠g++和gcc编辑器来完成，这两个编译阶段是相同的，但是链接阶段g++默认链接c++库，所以一般情况下用gcc编译c文件，而g++编译cpp文件。当然g++也可以编译c文件，而gcc编译cpp文件则需要在后面加上参数-lstdc++，作用就是链接c++库。

首先介绍一下GCC：GNU Compiler Collection(GNU 编译器集合)，在为Linux开发应用程序时，绝大多数情况下使用的都是C语言，因此几乎每一位Linux程序员面临的首要问题都是如何灵活运用C编译器。目前 Linux下最常用的C语言编译器是GCC（GNU Compiler Collection），它是GNU项目中符合ANSI C标准的编译系统，能够编译用C、C++和Object C等语言编写的程序。GCC不仅功能非常强大，结构也异常灵活。最值得称道的一点就是它可以通过不同的前端模块来支持各种语言，如Java、 Fortran、Pascal、Modula-3和Ada等。

## gcc与g++区别：

gcc是GCC中的GNU C Compiler（C 编译器）\
g++是GCC中的GNU C++ Compiler（C++编译器）\
更准确的说法是：gcc调用了C compiler，而g++调用了C++ compiler

gcc和g++的主要区别

1. 对于 _.c和_.cpp文件，gcc分别当做c和cpp文件编译（c和cpp的语法强度是不一样的）; g++则统一当做cpp文件编译;
2. 使用g++编译文件时，g++会自动链接标准库STL，而gcc不会自动链接STL，在用gcc编译c++文件时，为了能够使用STL，需要加参数 –lstdc++ ，但这并不代表 gcc –lstdc++ 和 g++等价;
3. 3.gcc在编译C文件时，可使用的预定义宏是比较少的;
4. 4.gcc在编译cpp文件时/g++在编译c文件和cpp文件时（这时候gcc和g++调用的都是cpp文件的编译器），会加入一些额外的宏，这些宏如下： `defineGXX_WEAK 1 define __cplusplus 1 define __DEPRECATED 1 define GNUG 4 define __EXCEPTIONS 1 define private_extern extern`

## g++ 的简单使用

```
g++ -o name_of_executable src_file1 src_file2 src_file3

e.g.
g++ -o test_sort.exe test_sort.cpp
```

## gcc/g++与make区别：

当你的程序只有一个源文件时，直接就可以用gcc命令编译它。但是当你的程序包含很多个源文件时，用gcc命令逐个去编译时，你就很容易混乱而且工作量大.

所以出现了make工具！make工具可以看成是一个智能的批处理工具，它本身并没有编译和链接的功能，而是用类似于批处理的方式—通过调用makefile文件中用户指定的命令来进行编译和链接的。

makefile是什么？简单的说就像一首歌的乐谱，make工具就像指挥家，指挥家根据乐谱指挥整个乐团怎么样演奏，make工具就根据makefile中的命令进行编译和链接的。makefile命令中就包含了调用gcc（也可以是别的编译器）去编译某个源文件的命令。

## make与cmake区别：

makefile在一些简单的工程完全可以人工手下，但是当工程非常大的时候，手写makefile也是非常麻烦的，如果换了个平台makefile又要重新修改。

这时候就出现了Cmake这个工具，cmake就可以更加简单的生成makefile文件给上面那个make用。当然cmake还有其他功能，就是可以跨平台生成对应平台能用的makefile，你不用再自己去修改了。

可是cmake根据什么生成makefile呢？它又要根据一个叫CMakeLists.txt文件（学名：组态档）去生成makefile。

## Using cmake for projects using opencv in Windows

[Issues related to x86 and x64](https://answers.opencv.org/question/200302/error-using-cmake-on-windows-with-visual-studio-2017-and-opencv-343/)

## Some blogs to read about creating cmake files

[https://cliutils.gitlab.io/modern-cmake/chapters/basics.html](https://cliutils.gitlab.io/modern-cmake/chapters/basics.html)

[https://blog.csdn.net/gg\_18826075157/article/details/72780010](https://blog.csdn.net/gg\_18826075157/article/details/72780010)

​[https://preshing.com/20170511/how-to-build-a-cmake-based-project/](https://preshing.com/20170511/how-to-build-a-cmake-based-project/)

[https://blog.csdn.net/whahu1989/article/details/82078563](https://blog.csdn.net/whahu1989/article/details/82078563)

[find\_package](https://zhuanlan.zhihu.com/p/97369704) command
