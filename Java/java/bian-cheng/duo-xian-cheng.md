# 多線程

## ThreadPoolExecutor

【强制】线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。

```java
/**
* Creates a new {@code ThreadPoolExecutor} with the given initial
* parameters.
*
* @param corePoolSize the number of threads to keep in the pool, even
* if they are idle, unless {@code allowCoreThreadTimeOut} is set
* @param maximumPoolSize the maximum number of threads to allow in the
* pool
* @param keepAliveTime when the number of threads is greater than
* the core, this is the maximum time that excess idle threads
* will wait for new tasks before terminating.
* @param unit the time unit for the {@code keepAliveTime} argument
* @param workQueue the queue to use for holding tasks before they are
* executed. This queue will hold only the {@code Runnable}
* tasks submitted by the {@code execute} method.
* @param threadFactory the factory to use when the executor
* creates a new thread
* @param handler the handler to use when execution is blocked
* because the thread bounds and queue capacities are reached
* @throws IllegalArgumentException if one of the following holds:<br>
* {@code corePoolSize < 0}<br>
* {@code keepAliveTime < 0}<br>
* {@code maximumPoolSize <= 0}<br>
* {@code maximumPoolSize < corePoolSize}
* @throws NullPointerException if {@code workQueue}
* or {@code threadFactory} or {@code handler} is null
*/
public ThreadPoolExecutor(int corePoolSize,
    int maximumPoolSize,
    long keepAliveTime,
    TimeUnit unit,
    BlockingQueue<Runnable> workQueue,
    ThreadFactory threadFactory,
    RejectedExecutionHandler handler) { 
```
* corePoolSize - 线程池核心池的大小。
* maximumPoolSize - 线程池的最大线程数。
* keepAliveTime - 当线程数大于核心时，此为终止前多余的空闲线程等待新任务的最长时间。
* unit - keepAliveTime 的时间单位。
* workQueue - 用来储存等待执行任务的队列。
* threadFactory - 线程工厂。
* handler - 拒绝策略。

### 关注点1 线程池大小

线程池有两个线程数的设置，一个为核心池线程数，一个为最大线程数。

在创建了线程池后，默认情况下，线程池中并没有任何线程，等到有任务来才创建线程去执行任务，除非调用了prestartAllCoreThreads()或者prestartCoreThread()方法
当创建的线程数等于 corePoolSize 时，会加入设置的阻塞队列。当队列满时，会创建线程执行任务直到线程池中的数量等于maximumPoolSize。

说明：Executors 各个方法的弊端：
1）newFixedThreadPool 和 newSingleThreadExecutor:
主要问题是堆积的**请求处理队列**可能会耗费非常大的内存，甚至 OOM(Out Of Memory)。
2）newCachedThreadPool 和 newScheduledThreadPool:
主要问题是线程数最大数是 Integer.MAX_VALUE，可能会**创建数量非常多的线程**，甚至 OOM。

## Executors

### newSingleThreadExecutor

创建一个单线程的线程池。这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。
此线程池保证所有任务的执行顺序按照任务的提交顺序执行。
```java
new ThreadPoolExecutor(1, 1,0L,TimeUnit.MILLISECONDS,new LinkedBlockingQueue<Runnable>())
```
### newFixedThreadPool

创建固定大小的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。
线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。
```java
new ThreadPoolExecutor(nThreads, nThreads, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>());
```

### newCachedThreadPool

创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，
那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。
此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
```java
new ThreadPoolExecutor(0, Integer.MAX_VALUE, 60L, TimeUnit.SECONDS,new SynchronousQueue<Runnable>());
```

## ThreadLocal

必须回收自定义的ThreadLocal变量，尤其在线程池场景下，线程经常会被复用，如果不清理自定义的 ThreadLocal变量，可能会影响后续业务逻辑和造成内存泄露等问题。尽量在代理中使用try-finally块进行回收。

```java
/**
     * @author caikang
     * @date 2017/04/07
     */
    public class UserHolder {
        private static final ThreadLocal<User> userThreadLocal = new ThreadLocal<User>();

        public static void set(User user){
            userThreadLocal.set(user);
        }

        public static User get(){
            return userThreadLocal.get();
        }

        public static void remove(){
            userThreadLocal.remove();
        }
    }

    /**
     * @author caikang
     * @date 2017/04/07
     */
    public class UserInterceptor extends HandlerInterceptorAdapter {
        @Override
        public boolean preHandle(HttpServletRequest request,
            HttpServletResponse response, Object handler) throws Exception {
            UserHolder.set(new User());
            return true;
        }

        @Override
        public void afterCompletion(HttpServletRequest request,
            HttpServletResponse response, Object handler, Exception ex) throws Exception {
            UserHolder.remove();
        }
    }
```

### 示例解決方法

新建一個攔截器
```java
public class CatInfoInterceptor extends HandlerInterceptorAdapter {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        HandlerMethod method = (HandlerMethod) handler;
        CatInfo.init(method.getMethod().getDeclaringClass() + "->" + method.getMethod().getName());
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        CatInfo.remove();
    }

    @Override
    public void afterConcurrentHandlingStarted(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    }
}
```

註冊攔截器
```java
@Configuration
public class AppConfig extends WebMvcConfigurerAdapter {

    /**
     *
     * @param registry
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new CatInfoInterceptor()).addPathPatterns("/api/mpg/**");
    }
}
```

CatInfo實現Remove
```java
public static void remove() {
        catInfoThreadLocal.remove();
    }
```
