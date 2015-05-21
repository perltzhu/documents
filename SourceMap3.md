# sourcemap
## 背景
source map 提案的作用在于可以在浏览器中的开发者工具Closure Inspector中像调试源代码一样地调试生成后的Javascript代码。

source map是一个记录代码转换前和转换后的位置信息文件，利用Closure Compiler生成。

source map 经历了3个版本，第三版本的source map代码量大量减少。

## 术语
生成后的代码： 指的是那些被编译器压缩过后的代码

源代码：指的是没有被编译器压缩过的代码

VLQ：变量长度量，一个base64值。相应转换规则请点击链接

Source Mapping URL：生成后的代码的source map文件URL地址

##版本3格式
###特点
减少代码量，减少了解析时间，减少内存消耗，减少下载时间

利用双向映射，支持源代码级的调试

支持反混淆

###格式
1. {
2.    “version”:3,
3.    “file”: “target.js”,
4.    “sourceRoot”: “”,
5.    “sources”:["map-test.js","map-test2.js"],
6.    “sourceContent”: [null,null],
7.    “names”:["a","b","c","d","global","global2","ss","aa","cc","dd","globa11l","globa2l2"],
8.    “mappings”:”AAAAA,QAASA,EAAC,EAAE,EAEZC,QAASA,EAAC,EAAE,EAEZC,QAASA,EAAC,EAAE,EAOZC,QAASA,EAAC,EAAE,EAUZ,IAAIC,OAAO,CAAX,CACIC,QAAQ,C,CCtBZC,QAASA,GAAE,EAAE,EAEbC,QAASA,GAAE,EAAE,EAEbC,QAASA,GAAE,EAAE,EAObC,QAASA,GAAE,EAAE,EAUb,IAAIC,SAAS,CAAb,CACIC,SAAS;”
9. }

target.js,map-test.js,map-test2.js,target.js.map

demo文件，具体实际效果可下载到本地调试

- 第一行:source map的JSON对象
- 第二行: version:文件版本，必须是个正整数
- 第三行: file:编译器生成后的文件
- 第四行: sourceRoot:源文件相对路径。如果都在当前路径上，则为空
- 第五行: sources:源文件列表
- 第六行: sourceContent:源文件配置，当没有配置source的时候则使用这个
- 第七行: names:变量列表
- 第八行: mappings:映射表

mappings数据结构如下:

- 生成后的文件，每一行的映射片段块都会以分号(;)分隔开来
- 每个片段以逗号(,)分隔
- 每个片段是有1,4或5个变量长度的字段组成
- 片段代表着每个位置的映射，其中片段的每一个字段的含义:
  - 第一个值，代表这个位置开始于编译后文件的第几列
  - 第二个值，代表这个位置属于sources中的哪个文件
  - 第三个值，表示这个位置开始于源代码的第几行
  - 第四个值，代表这个位置开始于源代码的第几列
  - 第五个值，代表这个位置在于names中的哪个变量

##编码类型
默认的编码类型为UTF-8

##压缩
source map文件允许GZIP压缩

##拓展
额外的字段可以被添加到source map，用来提供拓展的功能，通常它们以”x_”开头。不过建议的命名规范是以域名为标准，举个例子, x_com_google_gwt_linecount

##已知拓展
“x_google_linecount”- source map 行的数量

##约定
###Source Map 命名
通常来说，一个source map文件都会以源文件名作为文件吗，以”.map”为拓展文件类型。举个例子，对于”page.js”源文件，source map 以”page.js.map”命名

链接生成后的文件和源文件

在生成后的文件的结尾加入如下代码，可以使生成后的文件和源文件进行链接:

`//@ sourceMappingURL=target.js.map`

例子: http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js

JSON 通过HTTP 传输

通过重写Array构造器，直接利用script src引用一个source map，可以造成XSSI攻击。而服务器可以通过在响应内容前面添加”)]}”字符串可以有效的阻止XSSI攻击。
对于加载sourcemap文件，每次需要进行一次替换再转换为JSON对象，例子：
JSON.parse(aSourceMap.replace(/^)]}’/, ”))

##source map使用
编译命令
`java -jar /tools/clourse-compiler/compiler.jar –js map-test.js map-test2.js –create_source_map ./target.js.map –source_map_format=V3 –js_output_file target.js`

##启用
Chrome开发者工具中勾选 enable javascript source maps开启。另外，如果想要生成后文件不支持source map, 请去掉生成后的代码的sourceMappingURL注释

#参考文档
- [Source Map Revision 3 Proposal](https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/)
- [Closure Compiler](https://code.google.com/p/closure-compiler/)
- [Source Map](https://github.com/mozilla/source-map)
- [JavaScript Source Map 详解](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)
- [Introduction to JavaScript Source Maps](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)
- [website-security-for-webmasters](http://googleonlinesecurity.blogspot.com/2011/05/website-security-for-webmasters.html)