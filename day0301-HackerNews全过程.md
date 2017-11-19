###HackerNews 案例 全过程

- **学习目标**: 编写一个 http 服务程序, 根据不同的 url 响应不同的html页面

  ``` js
  1. 编写一个 http 程序
  2. 获取不同的 url
  3. 响应不同的 html 页面
  ```

- **步骤如下:**

  1. #####创建 server.js文件 (入口文件) —> 编写一个 http 服务程序

  2. #####注册路由: 响应不同的文本  —> 测试 ok

     - /index           <---- res.end('index')
     - /detail           <---- res.end('detail  ')
     - /submit         <---- res.end('submit  ')
     - /add     get    <---- res.end('addGet')
     - /add     post  <—— res.end('addPost')
     - /404     404   <—— red.end('404 no found page')

  3. #####拖入素材:  resources(静态资源文件) + views (没有模板)

  4. #####首先加载 index 的 index.html

     - 1. 读取文件 ,  2.响应给浏览器
     - 静态资源有问题;

  5. #####访问静态资源

     - 处理静态资源  `req.url.startWith('/resources')`

        ```
        //1. 设置响应头
        //2. 读取静态资源
        //3. 结束响应
        ```

     - mime

       ``` js
       //1. 安装: npm i mime -S
       //2. 加载 var mime = require('mime')
       //3. 使用:  var type = mime.getType(req.url) // 主要看后缀
       ```

  6. #####补全其他文件的 html 加载

  7. #####封装第一波  :    封装渲染页面  ml_render()

     ```js
     res.ml_render() = function(){
       
     } // 挂载到 res 上,,方便模块化使用

     -  封装 渲染页面 = 读取文件 + res.end(data) + 静态资源的 
     -  ml_render(path.join(__dirname, './views/submit.html'), res);
     -  res.ml_render(path.join(__dirname,'./views/submit.html'));
     ```

     ​

  8. #####处理  add get 请求

     - 使用 **url 模块** : 解析 req.url  -> 对象  

       ```js
       URL.parse(req.url,true);
       // 处理: /add?title=123&url=123&text=123;
       ```

     - 逻辑

       ```js
       //1. 步骤
        - 从 data.json 取出字符串数组, 然后转化为 数组
        - 添加新的对象,生成新的数组
        - 再将新的数组转化为字符串写入 data.json
       //2. 为什么要转化对象?
       - 因为对象好处理
       //3. 为什么要有数组
       - 方便后续添加元素,存储集合
       //4. 转变
       //4.1 /add?titile=123 --> {title:123}  --> [].push(newObj) -> '[]' --> data.json
       //4.2 data.json --> '[X,X]' -> [] -> 接上面 -> [].push(newObj) --> ...... 
       ```

     - 重定向

       ```js
       res.statusCode = 301;
       res.statusMessage = 'Moved Permanently';
       res.setHeader('location','/');

       res.end();
       ```

     ------

     ### 	               天数分隔

     -----

  9. ##### 处理  add post 请求

     - 步骤:

       ```js
           //1. 先从 data.json 取出字符串数组,然后转化为真正的数组
           //2. 获取新的对象  
           //3. 数组添加新的对象
           //4. 把数组转化为字符串,写入 data.json
           //5. 重定向
       ```

       ​


     - 使用 **querystring 模块** : 解析 **查询字符串**  -> 对象

       ```js
       var obj = querystring.parse(buffer)  ;
       // 处理 查询字符串 --> 对象
       ```
    
     - 获取新的对象(其实是查询字符串)
    
       ```js
       // - ★ 获取新对象 (post 请求的数据是 buffer 一段一段传的,我们需要监听 data 和 end) ★
       //1. 定义一个空 buffer 数组
       var bufferArr = [];
       //2. 监听 data 和 end 事件
       req.on('data',function(chunk){
         bufferArr.push(chunk);
       })
       req.on('end',function(){
         var queryStr = Buffer.concat(bufferArr);//生成查询字符串,,然后通过 querystrig 转化即可
       })
       ```
    
     - 重定向 (见 add get)
    
     - 注意:
    
       ```js
       -  修改:   <form method="post" action="/add">
    
         - 获取新对象 (post 请求的数据是 buffer 一段一段传的,我们需要监听 data 和 end)
    
         - 注意坑.不要把 res.end() 放在外面,以为是异步,就不会监听 data 了
    
       ```


     #####

  10. #####导入素材 :  有模板 views 文件夹

  11. ##### 处理 index.html 页面

      - 从 data.json 里读取数据

      - 获取的数组 , 将要与 underscore 模板引擎 配合使用

        ```js
        //字符串数组 转成真正的数组
        var list = JSON.parse(data || '[]');

        res.ml_render(path.join(__dirname,'./views/submit.html'),list); //参数 list
        ```

      - underscore

        ```js
        //1. 模板'字符串'
        var tplStr = '<h1><%= name %></h1>'
        //2. 模板函数
        var template = _.template(tplStr);
        //3. 传值,并生成一个有数据的 html 字符串
        var newHtml = template({ name : '星哥' })
        ```

  12. #####处理 detail.html 页面

      - 获取 id 发现没有 id ,处理之前添加的地方,给对象添加 id

      - 遍历数组,找到 id 匹配的对象

  13. ##### 封装第二波:

      - 封装一: 读取数据从 data.json

        ```js
        //1. function ml_readData(callback){
        ... fs.readFile()
        }
        //2.通过 callback 返回值
        ```

      - 封装二: 写入数据从 data.json

        ```js
        //1. function ml_writeData(list,callback){
        ... fs.readFile()
        }
        //2.通过 callback 返回值
        ```

      - 封装三:  postbody

        ```js
        //1. function ml_postBody(){
          ....data + end
        }
        //2. //2.通过 callback 返回值

        //3. 使用 注意
        //把 buffer数组拼组成一个完整的 buffer

          var postBody =  Buffer.concat(bufferArr);

          postBody = postBody.toString();

          console.log(postBody); //title=44&url=444&text=444

          //4. 把字符串转化为对象

          postBody =  queryString.parse(postBody);

          postBody.id = list.length;

          list.push(postBody);
        ```

        ​

      ​

