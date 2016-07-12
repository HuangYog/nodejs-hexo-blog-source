---
title: Express4-Response使用⑤
date: 2016-06-18 15:35:54
tags:
  - 我的Blog
  - Express
categories:
  - 日志
  - 笔记
---
res.status(code); 设置http 响应码，比如: res.status(404);
res.set(field, [value]) 设置响应头信息:
{%codeblock%}
  res.set('Content-Type', 'text/plain');
{%endcodeblock%}
或是用json 的写法:
{%codeblock%}
  res.set({
  'Content-Type': 'text/plain',
  'Content-Length': '123',
  'ETag': '123456'
  });
{%endcodeblock%}
res.set(field,[value]) 还可以写成 res.header(field,[value]);
res.get([value]); 得到响应头的信息；
res.cookie(name, value, [options]); 设置cookie name 值为value,接受字符串参数或者JSON对象，path 属性默认为'/'.
res.cookie('name', 'tobi', {domain: 'example.com', path: '/admin', secure: true});
res.cookie('rememberme', '1', {expires: new Date(Date.noe() + 900000), httpOnly: true});
maxAge 属性是一个便利的设置'expires',它是一个从当前时间算起的毫秒：
{%codeblock%}
  res.cookie('rememberme', '1', {maxAge: 900000, httpOnly: true});
{%endcodeblock%}
可以传一个序列化的json对象作为参数，它会自动被bodyParser()中间件解析。
{%codeblock%}
  res.cookie('cart', {items:[1,2,3,4]});
  res.cookie('cart', {items: [1,2,3]}, {maxAge: 900000});
{%endcodeblock%}
