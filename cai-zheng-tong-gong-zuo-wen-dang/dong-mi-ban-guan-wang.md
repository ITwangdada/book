# 董秘办官网

## 技术栈

* vuejs
* nodejs
* vue-cli3
* vue-router
* vuex
* elementUI
* sass
* webpack
* axios

## 项目目录解构

```text
├── node_modules                   // npm加载的项目依赖模块
├── public                         // 静态文件
│   ├── index.html                 // 静态index.html文件
│   ├── favicon                    // 浏览器icon
│—— src                            // 放置组件和入口文件
│   ├── api                        // api配置，封装了axios，整体api文档对应api.js文件
│   ├── assets                     // 主要存放一些静态图片资源的目录 
│   ├── common                     // 公共文件 
│   ├── components                 // 这里存放的是开发需要的的各种组件
│   ├── view                       // 与路由和业务相关，单个组件联系多个组件组成一个完整的项目。 
│   ├── app.vue                    // 所有vue实例都会挂载到这里
│   ├── main.js                    // 入口js文件
│   ├── router.js                  // 路由文件
│   ├── store.js                   // 数据管理器
│—— package.json                   // 项目及工具的依赖配置文件
│—— vue.config.js                  // cli3配置文件 
│—— babel.config.js                // babel配置文件
│—— README.md                      // 说明文件
```

## run

```text
    git clone 仓库地址
    npm install
    npm run serve
```

## 启动文件

```text
    npm run serve
```

## 打包文件

```text
    npm run build
```

## 代理配置

```text
    在vue.config.js文件下，找到devServer。
    更改proxy属性下target对应的url就ok，其他不用动。
```

