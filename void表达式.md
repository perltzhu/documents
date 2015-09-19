# void 操作符 #
##  语法 ##
    void expression
## 描述 ##
这个操作符允许插入一个计算结果等效于undefined的表达式。
void操作符经常仅仅是用来获得undefined值，比如经常使用到的“void(0)”(这等效于“void 0”)。
## 快速调用函数表达式 ##
但使用一个快速调用函数表达式，void可以用来强制将函数关键语作为一个表达式来使用，而不是一个声明。
    
    void function iife() {
    	var bar = function () {};
    	var baz = function () {};
    	var foo = function () {
    		bar();
    		baz();	
    	};
    	var biz = function () {};
    }

## Javascript URIs ##
当浏览其遇到一个`javascript:URI`，它会运算带有URI的代码，然后将计算结果替换当前页面的内容，除非它的返回值是undefined。void操作符可以被用于返回undefined。
    
    <a href="javascript:void(0);">
      Click here to do nothing
    </a>
    
    <a href="javascript:void(document.body.style.backgroundColor='green');">
      Click here for green background
    </a>

然而，值得注意的是，相对其他方案来说，javascript:伪协议并不被推荐作为句柄使用。

## 规范 ##
Specification	Status

ECMAScript 1st Edition.	Standard

ECMAScript 5.1 (ECMA-262)
The definition of 'The void Operator' in that specification.	Standard

ECMAScript 2015 (6th Edition, ECMA-262)
The definition of 'The void Operator' in that specification.	Standard

## 浏览器兼容性 ##
### Desktop ###

Chrome (yes)

Firefox (Gecko)	(yes)

Internet Explorer (yes)

Opera (yes)

Safari (yes)

### Mobile ###

Android	(yes)

Chrome for Android (yes)

Firefox Mobile (Gecko) (yes)

IE Mobile (yes)

Opera Mobile (yes)

Safari Mobile (yes)


## 参考文献 ##

[void 运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void)

[void operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void)