-------

  ###	                模块开发

-----

  > 1. 内容 - 该模块封装什么内容?
  > 2. 参数 - 该模块是否需要参数,来决定是导出对象还是函数?
  > 3. 模块 - 是否需要加载其他模块? 

  1. #####config.js     —— 配置模块

     ```js
     var path = require(path);
     module.export = {
       port:8080,
       filePath: path.json(__dirname,'./data/data.json')
     }
     ```

  2. #####context.js  —— 拓展模块

     ```js
     module.exports = {
       req.url = req.url.toLowerCase();  //  小写
       res.ml_render = function(){
           //.....
       }
     }
     ```

     ​

  3. #####router.js    —— 路由模块

     ```js
     //1. 创建 router
     module.exports = function(res) {
       if(){
          ...
       }
     }
     //2. 使用
     router(res);
     ```

     ​

  4. #####hanlder.js   —— 业务模块

     ```js
     module.exports.index = function(){ //... }
     module.exports.submit = function(){ //... }
       //...
     ```

     ​

#### ★ 封装的自定义方法

- #####ml_reader()   —  渲染方法

  ```js
  //挂载一个方法 渲染 render
      res.ml_render = function (filename,tplData) {

        //读取文件
        fs.readFile(filename, function (err, data) {

          //如果读取不到,返回 404
          if (err) {
            res.writeHead(404, 'Not Found')
            res.end('404 no found page')
          }

          // 样式
          res.setHeader('content-type',mime.getType(filename));

          if (tplData) {
              //buffer 转化为 html 字符串
              //1. data.toString()
              var oldHtml = data.toString('utf8');
              //2. 模板函数
              var template =  _.template(oldHtml);
              //3. 传值
              var newHtml =  template({list:tplData})
              res.end(newHtml);
          }else{
              // 结束响应
              res.end(data);
          }
        })
      }
  ```

