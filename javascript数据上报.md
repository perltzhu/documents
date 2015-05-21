# js-data-report

项目开发完成外发后，没有一个监控系统，我们很难了解到发布出去的代码在用户机器上执行是否正确，所以需要建立前端代码性能相关的监控系统。

所以我们需要做以下的一些模块：

##一、收集脚本执行错误
```
function error(msg,url,line){
   var REPORT_URL = "xxxx/cgi"; // 收集上报数据的信息
   var m =[msg, url, line, navigator.userAgent, +new Date];// 收集错误信息，发生错误的脚本文件网络地址，用户代理信息，时间
   var url = REPORT_URL + m.join('||');// 组装错误上报信息内容URL
   var img = new Image;
   img.onload = img.onerror = function(){
      img = null;
   };
   img.src = url;// 发送数据到后台cgi
}
// 监听错误上报
window.onerror = function(msg,url,line){
   error(msg,url,line);
}
```
通过管理后台系统，我们可以看到页面上每次错误的信息，通过这些信息我们可以很快定位并且解决问题。


##二、收集html5 performance信息

performance 包含页面加载到执行完成的整个生命周期中不同执行步骤的执行时间。performance相关文章点击如下：使用performance API 监测页面性能

计算不同步骤时间相对于navigationStart的时间差，可以通过相应后台CGI收集。

```
function _performance(){
   var REPORT_URL = "xxxx/cgi?perf=";
   var perf = (window.webkitPerformance ? window.webkitPerformance : window.msPerformance ),
      points = ['navigationStart','unloadEventStart','unloadEventEnd','redirectStart','redirectEnd','fetchStart','domainLookupStart','connectStart','requestStart','responseStart','responseEnd','domLoading','domInteractive','domContentLoadedEventEnd','domComplete','loadEventStart','loadEventEnd'];
   var timing = pref.timing;
   perf = perf  ? perf : window.performance;
   if( perf  && timing ) {
      var arr = [];
      var navigationStart = timing[points[0]];
      for(var i=0,l=points.length;i<l;i++){
         arr[i] = timing[points[i]] - navigationStart;
      }
   var url = REPORT_URL + arr.join("-");
   var img = new Image;
   img.onload = img.onerror = function(){
      img=null;
   }
   img.src = url;
}
```
通过后台接口收集和统计，我们可以对页面执行性能有很详细的了解。

##三、统计每个页面的JS和CSS加载时间

在JS或者CSS加载之前打上时间戳，加载之后打上时间戳，并且将数据上报到后台。加载时间反映了页面白屏，可操作的等待时间。
```
<script>var cssLoadStart = +new Date</script>
<link rel="stylesheet" href="xxx.css" type="text/css" media="all">
<link rel="stylesheet" href="xxx1.css" type="text/css" media="all">
<link rel="stylesheet" href="xxx2.css" type="text/css" media="all">
<sript>
   var cssLoadTime = (+new Date) - cssLoadStart;
   var jsLoadStart = +new Date;
</script>
<script type="text/javascript" src="xx1.js"></script>
<script type="text/javascript" src="xx2.js"></script>
<script type="text/javascript" src="xx3.js"></script>
<script>
   var jsLoadTime = (+new Date) - jsLoadStart;
   var REPORT_URL = 'xxx/cgi?data='
   var img = new Image;
   img.onload = img.onerror = function(){
      img = null;
   };
   img.src = REPORT_URL + cssLoadTime + '-' + jsLoadTime;
</script>
```
##参考资料：
- [html5 performance en](https://dvcs.w3.org/hg/webperf/raw-file/tip/specs/NavigationTiming/Overview.html)
- [html5 performance cn](http://www.alloyteam.com/2012/11/performance-api-monitoring-page-performance/)
- [javascript onerror api](http://www.w3school.com.cn/js/js_onerror.asp)