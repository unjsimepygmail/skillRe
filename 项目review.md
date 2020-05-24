1.如何使用redis：传感器写库之前，存入redis，接口直接去查redis。
2.使用kafka干啥?通过socket长连接获取的传感器数据，直接丢入kafka中，随后监听sensor topic 设置消费者组id，一次拉取50条处理
3.项目中如何使用token校验权限: 实现拦截器接口，实现preHandle postHandle 函数，实现WebMvcConfigurer 接口，实现addInterceptors函数，进行拦截器的配置，（指定拦截器，指定拦截的url）使用jwt生成token 用秘钥解密验证token

4.akka框架  提供了一种称为Actor的并发模型，其粒度比线程更小，你可以在系统中启用大量的Actor
数据清洗怎么做的?通过字符缓存流读取一行一行的文本，判断文本是否符合json格式，不符合则直接读取下一行。

5.
