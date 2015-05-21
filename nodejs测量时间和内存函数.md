# 【nodejs】测量时间和内存函数

本篇文章用来介绍nodejs中，用来测量时间和内存的函数

## process.memoryUsage ##

返回描述以字节为单位的节点进程的内存使用量的对象。相应执行代码例子如下：

    var util = require('util');
    
    console.log(util.inspect(process.memoryUsage()));
    
相应执行结果如下：

    { rss: 11259904, heapTotal: 5066496, heapUsed: 2049924 }

截图如下：


heapTotal和heapUsed参考V8的内存使用情况。rss为驻留集大小，相应说明可以参考[驻留集](http://baike.baidu.com/view/3319068.htm "驻留集")

相应api链接为[http://nodejs.org/docs/v0.4.10/api/process.html#process.memoryUsage](http://nodejs.org/docs/v0.4.10/api/process.html#process.memoryUsage "process.memoryUsage")



## process.hrtime ##

返回当前高分辨率实时在[秒，纳秒]元组。它是相对于过去任意的时间。它和一天中的时间不相关，因此，不会受到[时钟偏移](http://zh.wikipedia.org/wiki/%E6%97%B6%E9%92%9F%E5%81%8F%E7%A7%BB "时钟偏移")影响。主要用途是用于测量时间间隔之间的性能。

相应执行代码例子如下：
    
    var time = process.hrtime();
    // [ 1800216, 25 ]
    
    setTimeout(function() {
      var diff = process.hrtime(time);
      // [ 1, 552 ]
    
      console.log('benchmark took %d nanoseconds', diff[0] * 1e9 + diff[1]);
      // benchmark took 1000000527 nanoseconds
    }, 1000);

相应执行结果如下：

    { _idleTimeout: 1000,
      _idlePrev:{ 
		_idleNext: [Circular],
        _idlePrev: [Circular],
        msecs: 1000,
        ontimeout: [Function: listOnTimeout] 
	  },
      _idleNext:{
		_idleNext: [Circular],
        _idlePrev: [Circular],
	    msecs: 1000,
	    ontimeout: [Function: listOnTimeout] 
	  },
      _idleStart: 1432193297110,
      _onTimeout: [Function],
      _repeat: false 
    }
    benchmark took 2598631510 nanoseconds

截图如下：

相应api链接为[https://nodejs.org/api/process.html#process_process_hrtime](https://nodejs.org/api/process.html#process_process_hrtime "process.hrtime")