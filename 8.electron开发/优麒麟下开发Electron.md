# 优麒麟下开发Electron

首先，什么是electron? 

> electron 是一款利用 Web技术JavaScript、HTML 和 CSS 开发跨平台（Mac，Windows和Linux系统）桌面应用的开源框架，最初是 Github 发布的 Atom 编辑器衍生出的 Atom Shell，后更名为 Electron。

可以看到Electron本质上是基于Web的一款桌面应用框架，因为其基于Web的特性，我们可以用electron开发跨平台的应用。编写一套代码，只要对应平台可以支持运行Web浏览器和node.js，那么我们的electron应用就可以运行在上面。

使用electron构建的应用有很多已经应用到了非常大型的项目中，比如[Visual Studio Code](https://links.jianshu.com/go?to=https%3A%2F%2Fcode.visualstudio.com%2F)，GitHub的[Atom](https://links.jianshu.com/go?to=https%3A%2F%2Fatom.io%2F)。企业协作工具[slack](https://links.jianshu.com/go?to=https%3A%2F%2Fslack.com%2F)等等。

## 开发环境搭建

在优麒麟上开发electron应用，需要安装以下工具:

* node.js
* electron

### node.js

通过以下命令安装``node.js``

```bash
sudo apt update
sudo apt install nodejs npm
```

上面的命令将会安装一系列包，包括编译和安装从 npm 来的本地扩展。

![installNpm](%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Electron.assets/installNpm-16327477210271.png)

安装完成后运行下面的命令，验证安装过程：

```bash
nodejs --version
```

或者

```
node -v
```

然后在终端会输出当前``node.js``的版本号。

![nodeVersion](%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Electron.assets/nodeVersion-16327477273802.png)

此外我们还需要安装``cnpm``。众所周知`npm`（即 node package manager ）是`Node`的包管理工具，能解决NodeJS代码部署上的很多问题。可是`cnpm`又是什么呢？

> cnpmjs.org: Private npm registry and web for Company
So `cnpm` is meaning: Company npm.

npm是一个完整的npmjs.org的镜像，并且每10分钟与官方镜像进行一次同步，由于npmjs.org的服务器架设在国外，国内的用户访问可能会出现各种各样的问题，譬如包的下载速度极慢，甚至因为下载时间过长而卡死的情况。

通过下面的命令安装cnpm

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

![installCnpm](%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Electron.assets/installCnpm-16327477303103.png)

然后就可以通过cnpm安装electron了

```
cnpm install elctron -g
```

之后同样可以通过

```
electron -v
```

确认版本。

![electronInstall](%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Electron.assets/electronInstall-16327477324124.png)

在npm的安装过程中，可能会遇到下面这样的问题

![npmInstallError](%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Electron.assets/npmInstallError-16327477337565.png)

根据命令行的报错我们可以知道，主要问题在在于npm没有权限写入系统中某个指定目录。因此我们只需要在执行命令时加上sudo即可，或者利用chmod命令赋予npm指定的权限。

## 构建应用程序

在任意一个指定名录，创建一个空的文件夹作为新的electron的文件夹

```bash
mkdir first-app		
```

然后通过npm初始化项目，生成package.json文件:

```bash
npm init
```

![npmInit](%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Electron.assets/npmInit-16327477615317.png)

随后会要求自己配置项目的名称，版本，描述，主要入口，脚本和作者等信息。测试用途的话上面均可以选择默认。

最后生成的package.json文件格式如下：

```json
{
  "name": "first-app",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

在scripts字段，我们需要添加一个start脚本来告诉electron如何去执行当前的package。这里我们将原来的package.json文件修改成下面的样式

```json
{
  "name": "first-app",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "scripts": {
  	"start": "electron .",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

### 添加启动程序代码

在main.js文件中添加下面的代码

```javascript
{
  const {app, BrowserWindow} = require('electron')
  
  // Keep a global reference of the window object, if you don't, the window will
  // be closed automatically when the JavaScript object is garbage collected.
  let win
  
  function createWindow () {
    win = new BrowserWindow({width: 1920, height: 1080})
  
    win.loadFile('index.html')
  
    win.on('closed', () => {
      win = null
    })
  }
  
  app.on('ready', createWindow)
  
  app.on('window-all-closed', () => {
    if (process.platform !== 'darwin') {
      app.quit()
    }
  })
  
  app.on('activate', () => {
    if (win === null) {
      createWindow()
    }
  })
}
```

在index.html文件中添加下面的代码;

```html
<!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <title>Hello World!</title>
    </head>
    <body>
      <h1>Hello Electron!</h1>
    </body>
  </html>
```

然后在终端进入项目的文件夹，输入下面的指令，即可看见运行效果。

````
electron .
````

![helloElectron](%E4%BC%98%E9%BA%92%E9%BA%9F%E4%B8%8B%E5%BC%80%E5%8F%91Electron.assets/helloElectron-16327477381706.png)
