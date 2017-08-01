# 定時任務

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