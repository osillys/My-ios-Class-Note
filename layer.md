# 1

- 子layer用KVC时做形变隐藏动画时要用 
[setValue： forKeyPath：],不能用
[setValue： forKey：]

- 如何将图片存到照片库：要将图片转换成NSData才能储存

- 定时器有二种（以上），NSTimer执行优先级低导致效率差，要使用另外一种加入到主运行循环。

- 枚举中如果有移位运算符，则可以使用 "并" 运算符 = "|"

- 将UIImage画到layer中可用CG drawinrect函数

- 获取当前时间的方法：
 - 1.先获取[NSCalend calend]
 - 2.[calend component: date:[NSDate Date]]，由获取的日历调取事件组件，获得当前时间