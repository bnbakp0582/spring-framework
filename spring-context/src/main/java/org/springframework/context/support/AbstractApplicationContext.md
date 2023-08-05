## 实现Lifecycle

### close

- doClose
> 1. 发布ContextClosedEvent
> 2. 暂停所有Lifecycle的bean，避免真正销毁的时候有延迟
> 3. 销毁beanfactory中缓存的bean
> 4. 关闭context状态
> 5. 重置context的监听器
> 6. 触发子类的onclose
- 撤回jvm钩子
> 之前注册到jvm上的销毁钩子此时已经不需要了，因为context已经关闭了，所以撤回
> 下面可以看看，这个钩子是怎么注册的：

在Spring框架的AbstractApplicationContext类中，当ApplicationContext被创建并初始化后，会注册一个JVM Shutdown Hook。具体的注册过程在registerShutdownHook()方法中进行，这个方法通常在ApplicationContext初始化完成后被调用。

这个JVM Shutdown Hook是一个线程，它的任务是在JVM关闭时关闭ApplicationContext，以确保所有的Spring Bean都能被正确地销毁，所有的资源（如数据库连接、文件流等）都能被正确地释放。

以下是registerShutdownHook()方法的一个基本实现：

```java
public void registerShutdownHook() {
    if (this.shutdownHook == null) {
        // No shutdown hook registered yet.
        this.shutdownHook = new Thread() {
            @Override
            public void run() {
                doClose();
            }
        };
        Runtime.getRuntime().addShutdownHook(this.shutdownHook);
    }
}
```