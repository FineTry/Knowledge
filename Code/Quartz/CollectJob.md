```java
// 执行器，执行具体定时任务
@Component
public class CollectJob extends QuartzJobBean {
    
    private static TransactionTemplate transactionTemplate;

    /**
     * Quartz不支持直接bean注入，我们这里去容器拿bean
     * @PostConstruct  Constructor(构造方法) -> @Autowired(依赖注入) -> @PostConstruct(注释的方法)
     *          被@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次。
     *          PostConstruct在构造函数之后执行，init（）方法之前执行。
     */
    @PostConstruct
    private void init(){
        transactionTemplate = SpringUtil.getBean("transactionTemplate");
    }

    /**
     * 不支持@Transactional,我们采用编程性事务
     */
    @Override
    protected void executeInternal(JobExecutionContext context) throws JobExecutionException {
        JobDataMap jobDataMap = context.getMergedJobDataMap();
        // 获取数据
        JobCollectMetadataDetail jobCollect = (JobCollectMetadataDetail) jobDataMap.get("jobCollect");
        // 事务支持
        transactionTemplate.execute(ts -> {
            try {
             // 业务代码
            } catch (Exception e) {
                // 错误是否回滚
                ts.setRollbackOnly();
            }
            return null;
        });
    }
}
```

