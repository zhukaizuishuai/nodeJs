###SMS 学生管理系统

```js
 ### 技术栈:
1. express
 - Router
 - static
2. mongodb
3. body-parser
4. ejs
5. 模块化开发

 ### 步骤:
##### 准备工作
1. 环境: npm 安装
2. 静态文件+数据库

##### 步骤;
1. 搭建个最简单的服务器  - (见: server.1.简单的请求.js)
2. 模块化 - config.js 和 router.js 
3. 创建 handler.js 
   -  index.html
   -  students.html -> 要使用到 mongodb findAll
   -  封装到 db.js
  - info 先访问静态页面 -> 获取数据 -> 渲染 -> 封装

  4. 首页 -> 详情页 -> 添加 -> 编辑 ->删除
```

