###Node.js 第三天笔记

####一、HackerNews 步骤  (见文档 : 01-HackerNews全过程.md)

####二、underscore

- 模板引擎, 给静态 html 页面赋值

- 安装: `npm i underscore -SD`

- 加载:`var _ = require('underscore')`

- 使用:

  ```js
  //1. 模板 html `字符串`
  var htmlStr = '你好<h1><%= name %></h1>';   // 或者通过 fs 读取后的 data.toString('utf8')

  //2. 模板函数
  var fn = _.template(htmlStr);

  //3. 传值  -- 必须是以对象的形式传
  fn({name:'前端'})
  ```

  ​

####三、node.js 模块 介绍  ==> { 02-node.js模块介绍.md }



####四、模块化   ====>  先阅览下面 6.1 ====>  { 03-模块化开发 }

##### 6.1 什么是模块？

- 每个.js文件就是一个模块

- 从npm上下载的一个包（可能是由多个文件组成的一个实现特定功能的包）也是一个模块

- 任何文件或目录只要可以被Node.js通过`require()`函数加载的都是模块

- 每个模块就是一个独立的作用域，模块和模块之间不会互相"污染"

- 我们可以通过编程的方式，指定某个模块要对外暴露的内容（其实就是指定require的返回值，通过require的返回值对外暴露指定内容）。这个对外暴露内容的过程也叫"导出" `module.exports`

  ​

