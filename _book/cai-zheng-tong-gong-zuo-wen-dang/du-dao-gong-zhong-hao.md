# 督导公众号

##技术栈
* vue
* vuex
* vue-router
* sass
* webpack
* vue-cli2
* axios

##项目目录结构
```
├── node_modules                   // npm加载的项目依赖模块
├── public                         // 静态文件
│   ├── index.html                 // 静态index.html文件
│   ├── favicon                    // 浏览器icon
├── build                          // 文件夹下存放webpack的一些配置，webpack是前端网站的一种项目编译、运行、打包工具。
├── config                         // 文件包含webpack环境配置文件。
├── doc                            // api文档
│—— src                            // 放置组件和入口文件
│   ├── api                        // api配置，封装了axios，整体api文档对应api.js文件
│   ├── assets                     // 主要存放一些静态图片资源的目录 
│   ├── common                     // 公共文件 
│   ├── components                 // 这里存放的是开发需要的的各种组件，多个组件组成一个完整的项目。 
│   ├── app.vue                    // 所有vue实例都会挂载到这里
│   ├── main.js                    // 入口js文件
│   ├── router                     // 路由文件
│   ├── vuexData                   // 数据管理器
│   ├── filters                    // 全局filter
│   ├── polyfill.js                // 垫片
│—— package.json                   // 项目及工具的依赖配置文件

```

##run
```
git clone 仓库地址
//公众号当中路由是后端重定向的，需要后端配合拼接。
//代理配置需和url的location.host一样，
npm install
npm run dev
//浏览器打开从后端拼接的路由，再将location.host改为本地启动服务
//localhost:4000 (端口号看配置，我这里是随便写的)
//拿到用户的session就行，方法我是用的这个。
```


##打包
```
    npm run build
```
