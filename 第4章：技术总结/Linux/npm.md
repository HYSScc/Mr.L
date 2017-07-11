国内优秀npm镜像推荐及使用:[http://blog.csdn.net/cengjingcanghai123/article/details/45045265](http://blog.csdn.net/cengjingcanghai123/article/details/45045265)



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

npm--registry[https://registry.npm.taobao.org](https://registry.npm.taobao.org) install express

**2.持久使用**

npm configsetregistry [https://registry.npm.taobao.org](https://registry.npm.taobao.org)

// 配置后可通过下面方式来验证是否成功

npm configgetregistry

// 或

npm info express

**3.通过cnpm使用**

npminstall-g cnpm--registry=[https://registry.npm.taobao.org](https://registry.npm.taobao.org)

// 使用

cnpminstallexpresstall express

