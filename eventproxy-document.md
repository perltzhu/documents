# eventproxy-doc
本篇介绍关于Node.js事件代理的一个解决方案。

项目实践中，有遇到使用nodejs解决实际问题场景。Node.js以Javascript作为语言，解决问题时，采用了EventProxy做事件代理，避免事件嵌套回调。

EventProxy做为一个组件，包含如下特点：

- 内部包含事件代理机制，能避免多重回调嵌套问题
- 符合CMD，AMD及CommonJS等其它的模块设计标准
- 包装友好的回调处理监听器，包含标准的Node.js错误处理方法
- 兼容多平台，能够被应用到Node.js和各种浏览器环境中

##一、Eventproxy 解决问题的例子
在使用Node.js解决问题的时候，经常遇到多函数嵌套回调问题，引用例子：

JavaScript
```
var render = function (template, data) {
  _.template(template, data);
};
$.get("template", function (template) {
  // something
  $.get("data", function (data) {
    // something
    $.get("l10n", function (l10n) {
      // something
      render(template, data, l10n);
    });
  });
});
```
为了避免这种较为臃肿的代码编写方式，可以考虑使用Eventproxy组件解耦调用模块，引用例子：

JavaScript

```
var ep = EventProxy.create("template", "data", "l10n", function (template, data, l10n) {
  _.template(template, data, l10n);
});
 
$.get("template", function (template) {
  // something
  ep.emit("template", template);
});
$.get("data", function (data) {
  // something
  ep.emit("data", data);
});
$.get("l10n", function (l10n) {
  // something
  ep.emit("l10n", l10n);
});
 ```
这样很好避免了回调函数嵌套使用，使得处理函数和调用函数能够声明在程序的合适位置。

##二、Eventproxy拥有的接口
- addListener 绑定事件监听器
- after 在某个事件之后在限制的次数之内执行某个监听器
- all 在所有绑定事件触发之后，执行一次监听器
- any 在某个绑定的事件触发之后，执行一次监听器
- asap 绑定事件，并且执行一次监听器
- assign 等同于 all
- assignAll 在所有绑定事件触发之后，优先执行一次监听器
- assignAlways 等同于 assignAll
- bind 等同于 addListener
- done 返回一个事件成功执行的代理函数
- doneLater 返回一个事件成功执行的异步代理函数
- emit 触发事件,并且执行所有监听器
- emitLater  异步触发事件,并且执行所有监听器
- fail 绑定一个只执行一次的错误回调函数
- fire 触发某个事件，并且执行所有监听器
- group 按绑定顺序执行某个监听器，并且将结果有序返回给用after绑定在这个事件的监听器
- immediate 等同于asap
- not 绑定时间监听器，非某个事件执行监听器
- on  等同于 addListener
- once 绑定并且返回只执行一次的事件监听器
- removeAllListeners  等同于removeListener
- removeListener 移除事件的监听器
- subscribe  等同于 addListener
- tail 等同于assignAll
- trigger 等同于emit
- unbind 等同于 removeListener
- Eventproxy.create 创建一个Eventproxy实例。如果有绑定事件，在每次事件触发后，减少调用参数，执行相应监听器。

##三、Eventproxy安装和使用
###Node用户
通过NPM安装即可使用：

JavaScript
```
$ npm install eventproxy
```
调用:

JavaScript

```
var EventProxy = require('eventproxy');
```
###普通环境
在页面中嵌入脚本即可使用：
```
<script src=”https://raw.github.com/JacksonTian/eventproxy/master/lib/eventproxy.js”></script>
```
使用：

JavaScript

```
var ep = new EventProxy();// EventProxy此时是一个全局变量
```

##参考资料：
- [https://npmjs.org/package/eventproxy](https://npmjs.org/package/eventproxy)
- [https://github.com/JacksonTian/eventproxy](https://github.com/JacksonTian/eventproxy)
- [https://github.com/JacksonTian/eventproxy/blob/master/lib/eventproxy.js](https://github.com/JacksonTian/eventproxy/blob/master/lib/eventproxy.js)
- [http://nodejs.org/api/process.html#process_process_nexttick_callback](http://nodejs.org/api/process.html#process_process_nexttick_callback)