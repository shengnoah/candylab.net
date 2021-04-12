---
#
# Use the widgets beneath and the content will be
# inserted automagically in the webpage. To make
# this work, you have to use › layout: frontpage
#

layout: frontpage

tags: 糖果实验室,开源,安全系统,机械键盘，吉他谱，电脑外设，静电容键盘,niz，光阴里的故事，orchina.org

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
  title: "吉他"
  url: '/hobby/elec-guitar//hobby/elec-guitar/'
  text: '<a href="/hobby/elec-guitar/">吉他是一种常见的乐器，便携方便，学习成本底，如果你喜欢音乐但又不以此为生，学学吉他也是不错的选择。</a><br/>'
  video: '<a href="#" data-reveal-id="videoModal"><img src="/images/widget-freebuf-302x182.jpg" width="302" height="182" alt=""/></a>'
   
widget5:
  title: "蜜罐"
  url: '/design/opencanary/'
  text: '<a href="/design/opencanary/">蜜罐系统本质是一种伪装系统,通过模拟真实服务的技术被动取得攻击信息。蜜罐分为主动蜜罐与被动蜜罐，应用的场景也不一样。</a><br/>'
  image: widget-point-line-302x182.jpg  


widget6:
  title: "扫描"
  url: '/design/auto-scan-code-review/'
  text: '<a href="/design/auto-scan-code-review/">开源世界有很多种的扫描器，而我们需要一种标准规范使用这些扫描器，通过统一调度形式，便利的驱动这些扫描工具。</a><br/>'
  image: widget-mailproxy-302x182.jpg
  
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





