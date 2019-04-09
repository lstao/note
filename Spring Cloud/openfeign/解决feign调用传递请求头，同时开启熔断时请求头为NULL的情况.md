Feign请求设置Token请求头，但是开启熔断之后，RequestContextHolder.getRequestAttributes()为空
====

最近在做Spring Cloud项目时，遇到了一个关于Feign携带token访问其他微服务模块的问题。通过查阅博客与相关文档，我在Spring Boot应用的启动类中添加了重写RequestInterceptor的匿名类，实现每次在发送feign请求时，都将当前用户的token头再次添加到feign的请求头中，实现了系统之中个微服务模块的相互认证调用。代码如下：
```
	@Bean
	public RequestInterceptor headerInterceptor() {
		return template -> {
			ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
			HttpServletRequest request = attributes.getRequest();
			Enumeration<String> headerNames = request.getHeaderNames();
			if (headerNames != null) {
				while (headerNames.hasMoreElements()) {
					String name = headerNames.nextElement();
					String values = request.getHeader(name);
					template.header(name, values);
				}
			}
		};
	}
```

到这里已经可以实现feign请求携带token对其他服务进行请求了，但是在进一步完善feign，开启熔断机制之后。上述代码的
```
ServletRequestAttributes attributes = (ServletRequestAttributes)=RequestContextHolder.getRequestAttributes();
```
返回的attributes返回为空，不能将token携带至token请求头中。在查阅相关资料之后，有两种解决方案

第一种是设置Hystrix的并发策略为信号量机制
----
```
hystrix.command.default.execution.isolation.strategy: SEMAPHORE
```

这样配置后，Feign可以正常工作。

但该方案不是特别好。原因是Hystrix官方强烈建议使用THREAD作为隔离策略！ 可以参考官方文档说明

第二种是继承HystrixConcurrencyStrategy类，自定义Hystrix的并发策略
----

```

@Component
public class FeignHystrixConcurrencyStrategy extends HystrixConcurrencyStrategy {

  private static final Logger log = LoggerFactory.getLogger(FeignHystrixConcurrencyStrategy.class);
  private HystrixConcurrencyStrategy delegate;  

  public FeignHystrixConcurrencyStrategy() {  
    try {  
      this.delegate = HystrixPlugins.getInstance().getConcurrencyStrategy();  
      if (this.delegate instanceof FeignHystrixConcurrencyStrategy) {  
        // Welcome to singleton hell...  
        return;  
      }  
      HystrixCommandExecutionHook commandExecutionHook =
          HystrixPlugins.getInstance().getCommandExecutionHook();
      HystrixEventNotifier eventNotifier = HystrixPlugins.getInstance().getEventNotifier();
      HystrixMetricsPublisher metricsPublisher = HystrixPlugins.getInstance().getMetricsPublisher();
      HystrixPropertiesStrategy propertiesStrategy =
          HystrixPlugins.getInstance().getPropertiesStrategy();  
      this.logCurrentStateOfHystrixPlugins(eventNotifier, metricsPublisher, propertiesStrategy);  
      HystrixPlugins.reset();  
      HystrixPlugins.getInstance().registerConcurrencyStrategy(this);  
      HystrixPlugins.getInstance().registerCommandExecutionHook(commandExecutionHook);  
      HystrixPlugins.getInstance().registerEventNotifier(eventNotifier);  
      HystrixPlugins.getInstance().registerMetricsPublisher(metricsPublisher);  
      HystrixPlugins.getInstance().registerPropertiesStrategy(propertiesStrategy);  
    } catch (Exception e) {  
      log.error("Failed to register Sleuth Hystrix Concurrency Strategy", e);  
    }  
  }  

  private void logCurrentStateOfHystrixPlugins(HystrixEventNotifier eventNotifier,  
      HystrixMetricsPublisher metricsPublisher, HystrixPropertiesStrategy propertiesStrategy) {  
    if (log.isDebugEnabled()) {  
      log.debug("Current Hystrix plugins configuration is [" + "concurrencyStrategy ["  
          + this.delegate + "]," + "eventNotifier [" + eventNotifier + "]," + "metricPublisher ["  
          + metricsPublisher + "]," + "propertiesStrategy [" + propertiesStrategy + "]," + "]");  
      log.debug("Registering Sleuth Hystrix Concurrency Strategy.");  
    }  
  }  

  @Override  
  public <T> Callable<T> wrapCallable(Callable<T> callable) {
    RequestAttributes requestAttributes = RequestContextHolder.getRequestAttributes();
    return new WrappedCallable<>(callable, requestAttributes);  
  }  

  @Override  
  public ThreadPoolExecutor getThreadPool(HystrixThreadPoolKey threadPoolKey,
                                          HystrixProperty<Integer> corePoolSize, HystrixProperty<Integer> maximumPoolSize,
                                          HystrixProperty<Integer> keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue) {
    return this.delegate.getThreadPool(threadPoolKey, corePoolSize, maximumPoolSize, keepAliveTime,  
        unit, workQueue);  
  }  

  @Override  
  public ThreadPoolExecutor getThreadPool(HystrixThreadPoolKey threadPoolKey,  
      HystrixThreadPoolProperties threadPoolProperties) {
    return this.delegate.getThreadPool(threadPoolKey, threadPoolProperties);  
  }  

  @Override  
  public BlockingQueue<Runnable> getBlockingQueue(int maxQueueSize) {  
    return this.delegate.getBlockingQueue(maxQueueSize);  
  }  

  @Override  
  public <T> HystrixRequestVariable<T> getRequestVariable(HystrixRequestVariableLifecycle<T> rv) {
    return this.delegate.getRequestVariable(rv);  
  }  

  static class WrappedCallable<T> implements Callable<T> {  
    private final Callable<T> target;  
    private final RequestAttributes requestAttributes;  

    public WrappedCallable(Callable<T> target, RequestAttributes requestAttributes) {  
      this.target = target;  
      this.requestAttributes = requestAttributes;  
    }  

    @Override  
    public T call() throws Exception {  
      try {  
        RequestContextHolder.setRequestAttributes(requestAttributes);  
        return target.call();  
      } finally {  
        RequestContextHolder.resetRequestAttributes();  
      }  
    }  
  }  

```

然后配合上面的RequestInterceptor，就可以实现开启熔断之后的feign请求携带token访问其他服务，但是具体内部实现还没有进一步学习，这里只提供解决方案，自身还远远需要更加深入的学习内部实现机制，这里还是需要感谢各位大神


本博客参考自 [feign调用session丢失解决方案](https://blog.csdn.net/Crystalqy/article/details/79083857) ，感谢这位大神，详情可进入此链接查看




