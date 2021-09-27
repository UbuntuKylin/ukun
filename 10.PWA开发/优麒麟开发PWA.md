# 优麒麟开发PWA

PWA这个名词可能很多人都没有听说过，他也是最近新出的一个概念，主要目的是结合Web应用与原生应用的两大优势

> PWA 指的是使用指定技术和标准模式来开发的 Web 应用，这同时赋予它们 Web 应用和原生应用的特性。
>
> 例如一方面，Web 应用更加易于发现：相比于安装应用，访问一个网站显然更加容易和迅速。你还可以通过链接来分享 Web 应用。
>
> 另一方面，原生应用与操作系统可以更加完美的整合，也因此为用户提供了无缝的用户体验。你可以通过安装应用使得它在离线的状态下也可以运行；相较于使用浏览器访问，用户也更喜欢通过点击主页上的图标来访问它们喜爱的应用。
>
> PWA 赋予了我们创建同时拥有以上两种优势的应用的能力。
>
> 这并不是一个新概念；类似的想法已经在过去的 Web 平台上通过各种方法实现了多次。渐进式增强和响应式设计已经可以让我们构建对移动端友好的网站。在多年以前的 Firefox OS 的生态系统中，离线运行和安装 Web 应用就已经成为了可能。
>
> 除此之外，PWA 还提供了更多的特性，并且无需在 Web 已有的那些优秀特点上妥协。

举一个很简单的例子，对于一个原生应用而言，即便是最微小的改动也需要强制用户去再次下载整个应用，但是Web应用只需要更新发生改变的那部分内容即可，对于开发提效是很有帮助的。下面通过一个例子来解释什么是PWA以及如何在优麒麟上开发PWA应用。因为PWA是在前端应用的基础上构建而来，因此推荐掌握了一定前端基础再来阅读本篇文章。

## 环境准备

* 一个现代浏览器，如chrome,firefox等等。
* 一个html文件，用来显示PWA应用的主页，这里采用最简单的html文件来演示

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>CNodeJS PWA</title>
    <link rel="stylesheet" href="app.css" />
  </head>
  <body>
    <!-- ,
  "splash_pages": null -->
    <h1>CNodeJS</h1>
    <div id="main"></div>
    <script src="app.js"></script>
  </body>
</html>
```

* CSS样式文件

```css
.topic {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  padding-top: 10px;
  padding-bottom: 10px;
  border-bottom: 1px dashed #222;
}

.topic:first-child {
  padding-top: 0;
}

.topic:last-child {
  border-bottom: 0;
}

.topic img {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  margin-right: 10px;
}

.topic .title {
  -webkit-box-flex: 1;
      -ms-flex: 1;
          flex: 1;
  font-weight: 500;
  font-size: 18px;
}
```

* 对应的脚本文件

```javascript
const main = document.getElementById("main");

window.addEventListener("load", e => {
  loadTopics();
});

async function loadTopics() {
  const res = await fetch(`https://cnodejs.org/api/v1/topics`);
  const json = await res.json();
  main.innerHTML = await json.data.map(createTopic).join("\n");
}

function createTopic(topic) {
  return `
    <div class="topic">
      <img src="${topic.author.avatar_url}" alt=""/>
      <div class="title">${topic.title}</div>
    </div>
  `;
}
```

![createFile](%E4%BC%98%E9%BA%92%E9%BA%9F%E5%BC%80%E5%8F%91PWA.assets/createFile-16327589123721.png)

## 支持添加到主屏幕(A2HS)

添加到主屏幕（Add to Home Screen，简称 A2HS）是现代智能手机浏览器中的一项功能，使开发人员可以轻松便捷地将自己喜欢的 Web 应用程序（或网站）的快捷方式添加到主屏幕中，以便用户随后可以通过单击访问它。通过将我们的PWA应用添加到主屏幕，可以为Web应用程序提供与原生应用程序相同的用户体验。

现在大部分浏览器都支持了添加到主屏幕这一操作。但是为了让我们开发的PWA应用支持A2HS，它需要满足以下条件。

* 通过 HTTPS 提供服务
* 从 HTML 头链接具有正确字段的 manifest 文件。
* 有合适的图标可显示在主屏幕上。
* Chrome 浏览器还要求该应用程序注册一个 Service Worker（这样在离线状态下就也可以运行）。

### manifest文件

manifest文件定了PWA的相关行为以及其某些信息，manifest文件需要以.webmanifest结尾，文件名可以任意。

需要的字段如下

* `background_color`:指定在某些应用程序上下文中使用的背景色
* `display`:指定应如何显示应用，为了让PWA应用看起来更像是一个应用程序，这里应该填写`fullscreen`
* `icons`:指定在不同位置显示的图标
* `name/short_name`：提供了在不同位置表示应用程序时要显示的应用程序名称。
* `start_url`：提供启动添加到主屏幕应用程序时应加载的资源的路径。

一个标准的manifest文件如下：

```
{
  "background_color": "purple",
  "description": "Shows random fox pictures. Hey, at least it isn't cats.",
  "display": "fullscreen",
  "icons": [
    {
      "src": "icon/fox-icon.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ],
  "name": "Awesome fox pictures",
  "short_name": "Foxes",
  "start_url": "/pwa-examples/a2hs/index.html"
}
```

完成了manifest文件编写之后还需要在应用程序的主页的html文件中引用它：

```html
<link rel="manifest" href="manifest.webmanifest">
```

## Service workers离线工作

Service Worker 是浏览器和网络之间的虚拟代理，不仅仅提供离线功能，还可以做包括处理通知、在单独的线程上执行繁重的计算等事务。Service workers 非常强大，因为他们可以控制网络请求、修改网络请求、返回缓存的自定义响应，或者合成响应。

给PWA应用添加上了网页缓存之后，即使是在离线情况下也可以正常访问资源。

通过

```javascript
navigator.serviceWorker.register("sw.js");
```

即可注册ServiceWorker。因为要注册的文件名是``sw.js``，因此需要在项目的根目录下创建``sw.js``文件。然后在sw.js文件中添加如下代码，注册对网页的事件。

```javascript
self.addEventListener("install", async event => {
  console.log('sw install');
});

self.addEventListener("fetch", async event => {
  console.log('sw fetch');
});
```

运行网页后，看到控制台里有打印 `sw install` 字样, 表示 serviceWorker 注册成功。然后可以在对应的事件监听函数中加入我们感兴趣的代码。