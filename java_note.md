### 序列化

​	Serializable接口中一个成员变量/函数都没有。

​	序列化保存的是**对象**的状态，而不是类的状态，所以static修饰的变量都不保存。

​	串行化保存的是变量的值，不保存变量的任何修饰符。

​	transient关键字：阻止用该关键字的声明的变量持久化。

​	父类实现序列化，子类自动实现。

​	SerialVersionUID：



### 匿名内部类：

```java
new InterfaceName(parameters){
	//inner class methods
}
```

看起来像是实例化接口，但其实只是省略的写法。



### AQS线程

static final class Node静态内部类：

volatile int waitState 五种状态：

​	static final int CANCELLED：1	表示当前线程被取消；

​	static final int SIGNAL：-1	表示当前节点的**后继节点**包含的线程需要运行；

​	static final int CONDITION：-2	表示当前节点在等待condition，即在conditon queue中；

​	static final int PROPAGATE：-3	表示当前场景下后续的acquireShared能够执行；

​	默认状态：0	表示当前节点在sync queue中等待获取锁。



### InterruptedException	中断

​	中断是一种状态信息，要中断一个线程就是将interrupted status设置为true，具体怎么操作去响应中断则是线程自己的事。

​	throw该异常的方法一般成为阻塞方法，他们的返回一般依赖于外部条件。`Object.wait()`依赖于其他线程notify()。抛出异常后会设interrupted status为false。



### 线程池

#### 	Executor接口

```java
new Thread(new Runnable(){
    //启动一个线程
}).start();

//线程池
Executor executor = anExecutor;
executor.execute(new RunnableTask());//重写execute方法
```

#### ExecutorService接口

​	submit()方法

#### ThreadPoolExecutor类

##### 属性

- corePoolSize：核心线程数

- maximumPoolSize：最大线程数，线程池允许创建的最大线程数。

- workQueue：

  任务队列，BlockingQueue 接口的某个实现（常使用 ArrayBlockingQueue 和 LinkedBlockingQueue）。

- keepAliveTime：

  空闲线程的保活时间，如果某线程的空闲时间超过这个值都没有任务给它做，那么可以被关闭了。注意这个值并不会对所有线程起作用，如果线程池中的线程数少于等于核心线程数 corePoolSize，那么这些线程不会因为空闲太长时间而被关闭，当然，也可以通过调用 `allowCoreThreadTimeOut(true)`使核心线程数内的线程也可以被回收。

- threadFactory：

  用于生成线程，一般我们可以用默认的就可以了。通常，我们可以通过它将我们的线程的名字设置得比较可读一些，如 Message-Thread-1， Message-Thread-2 类似这样。

- handler：

  当线程池已经满了，但是又有新的任务提交的时候，该采取什么策略由这个来指定。有几种方式可供选择，像抛出异常、直接拒绝然后返回等，也可以自己实现相应的接口实现自己的逻辑，这个之后再说。

  

  线程池状态：

- RUNNING：接受新的任务，处理等待队列中的任务（定义为-1）

- SHUTDOWN：不接受新的任务提交，但是会继续处理等待队列中的任务（定义为0）

- STOP：不接受新的任务提交，不再处理等待队列中的任务，中断正在执行任务的线程（>0）

- TIDYING：所有的任务都销毁了，workCount 为 0。线程池的状态在转换为 TIDYING 状态时，会执行钩子方法 terminated()（>0）

- TERMINATED：`terminated()` 方法结束后，线程池的状态就会变成这个（>0）

> ​	用一个32位整数存放线程池状态和当前池中的线程数：
>
> ```JAVA
> private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING,0));
> ```
>
> 前3位表示线程池状态，后29位表示线程数。

​	RUNNING -> SHUTDOWN：当调用了 shutdown() 后，会发生这个状态转换，这也是最重要的

​	(RUNNING or SHUTDOWN) -> STOP：当调用 `shutdownNow() `后，会发生这个状态转换，这下要清楚 `shutDown()` 和 `shutDownNow() `的区别了

##### 内部类Worker

​	Worker 继承自 AQS 类 实现了Runnable接口。

​	线程从任务队列（BlockingQueue）中取任务（`getTask()`方法）

​	run()方法调用外部类的`runWorker()`方法。

##### execute(Runnable command)方法

​	若当前线程数 < 核心线程数 `addWorker(command, true)`后返回；

​	否则若线程池处于RUNNING状态，将command任务添加到任务队列`workQueue.offer(command)`

> 重新判断RUNNING状态，若不处于RUNNING状态则**移除当前入队任务`remove(command)`，并执行拒绝策略**；
>
> 若线程池处于RUNNING状态 && 若线程数为0，则开启新线程`addWorker(null, false)`<!--担心任务提交到队列中了，但是线程都关闭了-->

​	若 workQueue 队列满了，则以 maximumQueueSize 创建worker，失败则拒绝`reject(command)`

##### private boolean addWorker(Runnable firstTask, boolean core)方法

> ​	第二个参数core为true表示采用 核心线程数corePoolSize 作为创建线程的界限；为false表示采用 最大线程数maximumPoolSize 为界限。

- ​	进入外死循环：获取当前线程池状态，若线程池状态 >= SHUTDOWN并且**排除**线程池处于SHUTDOWN状态 && firstTask为null && workQueue非空 的情况。直接返回false

- ​	进入内死循环：获取当前线程数，若线程数大于CAPACITY或大于core所指定的界限则直接返回false；若CAS增加线程数成功，跳出双重循环；若CAS失败判断线程池状态是否改变`runStateOf(c)!=rs`，若改变则回到外部循环。

  创建worker启动标志位workerStarted、成功加入workers HashSet的标志位workerAdded。获得线程池的全局锁ReentrantLock mainLock 、根据firstTask创建Worker，获得`worker.thread`。

  ​	当该线程不为null时,`mainLock.lock()`进行操作：若线程池状态小于SHUTDOWN（running） or 等于SHUTDOWN且firstTask为null，（判断之前worker的thread是否已启动，已启动抛出异常）将worker加入workers的HashSet中，获得`workers.size()`并用largestPoolSize记录最大size值。设workerAdded为true。最后`mainLock.unlock()`;

  ​	若workerAdded添加成功，启动线程，设workerStarted为true。

  若workerStarted没有启动，执行`addWorkerFailed(Worker w)`;

  返回workerStarted状态。

##### addWorkerFailed(Worker w)方法

​	获得线程池锁mainLock并加锁`mainLock.lock()`，若传入的Worker w 不为空则移除`workers.remove(w)`;`decrementWorkerCount()`WorkerCount减一；tryTerminate()后unlock()。

##### final void runWorker(Worker w)方法

​	获取w的firstTask，**若不为空 or `getTask()`返回不为null**进入**循环**，w上锁，若线程池状态大于STOP则中断当前线程，执行任务`task.run()`,后task指null，completedTasks累加并释放锁。

##### private Runnable getTask()方法

```java
1. 阻塞直到获取到任务返回。我们知道，默认 corePoolSize 之内的线程是不会被回收的，
     它们会一直等待任务
2. 超时退出。keepAliveTime 起作用的时候，也就是如果这么多时间内都没有任务，那么应该执行关闭
3. 如果发生了以下条件，此方法必须返回 null:
	- 池中有大于 maximumPoolSize 个 workers 存在(通过调用 setMaximumPoolSize 进行设置)
    - 线程池处于 SHUTDOWN，而且 workQueue 是空的，前面说了，这种不再接受新的任务
    - 线程池处于 STOP，不仅不接受新的线程，连 workQueue 中的线程也不再执行
```