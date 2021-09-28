# 优麒麟开发Flutter

Flutter是一个由谷歌开发的开源移动应用软件开发工具包，用于为Android、iOS、Windows、Mac、Linux、Google Fuchsia开发应用。而优麒麟也是支持使用Flutter进行开发跨平台应用的。下面简单的介绍一下如何利用优麒麟编写Flutter的跨平台应用。

## 入门

Flutter是用Dart写成的，在[Dart的官网]([https://dart.cn/](https://dart.cn/))可以看到现在最新的版本是2.13。在[开发语言概览]([https://dart.cn/guides/language/language-tour](https://dart.cn/guides/language/language-tour))中对Dart语言进行了一个简要的介绍。

这里只需要做一个简单的了解即可。

## 安装Flutter SDK

在Flutter的[开发文档]([https://flutter.cn/docs/get-started/install/linux](https://flutter.cn/docs/get-started/install/linux))中，我们可以查看安装Flutter的SDK必备的一些命令行工具，如bash, curl, mkdir，以及一些其他的公用库。

Flutter的SDK的安装方式有两种，一种是通过snapd直接进行安装，命令为：

```bash
sudo snap install flutter --classic
```

也可以通过下载安装包解压到指定名录安装。

在**手动安装Flutter部分**，网站提供了最新的Flutter版本压缩包。下载完成之后执行

```bash
tar xf path_to_flutter_sdk.tar.xz 
```

进行解压缩，之后需要将Flutter SDK所在的路径添加到$PATH环境变量中。

执行下面的代码

```bash
export PATH="$PATH:`pwd`/flutter/bin"
```

可以将flutter命令在当前的终端窗口中添加到PATH环境变量中，一旦关闭当前终端窗口就会失效。而要永久添加到环境变量中则需要修改配置文件。在Linux环境下的默认配置文件为~/.bashrc，如果安装了zsh那么终端的默认配置文件就是~/.zshrc。打开配置文件后，添加下面内容到文件的末尾，并将 [PATH_TO_FLUTTER_GIT_DIRECTORY] 改到你 clone Flutter 的 git 仓库目录下。

```bash
export PATH="$PATH:[PATH_OF_FLUTTER_GIT_DIRECTORY]/bin"
```

运行``source ~/.bashrc``或者``source ~/.zshrc``来更新配置文件

要验证flutter是否被正确的添加到环境变量中可以通过以下命令

```bash
echo $PATH
```

或者

```bash
which flutter
```

有更多关于设置环境变量方面的问题可以参考Flutter的[官方指南]([https://flutter.cn/docs/get-started/install/linux#update-your-path](https://flutter.cn/docs/get-started/install/linux#update-your-path))

## 安装IDE

Flutter支持使用Android Studio和VSCode进行开发，二者都需要在安装后之后安装对应的Flutter和Dart插件，才能开始使用。安装两个IDE的过程不再赘述，进入官网选择Linux对应的发行版即可。

以Android Studio为例， 

1. 打开插件偏好设置 (位于 File > Settings > Plugins)
2. 选择 Marketplace (扩展商店)，选择 Flutter plugin 然后点击 Install (安装)。
3. 提示会需要安装Dart插件，同意即可。

![installPlugin](%E4%BC%98%E9%BA%92%E9%BA%9FFlutter%E5%BC%80%E5%8F%91.assets/installPlugin-16327121499381.png)

## 第一个Flutter程序！

安装完成IDE与Flutter插件之后就可以开始我们的开发之路。

1. 打开 IDE，选择 新 Flutter 项目 (Create New Flutter Project).
2. 选择 **Flutter 应用程序** 作为项目类型，然后点 **下一步**
3. 确认 **Flutter SDK 路径** 区域所示路径是正确的 SDK 路径。如果你还没有安装 SDK，需要先进行安装，选择 **Install SDK…**。

![setProjInfo](%E4%BC%98%E9%BA%92%E9%BA%9FFlutter%E5%BC%80%E5%8F%91.assets/setProjInfo-16327121522862.png)

1. 输入项目名称(比如 ‘myapp’), 然后点击**下一步**。
2. 点击 **完成**。
3. 待 Android Studio 安装 SDK 后，创建项目。

![templateProj](%E4%BC%98%E9%BA%92%E9%BA%9FFlutter%E5%BC%80%E5%8F%91.assets/templateProj-16327121559583.png)

在AS的右上角我们可以看到他的工具条：

其中main.dart左边是项目运行的模拟器类型。因为Flutter跨平台的特性，可以选择在iOS，Android，Chrome多种设备上运行。如果列表里面没有可用的设备，选择工具 > AVD Manager 然后在这个窗口中选择并创建一个新的虚拟机。

![addSimulator](%E4%BC%98%E9%BA%92%E9%BA%9FFlutter%E5%BC%80%E5%8F%91.assets/addSimulator-16327121577034.png)

然后单机绿色的小三角形即可运行编写好的Flutter应用

![androidSimulator](%E4%BC%98%E9%BA%92%E9%BA%9FFlutter%E5%BC%80%E5%8F%91.assets/androidSimulator-16327121606145.png)

## 热重载

相比原生开发，Flutter的优势之一就在于，当开发者在开发时，无需重新编译代码，就能实时看到修改代码在UI上的改变。

这里是在热重载之前的画面。

![beforeHotReload](%E4%BC%98%E9%BA%92%E9%BA%9FFlutter%E5%BC%80%E5%8F%91.assets/beforeHotReload-16327121624036.png)

这里我们修改一下显示在屏幕上的文案，点击右上角的热重载按钮（闪电图标），待应用程序响应完成，即可看到UI修改后的结果

![afterHotReload](%E4%BC%98%E9%BA%92%E9%BA%9FFlutter%E5%BC%80%E5%8F%91.assets/afterHotReload-16327121640137.png)

