# 定時任務

## Scheduled注解

```java
public @interface Scheduled {
    String cron() default "";

    String zone() default "";

    long fixedDelay() default -1L;

    String fixedDelayString() default "";

    long fixedRate() default -1L;

    String fixedRateString() default "";

    long initialDelay() default -1L;

    String initialDelayString() default "";
}
```
* cron。cron表达式
* fixedDelay。固定等待时间。
* fixedRate。固定频率。
* initialDelay。初始延迟。如果不设置，程序启动就会执行一次。

Cron表达式
```
"*/5 * * * * ? "    每隔5秒执行一次
"0 */1 * * * ? "   每隔1分钟执行一次
"0 0 12 * * ?"    每天中午十二点触发
"0 0 23 * * ?"    每天23点执行一次
"0 0 1 * * ?"    每天凌晨1点执行一次
"0 0 1 1 * ?"    每月1号凌晨1点执行一次
"0 0 23 L * ?"    每月最后一天23点执行一次
"0 0 1 ? * L"    每周星期天凌晨1点实行一次
"0 26,29,33 * * * ?"    在26分、29分、33分执行一次
"0 0 0,13,18,21 * * ?"    每天的0点、13点、18点、21点都执行一次
"0 15 10 ? * *"    每天早上10：15触发
"0 15 10 * * ?"    每天早上10：15触发
"0 15 10 * * ? *"    每天早上10：15触发
"0 15 10 * * ? 2005"    2005年的每天早上10：15触发
“0 * 14 * * ?"    每天从下午2点开始到2点59分每分钟一次触发
"0 0/5 14 * * ?"    每天从下午2点开始到2：55分结束每5分钟一次触发
"0 0/5 14,18 * * ?" 每天的下午2点至2：55和6点至6点55分两个时间段内每5分钟一次触发
"0 0-5 14 * * ?"    每天14:00至14:05每分钟一次触发
"0 10,44 14 ? 3 WED"    三月的每周三的14：10和14：44触发
"0 15 10 ? * MON-FRI"    每个周一、周二、周三、周四、周五的10：15触发
```

## 示例

示例1：

```java
/**
 * 定时任务
 * @author Administrator
 *
 */
@Configuration
@EnableScheduling
publicclass SchedulingConfig {
   
    @Scheduled(cron = "0/20 * * * * ?") // 每20秒执行一次
    publicvoid scheduler() {
        System.out.println(">>>>>>>>> SchedulingConfig.scheduler()");
    }
}
```

示例2：

```java
@Component
public class ScheduledTasks {

    private static final Logger log = LoggerFactory.getLogger(ScheduledTasks.class);

    private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");

    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        log.info("The time is now {}", dateFormat.format(new Date()));
    }
}

@SpringBootApplication
@EnableScheduling
public class Application {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class);
    }
}
```

## Spring MVC 中使用Quartz
过程：
1. 引入依赖
2. 编写job
3. 实现ServletContextListener，配置和销毁StdSchedulerFactory。
