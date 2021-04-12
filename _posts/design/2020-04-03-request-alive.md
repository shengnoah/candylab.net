---
layout: page-fullwidth
title: "Keepalive Requests"
subheadline: "WEB服务运维服务与管理"
teaser: "新版本的Nginx中长连接请求数设定配置默认值变更了，默认值变为了100.升级到了1.15x的时候，如果只升级了系统没有改配置，某些情况下会出现问题。"
categories:
    - design
# Styling
#
header:
   image_fullwidth: "header_roadmap_2.jpg"
image:
    thumb: icons/design.png   
    homepage: slider/mariohead.jpg 
    caption: 糖果
    caption_url: http://candylab.net
mediaplayer: true

---
<!--more-->


<div class="row">
<div class="medium-4 medium-push-8 columns" markdown="1">
<div class="panel radius" markdown="1">
**目录**
{: #toc }
*  TOC
{:toc}
</div>
{% include _sidebar_tags.html %}
</div><!-- /.medium-4.columns -->


<div class="medium-8 medium-pull-4 columns" markdown="1">

{% include _improve_content.html %}


# 背景

T婶早上同步了一个消息，  Nginx代理和Upstream服务器之间在某种情况下一直发connection:close。Nginx从1.13.6升级到了1.15.8出现的问题，T婶牺牲了午休的时间堵上的这个坑， 其根本原因，是升级到1.15.8之后，Nginx的长链接Keepalive_Requests的默认值变成了：100。 过个这个极值就会出现这种问题。T婶子每次都自带抓包分析。

如果线上的那位老师的nginx升级到了这个版本，还有配置长连接的情况可以注意一下了。最近 OpenResty官方终于推出了自己的中文论坛， 以前有过各种论坛，后来都不温不火，希望官方的这事可以大家常去去。而且听说OPM也要有动作。


# 配置

```conf
upstream my_upstream {
    server 47.243.56.222:80;
    keepalive 2048;
}

server {
    listen 127.0.0.1:80 backlog=10240;

    location /{
        proxy_http_version 1.1;
        proxy_set_header X-Forwarded-For $http_x_forwarded_for;
        proxy_pass http://my_upstream;
    }

    upstream服务配置如下：
    http {
        keepalive_timeout 300s;
        keepalive_requests 1000;
        default_type text/plain;

        server {
            listen       80 backlog=102400;
            server_name  localhost;
            location / {
                root "/usr/local/openresty/nginx/data";
        }
    }
}

```

# 参考

最后给出官方中文论坛对这个问题的回复， 德江老师有回复。


https://forum.openresty.us/d/6365-openresty-11582-keepalive-requests
