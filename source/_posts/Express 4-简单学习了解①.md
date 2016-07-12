---
title: Express 4.* 简单学习了解①
date: 2016-06-14 14:22:00
tags:
- nodejs
- 我的Blog
- express4.*
categories:
- 日志
- 笔记
---

**安装和使用**

因为express4.*后的版本要安装express只是安装express的命令程序(用于生成express应用程序的工具),并不是express库,如下:
{%codeblock%}
$ npm install -g express-generator  // -g 表示安装成全局的
{%endcodeblock%}
查看是否安装成功:
{%codeblock%}
$ express -V //大写的V 查看express 的版本号
{%endcodeblock%}

**快速开发**

建立一个express项目，并进入到项目目录:
{% codeblock %}
$ express nodejs-express & cd nodejs-express
{% endcodeblock %}
进入项目目录后安装依赖模块:
{% codeblock %}
$ npm install
{% endcodeblock %}
启动项目:
{% codeblock %}
$ npm start
{% endcodeblock %}

**程序目录介绍**

  * app.js 主程序
  * node_modules 依赖模块包
  * views 动态页目录
  * public 静态网页目录
  * bin 启动程序(bin/www)
  * package.json npm命令对项目初始化产生的描述项目的信息

启动程序是 bin/www介绍:
{%codeblock%}
var debug = require('debug')('nodejs-express');
// 加载主程序
var app = require('../app');
//如果不没有指定服务器监听端口号，默认使用3000
app.set('port', process.env.PORT || 30000);
// 启动程序，开始监听客户端请求
var server = app.listen(app.get('port'), function(){
  debug('Express server listening on port ' + server.address().port);
});
{%endcodeblock%}
可以通过命令指定一个启动端口号:
{%codeblock%}
$ PORT=8000 npm start
{%endcodeblock%}

app.js主程序关键代码说明:
{%codeblock%}
// 创建应用程序
var app = express();
// 设置动态页目录
app.set('views', path.join(__dirname, 'views'));
// 设置动态页模版引擎
app.set('view engine', 'jade');
// 中间件加载，这里不多说，之后章节会详细介绍
app.use(favicon());
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded());
app.use(cookieParser());
// 指定静态页目录
app.use(express.static(path.join(__dirname, 'public')));
// 加载路由处理器
app.use('/', routes);
app.use('/users', users);
// 如果有任何错误都会设置成 404错误
app.use(function(req, res, next) {
    var err = new Error('Not Found');
    err.status = 404;
    next(err);
});
// 如果环境设置成 开发模式，那么会打印出详细的错误信息
if (app.get('env') === 'development') {
    app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('error', {
            message: err.message,
            error: err
        });
    });
}
// 生产模式下，不会泄漏error
app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
        message: err.message,
        error: {}  // 不会泄漏error
    });
});
{%endcodeblock%}

routes/index.js  注释说明:
{%codeblock%}
var express = require('express');
//创建一个路由器
var router = express.Router();

/**
 * 添加路由出来器，也就是为‘/’请求路由加入处理器
 * 当有人访问时会执行这个函数
 * req 是请求对象, res 是响应对象
 */
router.get('/', function(req, res) {
  // 调用响应对象的render返回给客户端一个响应结果
  // 这个响应结果是由动态页面 views/index.jade 生成
  res.render('index', {title: 'Express'});
});

module.exports = router;
{%endcodeblock%}
