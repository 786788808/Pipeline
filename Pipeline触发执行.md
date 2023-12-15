# 触发Pipeline
手动triger Pipeline是基操。近期有requirement下来，要每天触发数条pipeline，以拿到特定的数据。机械的事情，当然要留给Pipeline自己玩玩——定时触发。  
总体，可将pipeline触发条件，分为时间触发和事件触发。（主要区别是，声调不同，哈哈）。

## 一.时间触发
定时执行  
设定某一时间点，pipeline 到点就触发。
在这里会涉及到 cron 表达式，可参考[菜鸟的教程](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)，或者搜搜别人写的文章,也可以到这个网站玩玩[cron表达式体验站](https://crontab.guru/)。    
![image](https://github.com/786788808/Pipeline/assets/32427537/45c7944f-7a8d-45f7-8d81-aa0663bc0298)


pipeline 里用到的 cron 语法，跟 unix cron 的语法有轻微区别，但大差不差。  
比如，我想要 pipeline A 在 12 月的 14,15,16 号的 23:30 开始 trigger。这时候，可以在 Jenkins file 里面设置，也可以在 Jenkins Pipeline UI 设置。条条大路通罗马，任君选择。    

格式：MINUTE HOUR DOM MONTH DOW（分，时，日，月，周几）  
- MINUTE: 一小时内的分钟，取值范围为 0~59  
- HOUR: 一天内的小时，取值范围为 0~23  
- DOM: 一个月的某一天，取值范围为 1~31  
- MONTH: 月份，取值范围为 1~12   
- DOW: 星期几，取值范围为 0~7, 0 和 7 代表星期天   
  当然，需求是多种多样的，所以设置了 map 多种情况的字符：    
- *: 匹配所有值，如果是（ * * * * *），则表示每分钟都会trigger一次  
- M-N: 匹配 M 到 N 之间的值，比如想实现10-20分的时候trigger，那就在 `MINUTE` 写 10-20  
- M-N/X or */X: 比如想实现5号到20号，隔2天就trigger pipeline, 那就直接在 `DOM`写 5-20/2  
- A,B,…,Z: 取多个值，比如想实现每个月 1,2,3 号 trigger，那就直接在 `DOM` 直接写 1,2,3
- H: 代表hash。假设我们设置了大量在同一时刻执行的定时任务，比如，100个在半夜零点(0 0 * * *)执行的task（极端例子）。同一时间trigger会产生负载不平衡。如果我们的定时任务没有准确到一定要在零点0分执行，直接写(H 0 * * *)，它会在零点0分到零点59分之间任何一个时间点执行。注意，H在`DOM`的实现可能会不准确（2月有28天，12月有31日）
- 番外篇：@yearly @annually @monthly @weekly @daily @midnight @hourly，看名字就知道很好用，随便挑
![image](https://github.com/786788808/Pipeline/assets/32427537/e7ae5a80-303f-4ab3-88dc-ebe36bdd06d2)

  
## 一.事件触发
