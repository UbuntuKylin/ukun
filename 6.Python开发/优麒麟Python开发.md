# 优麒麟Python开发

现在Python已经应用到了越来越多的领域当中，例如人工智能与大数据，图像分析等。现如今最为流行的两大深度学习框架Pytorch与Tensorflow均由Python编写。因此本文介绍如何在优麒麟环境下进行Python环境的搭建与开发.

## 查看是否安装Python

通过在终端输入命令``python -V``或者``python --version``。可以查看安装在本机的Python版本。在笔者的电脑上，命令行的输出为：

```bash
Python 2.7.18
```

那么说明计算机安装的Python版本为2.7.18。

![pythonVersion](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/pythonVersion-16327482632661.png)

## 多版本Python安装

因为一些特殊原因比如说一些老项目中还用的是老版本的Python，如果贸然迁移可能会导致项目中部分老的API废弃无法正常运行，因此可能我们的计算机中需要同时安装两个版本的Python、

通过命令``ls -l /usr/bin/python``我们可以查看在``/usr/bin/``目录下面所有与python相关的文件，如下图：

![listPython](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/listPython-16327482651112.png)

![pythonInstalled](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/pythonInstalled-16327482668333.png)

可以看到这台电脑同时安装了python2.7与python 3.8。而``/usr/bin/python``后面指向**python2**的箭头说明，当我们在调用python命令的时候，实际上是在调用**python2**。但是如果我们想要默认的python命令调用python3.8，该如何操作呢？

## 更新软连接

首先明确一个问题，什么是软连接？

> In computing, a symbolic link (also symlink or soft link) is a term for any file that contains a reference to another file or directory in the form of an absolute or relative path and that affects pathname resolution

所谓**软连接**，就是一个指向其他文件或者路径（绝对相对均可）的引用。听起来是不是有点像编程中的指针？其实也可以用Windows中的快捷方式与macOS中的替身来类比。而与软连接相对应的则是硬连接(hard link)。软硬连接之间的区别具体可以参照[这里](https://en.wikipedia.org/wiki/Symbolic_link)。而上面的``/usr/bin/python``本质上就是指向``/usr/bin/python2.7``的一个软连接。当我们调用python，实际上也是在调用``/usr/bin/python2.7``。因此如果我们想让``/usr/bin/python``指向其他版本的python，只需要更新软连接即可。

更新软连接的方法如下:

```bash
ln -s target_path link_path
```

``target_path``是要指向的文件/路径的路径，``link_path``是创建的软连接的路径。

因此在上面如果我们想要改变``/usr/bin/python``的指向，例如指向python3.8，可以使用下面的代码：

```
ln -s /usr/bin/python /usr/bin/python3.8
```

当自己无法判断电脑中的python默认是2.x还是3.x的时候可以通过显式指定python2与python3的软连接来达到避免混淆的目的

![softLink](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/softLink-16327482707364.png)

## 包管理工具

python有着众多的优秀的第三方包，如``nparray``，``pandas``等。不少项目也会需要用到他们。python自带的pip工具可以帮助我们进行包的管理，如安装移除升级等等。但是这还不够，如果所有的项目都共享一个包环境的话难免会出现问题。因此引入全平台通用的包环境管理工具[Anaconda](https://www.anaconda.com/products/individual-d)

下载linux平台对应的installer之后，在终端执行安装脚本。

![anacondaInstall](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/anacondaInstall-16327482723695.png)

一路回车之后即完成了Anaconda的安装。

![anacondaCLI](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/anacondaCLI-16327482748366.png)

安装完成之后需要在终端的配置文件中加入anaconda的路径，在~/.zshrc中输入下面代码

```bash
export PATH=path_to_anaconda3:$PATH
```

在笔者的电脑上，这句指令就是

```bash
export PATH=/home/godlowd/anaconda3/bin:$PATH
```

通过在终端中输入

```
conda create --name test python=3.7 # 创建一个名为test，python版本为Python 3.7版本的环境
```

即可创建一个虚拟环境。在刚刚安装完成的环境下，会默认处于名为**base**的环境下，通过观察终端前面的小括号中的内容可以识别当前所处的虚拟环境的名字。

通过``conda activate test``可以激活刚刚创建的虚拟环境。同样，通过``conda deactivate test``可以退出当前的虚拟环境

![condaCreate](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/condaCreate-16327482768397.png)

![activateEnv](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/activateEnv-16327482782468.png)

### 更换清华conda源

因为服务器架设在国外的原因，我们在安装python相关包的时候容易引起时间过长而连接中断的问题。通过将conda的下载源设置成国内可以很好的解决这个问题

首先通过``conda config --show``打印当前conda环境的一些信息，其中就包括了下载的channels。可以看到此时的channel还不是清华的下载源。

在终端执行

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
# 以上两条是Anaconda官方库的镜像

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
# 以上是Anaconda第三方库 Conda Forge的镜像
```

![changeSource](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/changeSource-16327482800899.png)

将conda的下载源替换成清华。替换完成后可以使用``conda config --show``查看是否替换成功。

![showConfig](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/showConfig-163274828137510.png)

## Hello World

Python开发最为著名的ide就是JetBrain公司的**Pycharm**，通过访问他们的[官网](https://www.jetbrains.com/pycharm/download/#section=linux)，我们可以获取到Pycharm的Community版本。下载安装完成之后，根据提示一步步往下走，创建一个新的项目。

新的项目创建好之后如下图所示，其中空空如也。点击文件夹，右键，选择new，选择python file。给新创建的文件命名为main.py。

![createMainFile](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/createMainFile-163274828300611.png)

然后在新文件中复制一下的代码

```python
print('Hello, World');
```

这个时候还不能运行我们的项目，因为还没有配置项目的运行环境。点击上面的绿色小三角形，在配置文件中添加一个我们刚刚配置好的python环境即可。

![pycharmConfig](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/pycharmConfig-163274828453012.png)

配置完成后点击绿色小三角形即可正常运行。运行结果如下图所示：

![helloworld](%E4%BC%98%E9%BA%92%E9%BA%9FPython%E5%BC%80%E5%8F%91.assets/helloworld-163274828618313.png)

