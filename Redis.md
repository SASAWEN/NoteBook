# Redis



- redis 是单线程采用的是I/O多路复用线程模型。
- 通过指令队列和响应队列可以让redis指令有序不乱的执行。

### 缓存雪崩

缓存机器宕机/缓存扛不住qps

解决方案：

- 降级：限流 -> 直接访问DB or 返回查询失败

- 熔断：

  

### 缓存穿透

缓存命中率过低，访问DB请求量激增

解决方案：

- 每次系统 A 从数据库中只要没查到，就写一个空值到缓存里去



### 缓存击穿

热点key过期时，访问DB请求量激增

解决方案：

- 将热点数据设置为永远不过期
- 基于 redis or zookeeper 实现互斥锁，等待第一个请求构建完缓存之后，再释放锁，进而其它请求才能通过该 key 访问数据

