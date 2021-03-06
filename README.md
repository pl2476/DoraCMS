# doracms 2.0.0

![DoraCMS](http://git.oschina.net/uploads/images/2015/0930/174726_d78c4a23_352304.jpeg "DoraCMS")

## 说明

DoraCMS 使用的技术栈：

1、vue + vuex + vue-router 全家桶

2、webpack 2

3、nodejs 8.0 + express 4

4、mongodb 3+

演示地址： [前端开发俱乐部](https://www.html-js.cn)   
后台登录： https://www.html-js.cn/dr-admin     测试账号：doracms/123456

部署文档： [前端内容管理框架 DoraCMS2.0 部署介绍](https://www.html-js.cn/details/ryn2kSWqZ.html)   

## 目录结构

```
├─build // webpack 相关配置文件
│
├─configs // 配置文件
│  │  
│  └─logConfig.js  // 日志配置文件
│ 
├─logs // 日志目录
│
├─dist  // webpack 生成文档存放目录
│  │
│  ├─server
│  │
│  └─static
│      ├─css
│      │
│      ├─images
│      │
│      ├─img
│      │
│      └─js
│
├─server    // 服务端目录
│  │
│  ├─lib    // 核心层
│  │
│  └─routes // 路由文件
│
├─src           // 客户端程序目录
│  │
│  ├─api        // api 配置文件
│  │
│  ├─filters    // 过滤器
│  │
│  ├─index      // 前台组件
│  │
│  ├─manage     // 后台组件
│  │
│  ├─template   // 初始模版
│  │
│  └─utils      // 实用工具
│
└─utils
    ├─middleware // 中间件
    │
    ├─authPower.js // 资源鉴权
    │
    ├─authSession.js // session 鉴权
    │
    ├─authToken.js // token鉴权
    │
    ├─logUtil.js // 日志配置
    │
    ├─settings.js // 关键信息配置
    │
    └─validatorUtil.js // 信息校验

```





## 准备工作:
安装 NodeJS:
https://nodejs.org/zh-cn/

安装 Mongodb:
https://www.mongodb.com/download-center#community

```shell
# 安装依赖
$ npm install

# 开发模式
$ npm run dev

# 生产模式
$ npm run build

# 启动(需先生成静态文件)
$ npm run start
```

首页
http://localhost:8080

登录
http://localhost:8080/dr-admin

# LICENSE

MIT
