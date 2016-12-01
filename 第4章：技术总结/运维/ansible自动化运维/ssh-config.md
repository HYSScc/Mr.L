使用SSH CONFIG: [https://www.lainme.com/doku.php/blog/2011/02/%E4%BD%BF%E7%94%A8ssh\_config](https://www.lainme.com/doku.php/blog/2011/02/%E4%BD%BF%E7%94%A8ssh_config)

### ssh的配置文件 {#ssh的配置文件}

ssh client有两个配置文件，`/etc/ssh/ssh_config和~/.ssh/config，前者是对所有用户，后者是针对某个用户，两个文件的格式是一样的。`

## 配置文件格式 {#配置文件}

用户配置文件在`~/.ssh/config，没有的话新建一个。基本的写法是`

```
Host 名称(自己决定，方便输入记忆的)
    HostName 主机名
    User 登录的用户名
```



