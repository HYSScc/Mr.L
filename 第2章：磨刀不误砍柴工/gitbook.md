# 如何安装Gitbook插件（以`gitbook-plugin-mermaid-2为例`）

1.在终端执行命令`npm install gitbook-plugin-mermaid-2`安装Mermaid
  ![](/assets/859D69DB-D652-4821-8748-9D537AFB956D.png)
2.编译对应的“书”（以i1987为例）
  ![](/assets/1BB70990-DED6-4E9D-8BF8-665ACDA90D2E.png)
3.此时，插件`gitbook-plugin-mermaid-2`安装完成，配置book.json

```
{
    "plugins": ["mermaid-2"],
    "pluginsConfig": {
      "mermaid-2": {
         "theme": "forest" // here to change the mermaid theme
      }
    }
}
```

4.编译

![](/assets/897DFAAF-A53F-4513-B916-B989514CE551.png)

5.使用插件

```mermaid
  graph TD; 
    A-->B; 
    A-->C; 
    B-->D; 
    C-->D;
```

# 参考

1. **How to use gitbook: **[https:\/\/breezetemple.gitbooks.io\/how-to-use-gitbook\/content\/installation\/index.html](https://breezetemple.gitbooks.io/how-to-use-gitbook/content/installation/index.html)

**利用 gitbook 与 webhook 做一个展示前端项目的平台: **[http:\/\/www.jianshu.com\/p\/2a1cf8891d5f](http://www.jianshu.com/p/2a1cf8891d5f)

GitBook-下载介绍与使用说明: [http:\/\/www.mybanshu.com\/tools\/gitbook-%E4%B8%8B%E8%BD%BD%E4%BB%8B%E7%BB%8D%E4%B8%8E%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.html](http://www.mybanshu.com/tools/gitbook-%E4%B8%8B%E8%BD%BD%E4%BB%8B%E7%BB%8D%E4%B8%8E%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.html)

**使用gitbook制作属于自己的电子书: **[https:\/\/www.zybuluo.com\/yangfch3\/note\/158290](https://www.zybuluo.com/yangfch3/note/158290)

#### 插件

Gitbook 的使用和常用插件：[http:\/\/zhaoda.net\/2015\/11\/09\/gitbook-plugins\/](http://zhaoda.net/2015/11/09/gitbook-plugins/)

**指南扩展: 流程图：**https:\/\/liamcao.gitbooks.io\/markdown-guide\/content\/plugin-flow.html

**mermaid：**http:\/\/knsv.github.io\/mermaid\/\#mermaid

#### Q&A:

gitbook editor 插件如何启用: [https:\/\/segmentfault.com\/q\/1010000004937806](https://segmentfault.com/q/1010000004937806)

