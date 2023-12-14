# 触发Pipeline
手动triger Pipeline是基操。近期有requirement下来，要每天触发数条pipeline，以拿到特定的数据。机械的事情，当然要留给Pipeline自己玩玩——定时触发。  
总体，可将pipeline触发条件，分为时间触发和事件触发。（主要区别是，声调不同，哈哈）。

## 一.时间触发
定时执行
设定某一时间点，到点就触发。比如，我想要pipeline A 在12月的14,15,16号的23:30开始trigger。  
可以在Jenkins file里面设置，也可以在Jenkins Pipeline UI 设置。条条大路通罗马，任君选择。    
格式：MINUTE HOUR DOM MONTH DOW
- MINUTE:一小时内的分钟，取值范围为0~59  
- HOUR: 一天内的小时，取值范围为0~23  
- DOM: 一个月的某一天，取值范围为1~31  
- MONTH: 月份，取值范围为1~12  
- DOW: 星期几，取值范围为0~7,0和7代表星期天  
## 一.事件触发
