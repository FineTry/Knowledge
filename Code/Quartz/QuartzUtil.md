```java
// 工具类
public class QuartzUtil {
    private static SchedulerFactory schedulerFactory = new StdSchedulerFactory();

    /**
     * 采用cron表达式执行调度
     * @param jobCollect
     *          自定义数据类，作为数据传输到调度执行方法里面
     * @param jobKey
     *          标识这个job的唯一行，包含name和group（可用id做标识）
     * @param triggerKey
     *          标识这个任务的唯一性，包含name和group（可用id做标识）
     * @param cron
     */
    public static void create(JobCollectMetadataDetail jobCollect, JobKey jobKey, TriggerKey triggerKey, String cron) throws SchedulerException {
        Scheduler scheduler = schedulerFactory.getScheduler();
        JobDataMap jobDataMap = new JobDataMap();
        jobDataMap.put("jobCollect", jobCollect);
        JobDetail jobDetail = JobBuilder.newJob(CollectJob.class)
            .withIdentity(jobKey)
            .usingJobData(jobDataMap).build();

        CronTrigger cronTrigger = TriggerBuilder.newTrigger()
                .withIdentity(triggerKey)
                .forJob(jobKey)
                .withSchedule(CronScheduleBuilder.cronSchedule(cron))
                .startNow()
                .build();
        scheduler.scheduleJob(jobDetail, cronTrigger);
        scheduler.start();
    }

    public static void deleteJob(JobKey jobKey) throws SchedulerException {
        schedulerFactory.getScheduler().deleteJob(jobKey);
    }

    /**
     * 暂停
     */
    public static void pauseJob(JobKey jobKey) throws SchedulerException {
        schedulerFactory.getScheduler().pauseJob(jobKey);
    }

    /**
     * 恢复
     */
    public static void resumeJob(JobKey jobKey) throws SchedulerException {
        schedulerFactory.getScheduler().resumeJob(jobKey);
    }

    /**
     * 替换任务
     * jobCollect数据需要替换
     */
    public static void modifyJobTime(JobCollectMetadataDetail jobCollect, TriggerKey triggerKey, String cron) {
        try {
            CronTrigger trigger = (CronTrigger) schedulerFactory.getScheduler().getTrigger(triggerKey);
            if (trigger == null) {
                return;
            }

            String oldTime = trigger.getCronExpression();
            if (!oldTime.equalsIgnoreCase(cron)) {
                // 触发器
                TriggerBuilder<Trigger> triggerBuilder = TriggerBuilder.newTrigger();
                triggerBuilder.withIdentity(triggerKey)
                        .withSchedule(CronScheduleBuilder.cronSchedule(cron))
                        .startNow();
                // 方式一 ：修改一个任务的触发时间
                JobDataMap jobDataMap = new JobDataMap();
                jobDataMap.put("jobCollect", jobCollect);
                trigger = (CronTrigger) triggerBuilder.usingJobData(jobDataMap).build();
                schedulerFactory.getScheduler().rescheduleJob(triggerKey, trigger);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 任务是否存在
     */
    public Boolean notExists(TriggerKey triggerKey) {
        try {
            return schedulerFactory.getScheduler().getTriggerState(triggerKey) == Trigger.TriggerState.NONE;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

