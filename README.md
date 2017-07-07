# wlkg
卧龙控股项目
NodeJS: https://github.com/nodejs/node/ 在github中想要查找某个文件，按t;
例如: 在使用fs.createWriteStream时，文档中没有具体方法，就可以通过查看源码，通过源码找对应的构造函数看有哪些属性设置，查看原型有哪些方法可用；

npm中管理的包
  $ npm ls   // 该方式不方便查看(有太多子集)
  $ npm ls --depth 0  // 即依赖子集为0
  $ npm ls --depth 1  // 即依赖子集的第一层

[nodemon](https://github.com/remy/nodemon):监视您的node.js应用程序的任何更改并自动重新启动服务器
  $ npm install -g nodemon
  $ nodemon xxx.js  // 运行对应文件即可


# 一、文件监视
- fs.watchFile(filename[, options], listener(curr,prev))
```
  //  { persistent: true, interval: 5007 }，即表示持续不断监视，时间间隔为5007毫秒
  options:{persistent,interval}

  const target = path.join(__dirname, process.argv[2]);
  fs.watchFile(target, (curr, prev)=>{
      console.log(`curr:${curr.size}; prev: ${prev.size}`);
});
```

- fs.watch(filename[,options][,listener])

- 利用文件监视实现自动 markdown 文件转换
相关链接: 
Markdown转换: [https://github.com/chjj/marked](https://github.com/chjj/marked)
同步浏览器端: [https://github.com/Browsersync/browser-sync](https://github.com/Browsersync/browser-sync)
 ```
  思路：
  - 利用fs模块的文件监视功能监视指定MD文件
  - 当文件发生变化后，借助marked包提供的markdown to html功能将改变后的MD文件转换为HTML
  - 将得到的HTML替换到模版中(html = template.replace('{{content}}', html).replace('{{style}}', css);)
  - 利用BrowserSync模块实现浏览器自动刷新 
```
```
 markdown的样式有很多可以选择，选取的不同，html结构也会有所不同。
关于github.css主体内容是包裹在div为.vs中：
<body>
    <div class='vs'>{{content}}</div>
</body>
```
```
  BrowserSync模块的使用
    1、$ npm install browser-sync // 安装browser-sync
    2、const browserSync = require("browser-sync");  // 导入模块
    3、通过browserSync创建一个文件服务器
    browserSync({
            server: path.dirname(target),  // 文件服务器的目录
            index:indexName // 在开启服务器之后会打开页面，该页面作为网站根目录
    });
    4、当文件修改后，刷新浏览器页面
    browserSync.reload(indexName);  // fileName是完整路径
```
  > 添加完BrowserSync后，就会在对应目录创建并启动一个服务器，自动打开页面(或手动打开，命令行有提示)http://192.168.1.112:3000 此时是在根目录下，也可以具体访问某个文件http://192.168.1.112:3000/xxx.html
