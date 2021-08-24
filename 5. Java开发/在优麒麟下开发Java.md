# 在优麒麟下开发Java

## 安装JDK

### 命令行安装

JDK的安装方式主要有两种，第一种是通过命令行安装，安装命令为``sudo apt-get install openjdk-8-jdk``。根据安装版本的不同可以修改openjdk后面的数字。

### 官网安装

第二种是通过官网安装，前往JDK的[官方网址](http://www.oracle.com/technetwork/java/javase/downloads/index.html),下载对应的tar.xz文件。

![image-20210822223116384](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/image-20210822223116384-16296426783531.png)

如上图，选择对应的架构文件安装即可。这里我选择的是**Linux x64 Compressed Archive**。下载完成后将压缩文件移动到``/usr/local/`` 目录下

```
sudo mv ~/jdk-16.0.2_linux-x64_bin.tar.xz /usr/local/
```

``~/jdk-16.0.2_linux-x64_bin.tar.xz``是压缩文件的目录位置。移动完成后再对应名录解压压缩文件

```
sudo tar -zxvf jdk-16.0.2_linux-x64_bin.tar.xz
```

## 命令行配置JDK

经过上述的操作之后我们已经成功将JDK安装到了我们的电脑之中，但是我们要怎么才能使用他们呢。如果现在就去命令行输入``java``则会提示我们类似``No such file or directory``的错误。

因此我们需要修改我们的配置文件来让刚刚安装的JDK生效。根据当前终端类型的不同，需要修改的配置文件也不尽相同。如果安装了``zsh``，要修改的配置文件就是``~/.zshrc``。如果是没有修改过的默认终端，则修改``/etc/profile``。

打开配置文件，笔者这里是，在配置文件的末尾加入下面的代码

```bash
JAVA_HOME=/usr/local/jdk-16.0.2
export PATH=$JAVA_HOME/bin:$PATH
```

然后通过``source ~/.zshrc``命令来更新配置文件。刚刚修改过后的配置文件并不会立即生效，因此需要``source``一下。

### 检测是否安装成功

在终端输入

```bash
java -version
javac -version
```

如果有正确的输出则说明Java环境安装完成。

## 安装IDE

开发Java常用的两大IDE是**Eclipse**和**IntelliJ IDEA**。下面也介绍一下两种IDE的安装方式

### IDEA

首先来到IDEA的[官网](https://www.jetbrains.com/idea/download/#section=linux)，点击Download按钮进行下载。

![image-20210823003645953](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/image-20210823003645953-16296502075691.png)

这里介绍一下，Community版本是免费的，面向非商用，Ultimate相比Community解锁了更多的功能，同样的也不能一直免费使用，只有30天的试用期。下载好以后解压到指定目录即可。

```bash
sudo tar -zxvf ideaIC-2021.2.tar.gz
```

解压完成之后进入到对应目录下的``bin``文件夹，在终端输入

```bash
sudo ./idea.sh
```
来启动IDEA

当然如果每次都要进入IDEA的目录然后在终端打开未免太过麻烦。因此我们需要把IDEA添加到我们的启动菜单。添加到启动菜单的方式也很简单。

首先在终端进入``/usr/share/applications``文件夹，然后新建一个idea.desktop的文件

```bash
cd /usr/share/applications
sudo gedit idea.desktop
```

在打开的文本文件中将下面的代码替换进去

```bash
[Desktop Entry]
Version=1.0
Type=Application
Name=IDEA
Icon=/home/godlowd/桌面/idea-IC-212.4746.92/bin/idea.png
Exec=sh /home/godlowd/桌面/idea-IC-212.4746.92/bin/idea.sh
MimeType=application/x-py;
Name[en_US]=IDEA
```

![desktop](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/desktop.png)

其中``Icon``填写IDEA的图标对应的路径，``Exec``填写IDEA文件对应的路径。通常情况下都在解压出来的文件夹中的``bin``文件夹下。填写完之后保存就可以看到开始菜单中新增加了我们的IDEA。

![addIDEA](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/addIDEA.png)

### Eclipse

和IDEA相同，我们进入[Eclipse官网](https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2021-06/R/eclipse-java-2021-06-R-linux-gtk-x86_64.tar.gz) 下载对应的文件，并解压到我们要解压的目录

```bash
sudo tar -zxvf ideaIC-2021.2.tar.gz
```

解压完成之后双击文件夹中的eclipse即可执行文件

![clickEclipse](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/clickEclipse.png)

如果打开之后是显示创建默认工作区（workspace）则说明打开成功：

![launchEclipse](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/launchEclipse.png)

打开Eclipse之后的界面如下图所示:

![eclipseMain](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/eclipseMain.png)

## 创建一个Java项目

打开我们刚刚安装好的IDEA，选择创建Java项目：

![welcomeIDEA](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/welcomeIDEA.png)

点击``New Project``之后跳转到下面的界面。

![createProject](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/createProject.png)

这里可能会在选择项目的JDK版本时遇到一些小问题。默认情况下上面会显示**NO JDK**。如果是通过终端的``sudo apt-get installl``安装的JDK，JDK目录在``/usr/lib/jvm``下面，而不是``/usr/local``下。如果是通过下载的压缩包解压安装，选择压缩包的路径即可。配置好了JDK之后一路Next与Finish就创建了一个空的项目。

![newProject](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/newProject.png)

从上图中可以看到整个项目目前只有一个空的``src``文件夹。在``src``文件夹被选中之后，点击鼠标右键，选择``new class``。然后会要求我们填入新创建的类的名字。这里我们填入``main``。创建好了空类之后，将下面的代码复制进空类，

```Java
class main{
    public static void main(String []args){
        System.out.println("Hello, world");
    }
}
```

点击右上角的``Edit Configuration``。选择刚刚创建的main类，最终如下图所示。

最上面的**Name**用于区分不同的Configuration,同一个项目可以有不同的配置。**Build and run**下面分别是JDK版本以及要运行的main文件。从图中可以看到这里的JDK版本是java 16，而单击``Alt+M``下面的小图标可以选择要运行的主类文件。在一个项目中可以有多个主类，但是同时被运行的只能有一个主类。根据项目的需要，我们可以选择性的提供给程序一定的参数，这些参数可以被放到**Program arguments**中。**Working directory**定义了程序运行的目录，目录主要会涉及一些文件的相对路径。点击OK之后就完成了整个demo项目的文件配置。

![config](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/config.png)

然后点击右上角的绿色三角形，开始运行我们的程序。最终结果如下图所示可以看到在下方的控制台中输出了我们代码中的字符串**Hello, world**，说明我们的项目成功运行起来了

![runCode](%E5%9C%A8%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Java.assets/runCode.png)

