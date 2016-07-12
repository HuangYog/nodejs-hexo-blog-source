---
title: Express4.*配置说明②
date: 2016-06-15 19:50:59
tags:
  - 我的Blog
  - Express
categories:
- 日志
- 笔记
---

Express 可以通过在app.js文件中添加(或是修改默认)配置来实现更好的应用。

### 基本的配置说
  app.set(key,value) 可以配置web应用程序，通过app.get(key) 可以得到值，如下配置:
    1. 设置动态页目录
      app.set('views', path.join(__dirname, 'views'));
    2. 设置动态模版引擎，这里用的是默认的jade引擎，还可以选其他的如 Haml,Jade,EJS,CoffeeKup 等等
      app.set('view engine', 'jade');
    3. env 运行时的环境，默认为process.env.NODE_DEV 或是 ‘development’
    4. trust proxy 激活反向代理，默认未激活状态
    5. jsonp callback name 修改默认 ？callback=的jsonp回调的名称
    6. json replacer JSON replacer 替换时的回调，默认为null
    7. JSON spaces JSON响应的空格数量，开发环境下是 2， 生产环境下是 0
    8. case sensitive routing 理由的大小写敏感，默认是关闭状态即 '/Foo' 和'/foo' 效果是一样的
    9. strict routing 路由的严格模式，默认情况小'/foo' 和'/foo/' 是一样的
    10. view cache 模板缓存，在生产环境中默认是开启的
    11. view engine 模板引擎
    12. views 模板的目录,默认的是 'process。cwd() + ./views'

### 配置加载中间件
  1. 通过app.use()方法配置可以加入中间件,扩展web应用程序的功能，自己也可以方便的开发中间件。express默认配置的中间件有:
  {%codeblock%}
    app.use(favicon());
    app.use(logger('dev'));
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded());
    app.use(cookieParser());
    app.use(express.static(path.join(__dirname, 'public')));
  {%endcodeblock%}
  2. 可以通过app.use()实现多个静态目录的配置,加载顺序会先从public目录找，如果没有找到再去查找test目录。
  {%codeblock%}
    app.use(express.static(path.join(__dirname, 'public')));
    app.use(express.static(path.join(__dirname, 'test')));
  {%endcodeblock%}

### 模版引擎的配置
  1. 通过调用app.engine('haml',engine.haml);这样就可以对特定扩张名的动态模板文件进行解析，如:
  {%codeblock%}
    var engines = require('consolidate');
    app.engine('haml', engines.haml);
    app.engine('html', engines.hogan);
  {%endcodeblock%}
  consolidate 是一个库，可以铺平各个引擎集成到express差异性，因为并不是每个第三方引擎都遵守约定，所以consolidate起到中间翻译的作用，***需要注意*** app.engine和app.set('view engine', 'ejs') 之间的关系可以理解为你想改变对不同扩展名的动态模板进行解析，就要用到app.engine,就像下面的情况:
  {%codeblock%}
    var engines = require('consolidate');
    app.set('view engine', 'html');
    app.engine('html', engines.jade);
  {%endcodeblock%}
  模版引擎采用了html引擎，这个很奇怪，其实是由于 app.engine('html', engines.jade); 这段代码注册了 html 引擎，而实际的解析是采用 jade 解析，就形成了可以对html扩展名文件进行解析，采用的解析方式是 jade.
