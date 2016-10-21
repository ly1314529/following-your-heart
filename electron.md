#### 最近接触了一个新的东西，很棒，有必要写点东西来加深日后的理解，这个神奇的东西就是electron，是一款跨平台的并且是用js写出来的东西

--------

* electron是什么呢？第一次接触的我不禁发生这样的感触

electron可以使用纯的JavaScript来调用丰富的原生APIs来创造桌面应用，对于只学习了前端的人就更加的方便了，因为你仅仅只需要掌握js,html ,css
就可以创造一个桌面程序出来

* 打造一个简单的应用hello-world
 * 在做这个应用之前，你需要保证你电脑里已经具备以下东西
   * node 在官网上下载好之后，node -v,可以查看node 版本号 [node官网](https://nodejs.org/en/)
   * npm 新版的node集成了npm ，所以直接npm -v即可
   * git 最好还是安装一个git吧，他的命令行跟Linux下感觉很像[git教程](https://github.com/ly1314529/ly/blob/master/nodeschool/git.md)
* 在这个时候你就可以放心的安装electron了
  * electron 要用淘宝镜像安装(原因你懂得)
  ```
  npm install -g cnpm --registry=https://registry.npm.taobao.org
  cnpm install electron-prebuilt -g
  electron -v 有版本号输出就对了
```
* 这下进入重头戏
 
 ```
 cd /e //习惯进e盘，o(╯□╰)o
 mkdir app //create a file
 ```
 * 这时你的app文件下需要有这三个文件
 
 ```
 main.js //用来打开窗口运行app
 index.js //需要展示的页面
 package.json //记录你的app.js和程序名字
 ```
 
* main.js
  
  ```javascript
  // 载入electron模块


var  electron=require("electron");
// 创建应用程序对象
var  app=electron.app;
// 创建一个浏览器窗口，主要用来加载HTML页面
var BrowserWindow=electron.BrowserWindow;

// 声明一个BrowserWindow对象实例
var mainWindow;


function createWindow(){
    // 创建一个浏览器窗口对象，并指定窗口的大小
    mainWindow=new BrowserWindow({
        width:400,
        height:400
    });

    // 通过浏览器窗口对象加载index.html文件，同时也是可以加载一个互联网地址的
    mainWindow.loadURL('file://'+__dirname+'/index.html'); 
    // 同时也可以简化成：mainWindow.loadURL('./index.html');

    // 监听浏览器窗口对象是否关闭，关闭之后直接将mainWindow指向空引用，也就是回收对象内存空间
    mainWindow.on("closed",function(){
        mainWindow = null;
    });
}

// 监听应用程序对象是否初始化完成，初始化完成之后即可创建浏览器窗口
app.on("ready",createWindow);

// 监听应用程序对象中的所有浏览器窗口对象是否全部被关闭，如果全部被关闭，则退出整个应用程序。该
app.on("window-all-closed",function(){
    // 判断当前操作系统是否是window系统，因为这个事件只作用在window系统中
    if(process.platform!="darwin"){
        // 退出整个应用程序
        app.quit();
    }
});

app.on("activate",function(){
    if(mainWindow===null){
        createWindow();
    }
});
```

* package.json

```javascript
{
    "name":"app name", 
    "version":"0.0.1", 
    "main":"main.js" 
}
```

* index.html

```javascript
<html>
<head>
    <title>app</title>
    <meta charset="utf-8"/>
    
</head>
  <body>
  HELLO WORLD!
  </body>
  </html>
  ```
 
 *  开始进行打包成exe  ,需要下载 electron-packager
  
  
  ```
  npm install -g electron-packager  (安装完成)
  
  ```
  
  ```
  electron-packager /e/node-webkit node --platform=win32 --arch=x64 --icon=/e/node-webkit/a14.icon --overwrite --out /e/dist --version=1.0.0  
  
  //打包命令，/e/node-webkit(代表需要打包的路径名，绝对路径，路径不要写错了)  node（app在packag.json中的名字）--platform=win32(打包的平台) --arch=x64（操作系统的位数）--icon=(图标的路径)  --overwrite --out /e/dist（exe打包好后存放的位置） --version=1.0.0 （打包的版本号）
  ```
  ```
//Packaging app for platform win32 x64 using electron v1.0.0
//Wrote new app to E:\dist\node-win32-x64 （输入这个代表打包成功）
 ```
  
  
  
  今天先更到这里，更多资料参考[资料](https://wangdashuaihenshuai.gitbooks.io/electron-zh-document/content/tutorial/quick-start.html)
