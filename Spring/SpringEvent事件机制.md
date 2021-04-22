```java
// 定义一个事件
public class EventDemo extends ApplicationEvent {
    private String message;


    public EventDemo(Object source, String message) {
        super(source);
        this.message = message;
    }

    public String getMessage() {
        return message;
    }
}

// 定义一个事件监听者
@Component
public class EventDemoListener implements ApplicationListener<EventDemo> {
    @Override
    public void onApplicationEvent(EventDemo event) {
        System.out.println("receiver " + event.getMessage());
    }
}

// 事件发布
@Component
public class EventDemoPublish {
    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    public void publish(String message) {
        EventDemo demo = new EventDemo(this, message);
        applicationEventPublisher.publishEvent(demo);
    }
}
```

