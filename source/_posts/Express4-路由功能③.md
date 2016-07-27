---
title: Express4*路由功能③
date: 2016-06-15 21:00:54
tags:
  - 我的Blog
  - Express
categories:
  - 日志
  - 笔记
---

## 路由器、响应与渲染
  {%codeblock%}
  //router路由器，根据 /book 知道应该用相应的handle处理器来处理这个请求
  router('/book',function handle(req, res) {
    req // 请求对象，记录有客户端请求的所有信息，告诉你，他要什么
    res // 响应对象，你可以通过它，把数据交给客户端
  });
  {%endcodeblock%}

### 路由器(router)
express 的路由器表现形式是:
{%codeblock%}
//客户端通过get方式进行请求
app.get('/book', function router_handle(req,res) {
  ... //
  });

// 客户段通过post请求
app.post('/save', function router_handle(req,res) {
  ... //
});
{%endcodeblock%}
除了post、get请求外还有 delete、put或是自己创建的方法，如果浏览器部支持，可以通过一个参数搞定:
{%codeblock%}
  <form mothod = 'post'>
    <input type = 'hidden' name = '_method' value = 'put'/>
  </form>
{%endcodeblock%}
这样还不够，我们还需要在服务端加上一个插件(添加插件这里就不详细介绍，在上一节中有介绍过)。
{%codeblock%}
  var methodOverride = require('method-override');
  app.use(methodOverride('_method'));
{%endcodeblock%}

为了提高代码的可读性很复用性可以采用express.Router类将 app.get()和app.post() 组织为更好的路由方式.如下创建一个userRouter.js
{%codeblock%}
  var express = require('express');
  var router = express.Router();
  //浏览地址为localhost:port/foo
  router.get('/', function(req, res) {
      console.log('浏览器会看到json 数据');
      //浏览器会看到json 数据
      res.send({title: 'hello world'});
    });

  //浏览地址为localhost:port/foo/down
  router.get('/down', function(req, res) {
      console.log('下载数据');
      //浏览器会下载.log文件
      res.download('./npm-debug.log');
  });

  //浏览地址为localhost:port/foo/render
  router.put('/render', function(req, res) {
      console.log('渲染index.html 文件');
      //会渲染一个页面
      res.render('index.jade', {
          title: 'hello world'
      });
  });

  module.exports = router;
{%endcodeblock%}
在 app.js 中添加路由代码如下:
{%codeblock%}
  app.use('/foo',require('./routes/userRouter'));
{%endcodeblock%}
一个web应用程序可能会有很多的路由，我们可以通过这中简单切换的方式来模块化路由，便于我们对每个功能的管理。

### 页面渲染
  页面渲染也可以叫动态页面渲染，动态模版引擎有很多，最终都是要生成一个html页面，无论哪个引擎再高深，原理只有一个：“ 模版引擎的作用就是，通过把数据注入到模版中，转换一个html页面，一个模版通过给定数据的不同，生成的html页面也不相同，所以叫做动态页面。 ”
  express对页面的渲染主要有下面几种方式:
    * res.send(“hello”); // 浏览器会看到hello
    * res.render(“index.html”,{title:”world”});  // 会渲染一个页面
    * res.download(“/my.pdf”);   // 浏览器会下载一个文件。
  还有很多方法，这里的 res.render 方法是渲染一个页面，但响应方法不只是渲染页面，可以下载,还可以只是一个字符串等等.

