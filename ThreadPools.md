## 线程池相关

# 为什么要用线程池？
> + 线程频繁创建销毁占用资源
> + 方便管理线程

# 线程池构造函数解析 new ThreadPoolExecutor()
> + 核心线程池数量 corePoolSize 保留在线程池的线程数
the number of threads to keep in the pool, even if they are idle, unless {@code allowCoreThreadTimeOut} is set
> + maximumPoolSize 线程池允许的最大线程数
> + keepAliveTime 当线程数大于核心线程数时，非核心线程数最大空闲时间，超过空闲时间没有执行任务 线程将会被销毁
> + unit 空闲时间单元
> + workQueue等待队列，当队列中任务已经满了，则加开到最大线程数执行。若任务提交超过最大队列数，则执行拒绝操作
> + RejectedExecutionHandler 拒绝策略，当队列数容量已经达到最大的时候，拒绝任务提交的处理方法
> + 当线程数已经达到maxPoolSize，切队列已满，会拒绝新任务
> + 当线程池被调用shutdown()后，会等待线程池里的任务执行完毕，再shutdown。如果在调用shutdown()和线程池真正shutdown之间提交任务，会拒绝新任务，需要catch异常进行处理。
* ThreadPoolExecutor类有几个内部实现类来处理这类情况：
> + AbortPolicy 丢弃任务，抛运行时异常
> + CallerRunsPolicy 执行任务
> + DiscardPolicy 忽视，什么都不会发生
> + DiscardOldestPolicy 从队列中踢出最先进入队列（最后一个执行）的任务
> + 实现RejectedExecutionHandler接口，可自定义处理器


* 线程池构造函数常用对象 
* __原则:有界队列满了之后会加开额外的线程去执行任务__
> + ArrayBlockingQueue：基于数组的有界阻塞队列，按FIFO排序。新任务进来后，会放到该队列的队尾，有界的数组可以防止资源耗尽问题。当线程池中线程数量达到corePoolSize后，再有新任务进来，则会将任务放入该队列的队尾，等待被调度。如果队列已经是满的，则创建一个新线程，如果线程数量已经达到maxPoolSize，则会执行拒绝策略。
> + LinkedBlockingQuene:基于链表的无界阻塞队列（其实最大容量为Interger.MAX），按照FIFO排序。由于该队列的近似无界性，当线程池中线程数量达到corePoolSize后，再有新任务进来，会一直存入该队列，而不会去创建新线程直到maxPoolSize，因此使用该工作队列时，参数maxPoolSize其实是不起作用的。(若指定其容量，则maxPoolSize起作用？？？)
> + SynchronousQuene:一个不缓存任务的阻塞队列，生产者放入一个任务必须等到消费者取出这个任务。也就是说新任务进来时，不会缓存，而是直接被调度执行该任务，如果没有可用线程，则创建新线程，如果线程数量达到maxPoolSize，则执行拒绝策略。
> + PriorityBlockingQueue:具有优先级的无界阻塞队列，优先级通过参数Comparator实现。

* 


