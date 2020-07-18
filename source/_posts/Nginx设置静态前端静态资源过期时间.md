---
title: Nginx设置静态前端静态资源过期时间
date: 2020-07-08 18:36:15
tags:
- Nginx缓存
categories:
- 中间件
---
# Nginx设置静态前端静态资源过期时间

## 问题发现

博主使用Hexo搭建博客，前端文件渲染后，使用手机进行测试，发现手机微信查看网站依然是之前的页面，很快就联想到这是浏览器缓存造成的问题，由于博主使用了Nginx，那么修改Nginx配置文件实现静态文件过期设置。

## 配置

~~~shell
location ~.*\.(js|css|html|png|jpg)$
{
    expires    3d;
}
~~~

> 只需要在nginx反向代理中设置`expires`即可。

一些配置值如下：

* expires  3d;　　//表示缓存3天

* expires  3h;　　//表示缓存3小时

* expires  max;　　//表示缓存10年

* expires  -1;　　//表示永远过期。

> 如果设置为-1在js、css等静态文件在没有修改的情况下返回的是http 304，如果修改返回http 200
>
> http 304：自从上次请求后，请求的网页未修改过。服务器返回此响应时，不会返回网页内容。
>
> http 200：服务器已成功处理了请求，这表示服务器提供了请求的内容。

------

如果不想让代理或浏览器缓存，加`no-cache`参数
`add_header Cache-Control no-cache;`
这样浏览器F5刷新时，返回的就是http 200，而不是http 304
