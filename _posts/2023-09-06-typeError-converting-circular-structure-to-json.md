---
layout: post
title: 解决对axios请求返回对象进行json化时报"TypeError Converting circular structure to JSON"错误的问题
categories: json
---
发现直接对axios请求返回对象进行json化会报"TypeError: Converting circular structure to JSON"错误，而对返回对象下的data属性进行json化就没问题

如果想对循环引用的对象进行json化，解决方案可参考：

[TypeError: Converting circular structure to JSON in JS](https://bobbyhadz.com/blog/javascript-typeerror-converting-circular-structure-to-json)
