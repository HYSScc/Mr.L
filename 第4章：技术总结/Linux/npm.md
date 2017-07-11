国内优秀npm镜像推荐及使用:[http://blog.csdn.net/cengjingcanghai123/article/details/45045265](http://blog.csdn.net/cengjingcanghai123/article/details/45045265)



  
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 14.0px 'Helvetica Neue'; color: \#333333; -webkit-text-stroke: \#333333}  
p.p2 {margin: 0.0px 0.0px 0.0px 0.0px; font: 20.0px 'Helvetica Neue'; color: \#333333; -webkit-text-stroke: \#333333}  
p.p3 {margin: 0.0px 0.0px 0.0px 0.0px; font: 16.0px 'Helvetica Neue'; color: \#333333; -webkit-text-stroke: \#333333}  
p.p5 {margin: 0.0px 0.0px 0.0px 0.0px; font: 13.0px Consolas; color: \#657b83; -webkit-text-stroke: \#657b83; background-color: \#f6f6f6}  
p.p6 {margin: 0.0px 0.0px 0.0px 0.0px; font: 13.0px Consolas; color: \#657b83; -webkit-text-stroke: \#657b83; background-color: \#f6f6f6; min-height: 15.0px}  
p.p7 {margin: 0.0px 0.0px 0.0px 0.0px; font: 13.0px Consolas; color: \#93a1a1; -webkit-text-stroke: \#93a1a1; background-color: \#f6f6f6}  
li.li4 {margin: 0.0px 0.0px 0.0px 0.0px; font: 14.0px 'Helvetica Neue'; color: \#008e59; -webkit-text-stroke: \#008e59}  
span.s1 {font: 13.0px Consolas; font-kerning: none; color: \#c7254e; background-color: \#f6f6f6; -webkit-text-stroke: 0px \#c7254e}  
span.s2 {font-kerning: none; background-color: \#ffffff}  
span.s3 {font-kerning: none; color: \#df3434; -webkit-text-stroke: 0px \#df3434}  
span.s4 {color: \#333333; background-color: \#ffffff; -webkit-text-stroke: 0px \#333333}  
span.s5 {font-kerning: none; color: \#333333; background-color: \#ffffff; -webkit-text-stroke: 0px \#333333}  
span.s6 {font-kerning: none; color: \#008e59; -webkit-text-stroke: 0px \#008e59}  
span.s7 {font-kerning: none; color: \#268bd2; -webkit-text-stroke: 0px \#268bd2}  
span.s8 {font-kerning: none}  
span.s9 {font-kerning: none; color: \#b58900; -webkit-text-stroke: 0px \#b58900}  
span.s10 {font-kerning: none; color: \#2aa198; -webkit-text-stroke: 0px \#2aa198}  
span.s11 {font-kerning: none; color: \#859900; -webkit-text-stroke: 0px \#859900}  
span.s12 {font-kerning: none; color: \#93a1a1; -webkit-text-stroke: 0px \#93a1a1}  
span.s13 {font: 14.9px Consolas; font-kerning: none; color: \#c7254e; background-color: \#f6f6f6; -webkit-text-stroke: 0px \#c7254e}  
span.s14 {font-kerning: none; color: \#657b83; -webkit-text-stroke: 0px \#657b83}  
ul.ul1 {list-style-type: none}  


npm全称Node Package Manager，是[**Node.js**](http://lib.csdn.net/base/nodejs)的模块依赖管理工具。由于npm的源在国外，所以国内用户使用起来各种不方便。下面整理出了一部分国内优秀的npm镜像资源，国内用户可以选择使用。

**国内优秀npm镜像**

**淘宝npm镜像**

* 搜索地址：
  [http://npm.taobao.org/](http://npm.taobao.org/)
* registry地址：
  [http://registry.npm.taobao.org/](http://registry.npm.taobao.org/)

**cnpmjs镜像**

* 搜索地址：
  [http://cnpmjs.org/](http://cnpmjs.org/)
* registry地址：
  [http://r.cnpmjs.org/](http://r.cnpmjs.org/)

**如何使用**

有很多方法来配置npm的registry地址，下面根据不同情境列出几种比较常用的方法。以淘宝npm镜像举例：

**1.临时使用**

npm--registryhttps://registry.npm.taobao.org install express

**2.持久使用**

npm configsetregistry https://registry.npm.taobao.org

  


// 配置后可通过下面方式来验证是否成功

npm configgetregistry

// 或

npm info express

**3.通过cnpm使用**

npminstall-g cnpm--registry=https://registry.npm.taobao.org

  


// 使用

cnpminstallexpresstall express

