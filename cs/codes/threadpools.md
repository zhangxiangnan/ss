# 线程池
## 线程池工具类
```java
public class IdeThreadPools {
    // xx
    public static final ThreadPoolExecutor XX_POOL =
            initThreadPool(3, "XX_POOL", 500);
    
    private static ThreadPoolExecutor initThreadPool(int poolSize, String poolName, int workQueueSize) {
        ThreadPoolExecutor newExecutor = new ThreadPoolExecutor(poolSize, poolSize,
                0L, TimeUnit.SECONDS, new ArrayBlockingQueue<>(workQueueSize),
                new NamedTreadFactory(poolName), new DiscardLogPolicy());
        newExecutor.prestartAllCoreThreads();
        return newExecutor;
    }

    private static class NamedTreadFactory implements ThreadFactory {

        private final AtomicInteger threadNum = new AtomicInteger(1);

        private final String poolName;

        NamedTreadFactory(String poolName) {
            this.poolName = poolName;
        }

        @Override
        public Thread newThread(Runnable r) {
            return new Thread(r, poolName + "-" + threadNum.getAndIncrement());
        }
    }

    private static class DiscardLogPolicy implements RejectedExecutionHandler {

        DiscardLogPolicy() {
        }

        @Override
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            throw new RejectedExecutionException("task执行被拒绝了, task = "
                    + r.toString() + ", pool = " + e.toString());
        }
    }
}
```

## 异步执行工具类
```java
public class AsyncUtils {

    public static void runAsync(Runnable runnable, Executor executor) {
        CompletableFuture.runAsync(runnable, executor);
    }
}
```