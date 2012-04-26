---
layout: page
title: "Meng Bo's Blog"
---
{% include JB/setup %}

这个博客整理了我的一些随笔、脚本，还有一些乱七八糟的东西，总之这就是一个大杂烩。


## My Projects

* [mengbo.github.com](https://github.com/mengbo/mengbo.github.com)：就是这个博客本身，没有别的东西。
* [install](https://github.com/mengbo/install)：Linux系统安装过程中用到的一些脚本。
* [telnetdevice](https://github.com/mengbo/telnetdevice)：用Ruby写的针对CISCO/Ruijie/H3C网络设备的telnet工具。


## My Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


