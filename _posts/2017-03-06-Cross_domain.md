---
layout: post
title: 浏览器跨域问题
categories: [blog]
tags: [FE]
description: fe
---

# 浏览器跨域问题

* TOC
{:toc}

## 产生原因

Ajax就是使用js构建的异步网络请求。

```
function success(text) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = text;
}

function fail(code) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = 'Error code: ' + code;
}

var request = new XMLHttpRequest(); // 新建XMLHttpRequest对象

request.onreadystatechange = function () { // 状态发生变化时，函数被回调
    if (request.readyState === 4) { // 成功完成
        // 判断响应结果:
        if (request.status === 200) {
            // 成功，通过responseText拿到响应的文本:
            return success(request.responseText);
        } else {
            // 失败，根据响应码判断失败原因:
            return fail(request.status);
        }
    } else {
        // HTTP请求还在继续...
    }
}

// 发送请求:
request.open('GET', '/api/categories');
request.send();

alert('请求已发送，请等待响应...');
```

上述代码中`request.open('GET', '/api/categories')`中URL使用的是相对路径。默认情况下，JavaScript在发送AJAX请求时，URL的域名必须和当前页面完全一致。完全一致的意思是，域名要相同（`www.example.com`和`example.com`不同），协议要相同（`http`和`https`不同），端口号要相同（默认是`:80`端口，它和`:8080`就不同）。

## 解决办法

1. ### Flash

2. ### Proxy transmit

3. ### JSONP

   只能用GET请求，利用浏览器允许跨域引用JavaScript资源，即直接获得含有数据结果的js。所以，只要把URL当成js路径，服务端将data封到js里面返回就可以。

   [示例](http://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html)

   ajax的核心是通过XmlHttpRequest获取非本页内容，而jsonp的核心则是动态添加<script>标签来调用服务器提供的js脚本。

   ### 4.CORS

   （Cross-origin resource sharing）

   Jsonp因为只能用GET请求，所以使用范围小，这个是使用最广泛的。

   浏览器请求体中加上跨域请求的字段。

   简单请求：在头信息之中，增加一个`Origin`字段，声明来源。eg：`Origin: http://api.bob.com`

   非简单请求：浏览器会先发送预检请求，"预检"请求用的请求方法是`OPTIONS`，获得回应后方可请求。

   [详细说明请看阮一峰日志](http://www.ruanyifeng.com/blog/2016/04/cors.html)

   ​