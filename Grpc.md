# Grpc

#### 超时处理——链路指数扩大效应

解决：对重试请求不重试。

对于重试的请求，在 Request 中打上一个特殊的 retry flag，在 A -> B -> C 的链路：

- B 收到 A 的请求，先读取这个 flag 判断是不是重试请求
  - true：调用 C，即使失败也不会重试
  - false：调用 C，失败后会重试 C

| **策略**         | **说明**                                           |
| ---------------- | -------------------------------------------------- |
| 重试熔断         | 请求失败 / 成功 > 0.1 时停止重试                   |
| 链路上传错误标志 | 下层重试失败后上传错误标志，上层不再重试           |
| 链路下传重试标志 | 重试请求特殊标记，下层对重试请求不会重试           |
| DDL              | 当剩余时间不够时不再发起重试请求                   |
| 框架熔断         | 微服务框架本身熔断、过载保护等机制也会影响重试效果 |