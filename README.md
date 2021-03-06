# 商品秒杀演示
  
  抢购、秒杀是如今很常见的一个应用场景，主要需要解决的问题有两个：  
  1 高并发对数据库、磁盘IO产生的压力  
  2 竞争状态下如何解决库存的正确减少（"超卖"问题）
  
对于第一个问题，已经很容易想到用内存数据库来处理抢购，避免直接操作数据库，例如使用Redis。
重点在于第二个问题
  
  常规写法：

查询出对应商品的库存，看是否大于0，然后执行生成订单等操作，但是在判断库存是否大于0处，如果在高并发下就会有问题，导致库存量出现负数。
  
####  解决方案：
由于redis是单进程单线程的，对于写操作已经天然具有隔离性，即串行执行，所以不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗。网络上很多用锁事务之类的解决库存超卖就是不明白redis是单进程单线程这一点。当然也有将库存弄成队列，这样也行，但是不如decr直接，而且库存很大的话，占用内存也是个问题。本程序是decr操作库存的演示。其他的配置简单说一下：  


redis是默认配置。
  
网站请求参数：c是控制器，a是方法  
/index.php?c=Demo&a=index  
命令行执行脚本  
php ./index.php

