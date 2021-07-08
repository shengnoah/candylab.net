---
#
# Use the widgets beneath and the content will be
# inserted automagically in the webpage. To make
# this work, you have to use › layout: frontpage
#

layout: frontpage

tags: 糖果的实验室,墨守之道,WEB服务安全架构与实践,宁芝,niz

header:
  image_fullwidth: header_unsplash_12_1.jpg
  
widget1:
  title: "TestFreebuf专栏"
  url: 'http://zhuanlan.freebuf.com/column/index/?name=%E7%B3%96%E6%9E%9C%E5%AE%9E%E9%AA%8C%E5%AE%A4'
  text: '<a href="/design/opencanary/">基于开源蜜罐的实践与功能扩展</a>.<br/>'
  image: 'widget-freebuf-302x182.jpg'

widget2:
  title: "Test小专栏"
  url: 'https://xiaozhuanlan.com/candylab'
  image: widget-point-line-302x182.jpg  
  text: '糖果实验室小专栏'

widget3:
  title: "TestSOC日志收集实践"
  url: 'http://www.freebuf.com/articles/es/174281.html'
  image: widget-mailproxy-302x182.jpg
  text: '企业邮件服务日志收集'


widget4:
  title: "道高一尺"
  url: '/hobby/elec-guitar//hobby/elec-guitar/'
  text: '<a href="/design/orange-gateway/">为什么要介绍Orange网关，他有那些特色的地方，值得我们去借鉴和学习？</a><br/>'
  video: '<a href="#" data-reveal-id="videoModal"><img src="/images/thumb/orange_thumb.png" width="302" height="182" alt=""/></a>'
   
widget5:
  title: "太公钓鱼"
  url: '/design/opencanary/'
  text: '<a href="/design/opencanary/">蜜罐是一种伪装技术，也是防御手段。太公钓鱼，愿者上勾。</a><br/>'
  image: /thumb/fish_thumb.png  


widget6:
  title: "众擎易举"
  url: '/design/auto-scan-code-review/'
  text: '<a href="/design/auto-scan-code-review/">海量的数据日志，传统的简单说法不适用，神经网络方法来分析。</a><br/>'
  image: /thumb/fish_thumb.png  

  
widget7:
  title: "ClickHouse与威胁日志分析"
  url: 'http://www.freebuf.com/column/164671.html'
  image: widget-clickhouse-302x182.jpg
  text: '基于Clickhouse做威胁数据聚合分析。'

widget8:
  title: "Redis流量监听"
  url: 'http://www.freebuf.com/column/164671.html'
  image: widget-redis-monitor-302x182.jpg
  text: '用Lua处理Redis的端口监听数据。'

widget9:
  title: "开源SOC的设计与实践"
  url: 'http://www.freebuf.com/articles/network/173282.html'
  image: widget-opensoc-302x182.jpg
  text: '如何用一些开源产品实现一个SOC。'


widget10:
  title: "数据库"
  url: '/design/clickhouse-reader-writer/'
#  image: 'frontpage/suricata.png' 
  image: 'frontpage/clickhouse.png' 
  text: '实时日志查询分析处理'


widget11:
  title: "SIEM威胁事件"
  url: '/design/graylog/'
  image: 'frontpage/graylog.png' 
  text: '安全威胁事件管理'


widget12:
  title: "BI商业智能"
  url: '/design/superset/'
  image: 'frontpage/superset.png'
  text: '商业分析统计与数据可视化'


top1:
  title: "WEB防火墙工作原理"
  url: '/design/WAF原理/'
  text: '<a href="/design/WAF原理/">基于OpenResty服务与Lua扩展模块创建WEB防护业务的工作原理。</a><br/>'
  image: widget-mailproxy-302x182.jpg