### 路由处理器执行顺序
  路由器的处理执行顺序和代码的先后顺序有关，如下代码:
  {%codeblock%}
    app.get('/test', function(req, res, next){
      console.log('router A');
      next();
    });

    app.get('/test', function(req, res, next){
      console.log('router B');
      next();
    });
  {%endcodeblock%}
  这段代码执行后打印的是:
  {%codeblock%}
    router A
    router B
  {%endcodeblock%}
  其实无论我们调用的的 app.use, app.all, app.get, app.post 都是一样的按照编码顺序来执行。***需要注意的是***这个执行顺序的前提是 ***一致的路由***, 一致路由其实就是相同的方法和相同的路径,如下代码:
  {%codeblock%}
    app.get('/test', function(req, res, next){
      console.log('router A');
      next();
    });
    app.post('/test', function(req. res, next){
      console.log('router B');
      next();
    });
  {%endcodeblock%}
  这段代码就没有执行顺序可言了,因为他们请求的是不同的方法，不存在一致的路由,还有***需要注意*** **next** 方法调用之前是不会调用之后的路由处理器，如下代码:
  {%codeblock%}
    app.get('/test', function(req, res, next){
      console.log('router A');
    });
    app.get('/test', function(req, res, next){
      console.log('router B');
      next();
    });
  {%endcodeblock%}
  执行这段代码在控制台只会打印出 **router A**,因为第一个路由处理器没有调用next()方法。
  如果我们想要在浏览器上看到服务器的响应,就需要调用响应方法，如: res.send(),res.render()等等，如下代码:
  {%codeblock%}
    app.get('/test', function(req, res, next){
      console.log('router A');
      next();
    });
    app.get('/test', function(req, res, next){
      console.log('router B');
      res.send('router B');
    });
  {%endcodeblock%}
  这样，浏览器就会显示出**router B**字符串，我们发现第二个路由处理器没有调用next,因为调用next()的目的是执行之后的路由处理器，如果后面没有就不需要调用，另外 **调用任何响应方法都表示响应任务完成**.如下代码:
  {%codeblock%}
    app.get('/test', function(req, res, next){
      console.log('router A');
      res.send('router A');
      next();
    });
    app.get('/test', function(req, res, next){
      console.log('router B');
      res.send('router B');
      next();
    });
    app.get('/test', function(req, res, next){
      console.log('router C');
      res.send('router C');
      next();
    });
  {%endcodeblock%}
  调用后之会在浏览器上显示 router A, 因为**调用了任何响应方法，都表示响应的任务完成**，但是第二个路由处理器还是会执行，只不过所有的响应方法都不会在在起作用，调用上面这段代码在后台输出的是:
  {%codeblock%}
    router A
    router B
    router C
  {%endcodeblock%}

### app.all() 方法
  app.all 表示响应所有http方法,如下:
  {%codeblock%}
    app.all('/test', function(req, res, next){
      conosle.log('app.all method');
      next();
    });
    app.get();
  {%endcodeblock%}
  这样不论我们通过get或是post http方法访问后台终端都会打印app.all method，若用app.use替换app.all效果也一样的，但是use不支持多路由处理器，如下:
  {%codeblock%}
    app.all('/test', function(req, res, next){
      console.log('app.all method');
      next();
    }, function(req, res, next){
      lonsole.log('app 2');
      next();
    });
    app.get('/test', function(req, res, next){
      lonsole.log('app.get method');
      res.send('app.get method');
    });
    app.post('/test', function(req, res, next){
      console.log('app.post method');
      res.send('app.post method');
    });
  {%endcodeblock%}
  执行这段代码打印的是:
  {%codeblock%}
    app.all method
    app 2
  {%endcodeblock%}
  如果我们把all 改为use则会之后打印出:
  {%codeblock%}
    app.all method
  {%endcodeblock%}
  由此可见,app.all , app.get , app.post 方法支持多路由处理链,app.use 方法部支持，但它支持执行express.Router的实例.

### express.Router 类
  一个express.Router 的实例可以理解为一个独立的app,他调用use 方法加入中间件，但是只作用于自身。通过 var router = Express.Router(<可选参数>) 创建一个路由器对象，可选参数有两个，caseSensitive 区分大小写(如:/Foo和/foo)默认是false; strict 严格路由(如: /foo和/foo/)默认false关闭。创建方式如下:
  {%codeblock%}
    var router = express.Router();
    router.get('foo', function(req, res, next){
    res.send('abcdefg');
    });
    app.use('/user', router);
  {%endcodeblock%}
  我们通过 /user/foo可以访问，通过app.use 方法加入到应用程序的路由链上。如下我们程序用用到的模块:
  {%codeblock%}
    app.use('/user', require('./user'));
    app.use('book', require('./user'));
  {%endcodeblock%}
  当然， 它自身的use方法也可以应用其他router,如下:
  {%codeblock%}
    var sub = express.Router();
    sub.get('/sub', function(req, res, next){
    res.send('sub is me');
    });

    var router = express.Router();
    router.use('foo', sub);
    app.use('/user', router);
  {%endcodeblock%}
  通过访问 /user/foo/sub 可以访问到，但实际应用中不会这么做.
  还有就是app 的Router对象是支持糖衣写法的:
  {%codeblock%}
    router.route('/users/:user_id')
    .all(function(req, res, next){

    }).get(function(req, res, next){

    }).post(function(req, res, next){

    }).delete(function(req, res, next){

    });
  {%endcodeblock%}

## 泛式路由
  /user/id 这是明确路由，而 /user/:id 和/user/* 都属于泛式路由.
  /user/:id 这个路由通过，user/id 都可访问到。
  /user/:id? 这个路由通过，/user 和 user/id 都可访问到。
  /user/* 这个路由通过，/user 和 user/id 都可访问到。
  /user/*/id?  这个路由通过，/user 和 user/id001 和 user/xxx/id001都可访问到。
  /user/*/:id  这个路由通过， user/id001 和 user/xxx/id001都可访问到。
