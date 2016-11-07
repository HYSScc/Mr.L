#### Cron

`[root@node1 ~]# ansible-doc -s cron`

`- name: 设置管理节点生成定时任务`

`action: cron`

`backup # 如果设置，创建一个crontab备份`

`cron_file #如果指定, 使用这个文件cron.d，而不是单个用户crontab`

`day # 日应该运行的工作( 1-31, *, */2, etc )`

`hour # 小时 ( 0-23, *, */2, etc )`

`job #指明运行的命令是什么`

`minute #分钟( 0-59, *, */2, etc )`

`month # 月( 1-12, *, */2, etc )`

`name #定时任务描述`

`reboot # 任务在重启时运行，不建议使用，建议使用special_time`

`special_time # 特殊的时间范围，参数：reboot（重启时）,annually（每年）,monthly（每月）,weekly（每周）,daily（每天）,hourly（每小时）`

`state #指定状态，prsent表示添加定时任务，也是默认设置，absent表示删除定时任务`

`user # 以哪个用户的身份执行`

`weekday # 周 ( 0-6 for Sunday-Saturday, *, etc )`