name1:
  title: "--新浪微博 COO，新浪移动 CEO 王巍"
  url: '/design/superset/'
  image: 'frontpage/superset.png'
  text: '近年来，国家对网络信息安全高度重视，信息化社会的建设离不开网络安全技术的保障，企业的生产运营也需要依靠网络信息安全技术。这一切推动了计算机安全技术的快速发展和信息安全建设的创新实践。例如，大数据信息平台建设、基于人工智能的网络攻击分析、自动化与智能化的信息安全等，让网络信息安全建设迈上了新的台阶。本书从网络攻击与防御两种角度概括了网络信息安全建设中的技术实践与思考，让读者从多个视角了解网络信息安全建设中的攻守之道。'


name2:
  title: "--OpenResty 创始人兼 CEO 章亦春"
  url: '/design/superset/'
  image: 'frontpage/superset.png'
  text: '多年来，我们致力于用技术和创新助力 OpenResty 社区发展，并为遍布世界各地的 OpenResty 用户提供服务。随着 OpenResty 社区的壮大，OpenResty 推出了更多的革命性产品——OpenResty Edge、OpenResty XRay、YLang 语言和 YSQL 语言。本书介绍了这些 OpenResty 产品在安全领域的应用，为大家开启了一扇通往 OpenResty 技术世界的大门。希望这本书能让读者感受到 OpenResty 新技术为安全领域注入的一股强大力量。'

name3:
  title: "--新浪信息安全总监 康宇"
  url: '/design/superset/'
  image: 'frontpage/superset.png'
  text: '当前，网络攻击对抗已经日趋常态化，攻防双方压力同在。作为网络安全行业的从业者，我们要不断地更新自己的知识库，深入了解资产防护的手段和渗透攻击的方式，不仅要掌握经典的安全攻防原理，而且要了解新兴的技术理念。在网络攻防中，往往是奇兵制胜，以“魔法”战胜“魔法”，本书既有对常见攻击场景的原理回顾，又有对新防御手段的解析，能够帮助读者在网络攻防中做到胸有成竹、出奇制胜。'



name4:
  title: "--Orange 网关创始人 吴兆玉"
  url: '/design/superset/'
  image: 'frontpage/superset.png'
  text: 'Orange 是一个基于 OpenResty 的 API 网关，除 Nginx 的常规功能外，它还可用于 API 监控、访问控制（鉴权、WAF）、流量筛选、访问限速、A/B 测试、静态 / 动态分流等场景。Orange 凭借其灵活的插件机制提供了强大的功能扩展性，用户可基于其提供的分层和钩子机制定制丰富的流量处理算子，同时，基于Lor框架的sinatra风格API 也为可交互性提供了极大便利。这本书详细介绍了Orange网关技术在网络安全场景的应用，值得网络安全技术人员学习研读。'
#
# Use the call for action to show a button on the frontpage
#
# To make internal links, just use a permalink like this
# url: /getting-started/
#
# To style the button in different colors, use no value
# to use the main color or success, alert or secondary.
# To change colors see sass/_01_settings_colors.scss
#
callforaction:
  url: / 
  text: 实施 ›
  style: alert
permalink: /index.html
#
# This is a nasty hack to make the navigation highlight
# this page as active in the topbar navigation
#

view:
  url: / 
  text: 观点 ›
  style: alert
permalink: /index.html

archi:
  url: / 
  text: 设计 ›
  style: alert
permalink: /index.html


hobbything:
  url: http://lab.orchina.org/hobby/ 
  text: 好玩好物 ›
  style: alert
permalink: /index.html


homepage: true
---




<div id="videoModal" class="reveal-modal large" data-reveal="">

  <div class="flex-video widescreen vimeo" >
  <video id="marioid" src="" controls="controls" onmouseover="this.play()" onmouseout="this.pause()" autobuffer="true">您的浏览器不支持 video 标签。</video>



  </div>

  <a class="close-reveal-modal">&#215;</a>
</div>


<script src="/assets/js/cdn/jquery.min.js"></script>
<script>

$(".close-reveal-modal").on('click',function(){
    console.log('strat');
    var vid = document.getElementById("marioid");
    console.log(vid)
    vid.pause();
    vid.onpause = function() {
        console.log("pause");
    };
    console.log('end');
//    event.stopPropagation();
});

</script>



<meta name="keywords" content="糖果实验室,安全博客,freebuf">





