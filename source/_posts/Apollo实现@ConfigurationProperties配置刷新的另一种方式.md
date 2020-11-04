---
title: Apollo实现@ConfigurationProperties配置刷新的另一种方式
---

## 背景
目前apollo官方实现@ConfigurationProperties需要配合使用EnvironmentChangeEvent或RefreshScope（需要引入springCloud-context）,考虑一种简单的实现方式如下：


## 思路
监听apollo配置刷新事件，然后通过spring的工具类获取当前配置类的bean实例对象（单例），通过反射将对应改变的配置项注入配置类bean实例。

## 代码实现
```java
@Data
@Slf4j
@Configuration
@ConfigurationProperties
@EnableApolloConfig
public class AppConfig {

    private Map<String, String> xxConfigMap;

    @ApolloConfigChangeListener
    public void onChange(ConfigChangeEvent changeEvent) {
        //this.applicationContext.publishEvent(new EnvironmentChangeEvent(changeEvent.changedKeys()));
   changeEvent.changedKeys().stream().map(changeEvent::getChange).forEach(change -> {
            log.info("Found change - key: {}, oldValue: {}, newValue: {}, changeType: {}", change.getPropertyName(), change.getOldValue(), change.getNewValue(), change.getChangeType());
            String vKey = change.getPropertyName();
            String newValue = change.getNewValue();
            if (vKey.contains(".")) {
                //取出对应的field 反射重新赋值
                AppConfig appConfig = SpringContext.getBean(AppConfig.class);
                try {
                    PropertyDescriptor propertyDescriptor = new PropertyDescriptor(vKey.substring(0, vKey.indexOf(".")), appConfig.getClass());
                    Map<String, String> map = (Map<String, String>) propertyDescriptor.getReadMethod().invoke(appConfig);
                    map.put(vKey.substring(vKey.indexOf(".")+1,vKey.length()), newValue);
                    propertyDescriptor.getWriteMethod().invoke(appConfig, map);
                } catch (Exception e) {
                    log.error("replace field {} error", vKey);
                }
            }
        });
    }


    //测试是否生效
    @PostConstruct
    void test() {
        Executors.newSingleThreadExecutor().submit(() -> {
            while (true) {
                log.info(xxConfigMap.toString());
                Thread.sleep(2000);
            }
        });
    }
}
```
## Reference
[apollo wiki spring-boot集成方式](https://github.com/ctripcorp/apollo/wiki/Java%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97#3213-spring-boot%E9%9B%86%E6%88%90%E6%96%B9%E5%BC%8F%E6%8E%A8%E8%8D%90)