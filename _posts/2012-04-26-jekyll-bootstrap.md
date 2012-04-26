---
layout: post
title: "利用Jekyll Bootstrap和github来构建Blog"
category: tech
tags: [jekyll, bootstrap, github]
---
{% include JB/setup %}

首先得在github上建立一个Repo，名字采用`USERNAME.github.com`的格式`USERNAME`就是用户名，这样通过`http://USERNAME.github.com`就可以访问了。至于内容如何处理就不多废话，让代码说话。

把jekyll-bootstrap克隆出来：

{% highlight bash %}
    git clone https://github.com/plusjade/jekyll-bootstrap.git mengbo.github.com
    echo "rvm jekyll" > mengbo.github.com/.rvmrc
    cd mengbo.github.com
    git remote set-url origin git@github.com:mengbo/mengbo.github.com.git
    git push origin master
{% endhighlight %}
    

更改theme：

{% highlight bash %}
    rake theme:install git="git://github.com/jekyllbootstrap/theme-the-minimum.git"
    git add .
    git commit -m "install theme-the-minimum"
    git push origin master
{% endhighlight %}
    

删除没有用的例子：

{% highlight bash %}
    rm -rf _posts/core-samples
    git rm -f _posts/core-samples
    git commit -m "rm -f _posts/core-samples"
    git push origin master
{% endhighlight %}

然后修改_config.yml，push到github上就成了。

注意要把上面的mengbo换成你的用户名。