- #####ml_readData()  — 读取文件方法

  ``` js
  // 读取文件
  function  ml_readData( callback) {

    // 在异步里返回值,,用回调函数
    fs.readFile(path.join(__dirname,'./data/data.json'),'utf8',function (err,data) {
      if (err && err.code !== 'ENOENT') {
        throw err;
      }
      // 回调函数回传值
      callback(data)
    });
  }
  ```

- ##### ml_writeData()  — 写入文件方法

  ```js
  // 写入文件
  // 参数1: 要写入的数组
  // 参数2: 回调  写入成功,,告诉他们,我成功了
  function ml_writeData(list, callback) {

    fs.writeFile(path.join(__dirname,'./data/data.json'),JSON.stringify(list),function(err){
      if (err) {
        throw err
      }
      callback();
    })
  }
  ```

- #####postBody()    post  —  请求传过来的对象

  ```js
  // postBody
  function ml_postBody(req,callback) {

    var bufferArr = [];
    req.on('data',function (chunk) {

      bufferArr.push(chunk);
    })
    req.on('end',function () {

      //把 buffer数组拼组成一个完整的 buffer
      var postBody =  Buffer.concat(bufferArr);
      postBody = postBody.toString();

      callback(postBody);

    })
  }
  ```

  ​

-------



###涉及知识点:

1. 内置模块 url ,转化 包含查询字符串的 req.url

```js
 - var URL = require('url')

 - URL.parse(req.url)  -> query 依然是查询字符串

 - URL.parse(req.url,true)  -> query 才是一个 url 对象

```

2. 内置模块 queryString, 查询字符串 转化为对象

```js
  - var queryString =  require('queryString');

  - querystring.parse(bodypost)

```

3. 第三方模板: mime 

   ```js
    - 静态资源转化样式 防止报警告

    - var mime = require('mime')

    - mime.getType('路径')
   ```

   ​

4. 第三方模块: underscore

   ```js
   var _ = require('underscore');

   //使用 underscore 三步走

   //1. 有个模板文件 

   var oldHtml = '<h1><%= name %></h1>'

   //2. 生成模板函数 

   // 参数: 模板文件

   // 返回值: 模板函数

    var template = _.template(oldHtml)

   //3. 传值

    var newHtml = template({ name:' 小新哥' })

    console.log(newHtml);

   ```

   ​

###其他:

```js


\1. 读取文件如果发生错误,不需要 throw err ,因为这是做项目了

if (err) {

  res.writeHead(404,'Not Found')

  res.end('404 no found page')

}

\2. 处理大小写 因为 method = GET 和 POST

req.url  = req.url.toLowerCase();

req.method = req.method.toLowerCase();

\3. 数组转化为字符串

JSON.stringify(list)

\4. 重定向

res.statusCode = 301;

res.statusMessage = 'Moved Permanently';

res.setHeader('Location','/');

res.end()  // 一定要加上这个

\5. 错误,又不是文件不存在的错误处理

if (err && err.code !== 'ENOENT') {

      throw err;

}

\6. 拼接 buffer 的传递

var bufferArr = [];

req.on('data',function (chunk) {

  console.log('111')

  bufferArr.push(chunk);

})

req.on('end',function () {

  console.log('22')

 //把 buffer数组拼组成一个完整的 buffer

  var postBody =  Buffer.concat(bufferArr);

  postBody = postBody.toString();

  console.log(postBody);//title=44&url=444&text=444

  //4. 把字符串转化为对象

  postBody =  queryString.parse(postBody);

  postBody.id = list.length;

  list.push(postBody);

}

  7.注意,,不要在最后写 res.end(), 因是异步的 因为结束响应,,就不会传了

  ml_readData(function(data){

<!--   

  })

  var  data = null;

 ...

 data = 13 -->

 console.log(data)

 

  })

```

