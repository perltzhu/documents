 对于递归调用，我们都知道，其调用需要使用栈空间来保存每一次函数调用的现场值，以便函数从里面一层回调到外面一层的时候，还能继续外层的运行。因而，递归的次数越多，所要耗费的栈空间就越多。众所周知，每个浏览器的栈空间是有限的，当面临着很深层次的递归的时候，我们就可能面临着这样一个问题，栈空间不足导致内存的溢出。所以我们必须想办法解决这种问题。
 
对于使用递归实现的算法，我们考虑使用迭代去完成。所谓迭代就是使用反复的循环来模拟递归调用。这种方法可以从算法书中获得。

第二种办法就是适当的设置缓存来存储每次调用的结果来减少递归的次数。其实这种情况至合适用多重递归来求值的特殊情况。比如有以下的一个阶乘函数：
     
	function factorial(n) {
       if (n == 0) {
              return 1;
       } else {
              return n * factorial(n-1);
       }
	}

我们进行一般的调用，如下：诠释右边是调用函数的次数

     var fact5 = factorial(5);//       5次

     var fact6 = factorial(6);//       6次

     var fact7 = factorial(7);//       7次

以上总共调用函数为18次。


使用缓存来优化阶乘函数：

 	function memfactorial(n) {

       if (!memfactorial.cache) {

              mefactorial.cache = {

                     "0" : 1,

                     "1" : 1

              };

       }

       if ( !memfactorial.cache.hasOwnProperty(n) ) {

              memfactorial.cache[n] = n * memfactorial (n-1);

       }  

       return memfactorial.cache[n];  
	}

     var fact5 = memfactorial(5);//       4次

     var fact6 = memfactorial(6);//       1次

     var fact7 = memfactorial(7);//       1次


一上函数总共调用了6次。可想速度加快了多少。随着调用的次数越多，加快的速度越大。

接下来我们可以书写一个专门为某个需要递归函数创建缓存的工厂函数，其如下：


	//返回一个设置了缓存的函数

	function memorize(fundamental, cache) {

       cache = cache || {};

       var shell = function (arg) {

              if (!cache.hasOwnProperty(arg)) {

                     cache[arg] = fundamental(arg);

              }

              return cache[arg];

       };

       return shell;

	}

	// 为阶乘生成缓存
	var memfactorial = memorize(factorial, {"0" : 1, "1" : 1});
	var fact5 = memfactorial(5);// 4 次
	var fact6 = memfactorial(6);// 1 次
	var fact7 = memfactorial(7);// 1 次