# 优麒麟Qt开发

## 准备工作

在正式安装 Qt 之前，需要先做一些准备工作，这些都将是软件开发的前提，像 GNU gcc 编译器、make、以及其他开发包的安装。除此之外，要构建图形化 Qt 应用程序，还需要安装 OpenGL 库和头文件。

在 Ubuntu 和其他基于 Debian 的 Linux 系统上，可以通过安装 libgl1-mesa-dev 和 build-essential 包来获得 OpenGL 和最小的开发工具集，即运行以下命令：

```bash
sudo apt-get install build-essential libgl1-mesa-dev
```

通常情况下，我们还需要安装 gdb 调试器，甚至是一些其他的可选工具（如 git、clang 等）。当然了，如果需要的话，可以在安装完 Qt 之后的任何时候再安装它们。

![essential](%E4%BC%98%E9%BA%92%E9%BA%9FQt%E5%BC%80%E5%8F%91.assets/essential-16327423915141.png)

## 安装Qt相关开发工具

### 下载资源包手动安装

通过访问[Qt的官网](https://download.qt.io/archive/qt/)，可以下载最新的配套工具，当前最新的版本为5.14，在版本为5.14的页面内可以看到Qt为不同平台提供的安装包，我们只需要下载对应平台下的安装包即可。

下载完成之后，需要先为安装程序赋予一定的可执行权限

```bash
chmod +x qt-opensource-linux-x64-5.14.2.run
```

之后运行安装程序即可

```bash
sudo ./qt-opensource-linux-x64-5.14.2.run 
```

在安装过程中的组件一般选择默认即可。

安装完成之后可以通过``qmake -v``查询当前安装的Qt的版本，维基百科上关于``qmake``的解释是这样子的：

> qmake是一个协助简化跨平台进行项目开发的构建过程的工具程序，Qt附带的工具之一。qmake能够自动生成Makefile、Microsoft Visual Studio 项目文件 和 xcode 项目文件。不管源代码是否是用Qt写的，都能使用qmake，因此qmake能用于很多软件的构建过程。
> 手写Makefile是比较困难而且容易出错，尤其在进行跨平台开发时必须针对不同平台分别撰写Makefile，会增加跨平台开发复杂性与困难度。qmake会根据项目文件（.pro）里面的信息自动生成适合平台的 Makefile。开发者能够自行撰写项目文件或是由qmake本身产生。qmake包含额外的功能来方便 Qt 开发，如自动的包含moc 和 uic 的编译规则。

## 设置环境变量

想让 Qt 更好地为我们服务，就需要扩展一些环境变量。像 qmake、moc 以及其他的一些 Qt 工具所在的路径，都需要加到 PATH 里面。

这里我们通过修改``/etc/profile``文件内容添加环境变量。

```bash
sudo vim /etc/profile
```

然后跳转到文件的末尾，添加上下面两行

```bash
export PATH="/opt/Qt5.14.2/Tools/QtCreator/bin:$PATH"
export PATH="/opt/Qt5.14.2/5.14.2/gcc_64/bin:$PATH"
```

两个路径主要根据自己的实际情况灵活调整。修改完成之后，通过``source /etc/profile``命令生效。如果修改的是``~/.zshrc``那么也是一样的道理。

## Hello World

下面通过一个最简单的方式使用Qt进行开发

首先新建一个文件夹名为`hello`。然后在里面创建一个名字为`hello.cpp`的文件。将下面的代码复制进去

```CPP
#include <QApplication>  
#include <QLabel>  
 
int main(int argc,char *argv[])  
{
    QApplication app(argc,argv);
    QLabel *label=new QLabel("Hello QT!");
    label->show();
    return app.exec();      
}
```

需要注意的是，在Qt4以后，一些控件的在原来的SDK中的位置发生了改变，因此如果直接使用会报找不到对应控件的问题。这个问题我们会在之后的过程中处理。

在终端输入如下命令 ，生成工程文件。此时在hello文件夹下生成hello.pro文件。

```
qmake -project
```

然后输入如下命令修改工程文件

```
sudo gedit hello.pro
```

在打开的文件末尾加上如下命令

```
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets
```

这样就能保证可以在执行的时候正确引入我们所要的文件。然后在终端输入如下命令，生成Makefile

```
qmake hello.pro
```

在终端执行`make`操作，生成可执行文件`hello`，通过如下命令运行生成的可执行文件。

![progress](%E4%BC%98%E9%BA%92%E9%BA%9FQt%E5%BC%80%E5%8F%91.assets/progress-16327423961512.png)

运行结果如下图所示：

```
./hello
```

![demo](%E4%BC%98%E9%BA%92%E9%BA%9FQt%E5%BC%80%E5%8F%91.assets/demo.png)

### Qtcreator

除了使用上面提到的方法，还可以使用Qt提供的官方IDE进行开发，那就是Qtcreator。

![qtcreator](%E4%BC%98%E9%BA%92%E9%BA%9FQt%E5%BC%80%E5%8F%91.assets/qtcreator-16327424004534.png)

在终端进入Qtcreator所在的目录。然后调用可执行文件。

```bash
cd /opt/Qt5.12.11/Tools/QtCreator/bin
./qtcreator
```

下图就是打开qtcreator之后的默认界面。

![qtDemoProjCode](%E4%BC%98%E9%BA%92%E9%BA%9FQt%E5%BC%80%E5%8F%91.assets/qtDemoProjCode-16327424023805.png)

选择第二个项目选项卡，然后在左边栏空白地方内右键，选择新建项目，根据流程一步步走，创建一个最简的默认项目。

![creatorCreateProj](%E4%BC%98%E9%BA%92%E9%BA%9FQt%E5%BC%80%E5%8F%91.assets/creatorCreateProj-16327424041726.png)

![choosePath](%E4%BC%98%E9%BA%92%E9%BA%9FQt%E5%BC%80%E5%8F%91.assets/choosePath-16327424058427.png)

创建项目完成之后，点击左下角的绿色三角形，就能运行创建的项目中的代码，将创建的窗口显示在屏幕之上。

![emptyWindow](%E4%BC%98%E9%BA%92%E9%BA%9FQt%E5%BC%80%E5%8F%91.assets/emptyWindow-16327424080248.png)
