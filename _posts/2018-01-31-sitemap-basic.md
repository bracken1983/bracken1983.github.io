---
layout:     post
title:      Sitemap究竟是个什么鬼
subtitle:
date:       2018-01-31
author:     brackenbo
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - sitemap
    - seo
---

[find here](https://en.wikipedia.org/wiki/Site_map)

sitemap 就是一系列的页面和网站的。

* 网站设计者设计出的网站
* 网站上的网页的列表
* 为搜索引擎准备的供爬虫来处理的列表


## XML sitemap

Google SiteMap Protocol是Google自己推出的一种站点地图协议，此协议文件基于早期的robots.txt文件协议，并有所升级。在Google官方指南中指出加入了Google SiteMap文件的网站将更有利于Google网页爬行机器人的爬行索引，这样将提高索引网站内容的效率和准确度。
    
    <?xml version="1.0" encoding="UTF-8"?>
    <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
      <url>
        <loc>http://www.example.net/?id=who</loc>
        <lastmod>2009-09-22</lastmod>
        <changefreq>monthly</changefreq>
        <priority>0.8</priority>
      </url>
      <url>
        <loc>http://www.example.net/?id=what</loc>
        <lastmod>2009-09-22</lastmod>
        <changefreq>monthly</changefreq>
        <priority>0.5</priority>
      </url>
      <url>
        <loc>http://www.example.net/?id=how</loc>
        <lastmod>2009-09-22</lastmod>
        <changefreq>monthly</changefreq>
        <priority>0.5</priority>
      </url>
    </urlset>

