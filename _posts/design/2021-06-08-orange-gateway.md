---
layout: page-fullwidth
title: "为什么在书中介绍Orange网关？"
subheadline: "Web服务安全架构与实践"
teaser: "为什么是介绍Orange网关。"
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


FreeBuf上有老师问，为什么选用介绍Orange网关，当初我们是这么考虑的。

## 0x01 背景
国内基于OpenResty Lua的开源网关，从规模和影响力方面，Orange是一个历史标杆，当初在国产OR网关不多的时候， Orange就已经出现被使用于生产中。区别于其他网关产品，还有一些基于OpenResty Lua的实现WAF的Web防火墙项目，Orange本身使用了自己的轻量级Lua Web框架Lor，并不是直接用Lua裸写功能。所以无论从代码规模，还是系统的设计架构思路，Orange和Lor框架，都很有其特点，值得参考学习。Orange是属于开山劈地之作之一。


## 0x02 Lor框架
这本书除了介绍Lor轻量级的框架，还介绍了同是Lua Web框架的Lapis框架，也是基于OR规模更大，同时Lapis还提供一种Moonscipt的语言，这种语言是最后用于翻译成Lua，MoonScript支持面向对象OO， 功能编写效率比Lua高，因为很多场景是基于OR完成，所以向大家介绍其中典型优秀的作品。

## 0x03 作者与APISIX
Orange的作者，本身也参与Apache APISIX的前身网关的设计实现，只是大家可能不太记得那个产品。这本书也有介绍基于APISIX的安全过滤插件功能，是可以基于APISIX的一个或者一组插件，完成想要实现的防护功能。介绍的篇幅有限，当时我们在写的时候，APISIX还没有从Apache毕业，现在毕业了并且在不断的迭代，其核心引擎的性能，应该比一些云WAF的性能要高（同样基于OR），并且在某些场景比KONG也的性能高出很多，大家如果有兴趣，可以去看看这个数据。

## 0x04 7层防护
以上，都是基于OpenResty Lua实现对7层数据的操作，主要使用的是Lua语言。而我们在书的后面，引入了OpenResty Edge，OpenResty Edge对于OpenResty，简单理解相当于，Nginx Plus与Nginx的关系。OpenResty的WAF功能，区别于基于OR WAF产品，加入了一个EdgeLang，建立在机械编程技术之上的。简单理解，EdgeLang是一种DSL，可以用简单的几句语句，就可构建WAF的策略，EdgeLang像MoonScript一样，最后把EdgeLang翻译成Lua，因为经过优化，性能对比直接用Lua写的代码性能高。

## 0x05 原生语言
以上的操作，都是围绕7层数据，构建工具的应用场景。最后的章节，我们引入了更高级一些的云原生语言YLang，用于发现在主机层面，Webshell被执行时调用栈状态，基于火焰图展示和动态跟踪技术， 不过YLang还是过于复杂，门槛高，就同时提供了YSQL的实现描述，用YSQL去查询主机层面的审计数据（调用栈、生成火焰图）。这样将7层数据的审计与主机层的审计结合在一起发现威胁。

即使从7层和主机层面发现威胁，考虑到也可能不是够的。将OpenResty的访问数据与日志，生成数据样本，给出基于神经网络算法的自动化威胁分析方案，同时补充了基于正则、语义分析的威胁发现思路描述。

以上这些考虑，没有更多的在书中体现，也是您问到了，借机会给出回答。非常感谢大家的关注和提问，如果想深入交流更多细节，可以关注公众号：“糖果的实验室”，可以留言给我们，多谢了！

