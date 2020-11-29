https://blog.csdn.net/Michael_HM/article/details/81561805

https://blog.csdn.net/Michael_HM/article/details/81561805

https://blog.csdn.net/chengbinbbs/article/details/88409951

https://www.cnblogs.com/lyc88/articles/10999394.html

https://blog.csdn.net/chengbinbbs/article/details/88409951

https://www.imooc.com/article/263810

https://www.jianshu.com/p/409c78acf235

https://blog.csdn.net/qq_41907991/article/details/90489945

https://my.oschina.net/serge/blog/759072

https://blog.csdn.net/chengbinbbs/article/details/88409951

https://www.cnblogs.com/xubiao/p/9052440.html

https://www.cnblogs.com/xingzc/p/5830279.html

#### 事件机制Event

强依赖、耦合度高

```java
public class UserService {
   
    @Autowired
    EmailService emailService;

    @Autowired
    ScoreService scoreService;

    @Autowired
    OtherService otherService;

    public void register(String name) {
        System.out.println("用户：" + name + " 已注册！");
        emailService.sendEmail(name);
        scoreService.initScore(name);
        otherService.execute(name);
    }
}
```



#### 事件机制异步执行

> 参考文档 https://my.oschina.net/serge/blog/759072

```java
package hello;

import java.util.concurrent.Executor;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.AsyncConfigurerSupport;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@SpringBootApplication
@EnableAsync
public class Application extends AsyncConfigurerSupport {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(2);
        executor.setMaxPoolSize(2);
        executor.setQueueCapacity(500);
        executor.setThreadNamePrefix("GithubLookup-");
        executor.initialize();
        return executor;
    }

}
```

