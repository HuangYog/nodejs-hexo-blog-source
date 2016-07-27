---
title: Express-Request使用④
date: 2016-06-17 22:00:41
tags:
  - 我的Blog
  - Express
categories:
  - 日志
  - 笔记
---

### request 是客户端请求的抽象,这个对象是只读的，它包含了客户请求的信息，直接上例子吧:
{%codeblock%}
  // /user/id001
  app.get('/user/:id', function(req, res, next){
    var userid = req.params.id; // id001
  });
{%endcodeblock%}
this 也可以通过req.params[0]获得id参数,req.query：
{%codeblock%}
  // /user?id=id001&name=foo
  app.get('/user', function(req, res, next){
    var userid = req.query.id; // id001
    var name = req.query.name; // foo
  });
{%endcodeblock%}
### req.param(name),这个方法和params的作用是一样的:
{%codeblock%}
  // /user/id001
  router.get('/user/:id', function(req, res){
    // var userid = req.params.id; // id001
    var userid = req.param('id'); // id001
  });
{%endcodeblock%}
### req.route 可以获取路由的信息
  {%codeblock%}
    app.get('/user/:id', function(req, res){
      console.log(req.route);
    });
  {%endcodeblock%}
  通过访问 /user/12输出的结果为:
  {%codeblock%}
    {path:'/user/:id?'
      keys:[{name: 'id', optionl: true}],
      regexp: /^\/user(?:\/([^\/]+?))?\/?$/i,
      params:[id: '12']
    }
  {%endcodeblock%}
  这些信息有时候会很有用。

### req.cookies
  {%codeblock%}
    req.cookies.name //获取cookies信息
  {%endcodeblock%}

### req.signedCookies
  获取签名cookies信息,党我们在cookieParser(secret) 插件应用sercret时cookie是经过签名的:
  {%codeblock%}
    // 真实cookie值是 user=leo.CP7AWaXDfAKIRfH49dQzKJx7sKzzSoPq7/AcBBRVwlI3
    req.signedCookies.user //可以得到抛弃签名后的cookie，即 foo
  {%endcodeblock%}

### req.get(field) 得到请求头部信息，field不区分大小写
  {%codeblock%}
    req.get('Content-Type'); // => 'text/plain'

    req.get('content-type'); // => 'text/plain'
  {%endcodeblock%}
  它的别名是req.header(field)

### req.accepts(types) 检查给定的类型使可以接受的：
  {%codeblock%}
    // Accept: text/html
    req.accepts('html'); // => 'html'

    // Accept: text/* application/json
    req.accepts('html'); // => 'html'

    req.accepts('text/html'); // => "text/html"
    req.accepts('json, text');// => "json"

    req.accepts('application/json'); // => "application/json"

    // Accept: text/*, application/json
    req.accepts('image/png');
    req.accepts('png');// => undefined 不允许的类型，将返回undefined

    // Accept: text/*;q=.5, application/json
    req.accepts(['html', 'json']);
    req.accepts('html, json');// => "json"
  {%endcodeblock%}
  req.acceptsCharset(charset) 检查给定的字符集是可以接受的。
  req.acceptsLanguage(lang) 检查给定语言是可以接受的。
  req.is(type)检查请求对象的内容类型，内部根据Context-Type判断。
  {%codeblock%}
    // 如果头部信息是 Content-Type: text/html; charset=utf-8
    req.is('html');
    req.is('text/html');
    req.is(text/*);
    // 都会得到true

    // 如果请求头部信息 是 Content-Type is application/json
    req.is('json');
    req.is('application/json');
    req.is('application/*');
    // 都会得到 true

    req.is(html); // => false
  {%endcodeblock%}
  req.ip 获取客户端ip
  req.ips 返回代理ip和客户端ip，['客户ip','代理ip2'，'代理ip1']
  req.path 返回请求路径
  {%codeblock%}
     // /user？sort=desc
     req.path; // => /user
  {%endcodeblock%}
  req.host 返回主机名称
  {%codeblock%}
    // Host: 'localhost:8080'
    req.host; // ==> 'localhost'

    // Host: '192.168.1.101:8080'
    req.host; // ==> 192.168.1.101
  {%endcodeblock%}
  req.fresh 测试使用的req是否新鲜，true/false, 如果客户端没有这个资源的缓存，就说明这个请求是新鲜的，如果 fresh=false 这时我们可以调用res.status(304)，这样客户端会直接应用缓存的内容，节省了服务器的资源.
  req.stale 与req.fresh相反，测试请求req是否是陈旧的，true／false
  req.xhr 请求是否是ajax方式，true／false
  req.protocol 请求协议，http / https
  req.secure 是否为安全协议 req.protocol === “https”
  req.subdomains 得到子域名数组。 // Host: "a.book.jsera.net" req.subdomains // => ["book", "a"]
  req.originalUrl 得到完整请求地址
  req.url 这里的url是路由本身域内的：
  {%codeblock%}
    // 访问 /user/foo/id001
    var router = express.Router()；
    router.get('/:name/:id', function(req, res){
      console.log(req.originalUrl); // /user/foo/id001
      console.log(req.url); // /foo/id001
    });
    app.use('/user', router);
  {%endcodeblock%}
