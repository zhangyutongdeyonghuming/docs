
# Spring事件

> Event 定义事件

```java
@Getter
public class Event extends ApplicationEvent {  
  
    private String msg;  
  
    public Event(String msg) {  
        super(msg);  
        this.msg = msg;  
    }  

}
```

> Publish 定义发布

```java
@Component
public class Publisher {  
  
    @Resource  
    private ApplicationContext applicationContext;  
  
    public void publish(String msg) {  
        applicationContext.publishEvent(new Event(msg));  
    }  
}
```

>Listener 定义监听

```java
@Component
@Slf4j
public class Listener implements ApplicationListener<Event> {  

    @Override  
    @Async
	public void onApplicationEvent(Event event) {  
        String msg = event.getMsg();
        log.info("{}", msg);
    }  
}
```

> 使用

```java
@Resource 
private Publisher publisher;

@Test
public void testPublish(){
	publisher.publish("消息!");
}

```

# Spring接收Date参数



## Spring MVC 版

> convert代码

```java
public class DateConverter implements Converter<String, Date> {

    /**
     * date format 格式
     */
    private static final String[] PATTERNS = {"yyyy-MM-dd HH:mm:ss", "yyyy-MM-dd", "yyyy-MM", "HH:mm:ss", "HH:mm"};

    @Override
    public Date convert(String source) {
        if (StringUtils.isNotBlank(source)) {
            try {
                log.info("时间日期绑定:{}", source);
                return DateUtils.parseDateStrictly(source, PATTERNS);
            } catch (Exception e) {
                // 异常时参数绑定失败
                log.error("时间日期绑定异常!", e);
            }
        }
        return null;
    }
}
```



> xml配置

```xml
<bean id="dateConverter" class="com.oneclick.scm.util.DateConverter"/>

<!-- request请求参数绑定类型定义-->
<bean id="conversionServiceFactory" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <ref bean="dateConverter"/>
        </set>
    </property>
</bean>
```



## SpringBoot 版

> convert

```java
@Component
public class StringToDateConverter implements Converter<String, Date> {

    /**
     * 将String字符串转换成Date日期格式
     * @param source 字符串日期格式
     * @return Date日期格式
     */
    @Override
    public Date convert(String source) {
        /*创建Date日期对象*/
        Date date = null;

        try {
            /*判断传入的字符串日期格式是否为空*/
            if (StringUtils.isEmpty(source)) {
                /*返回为空*/
                return null;
            }

            /*转换日期格式*/
            date = new SimpleDateFormat("yyyy-MM-dd").parse(source);
        } catch (ParseException e) {
            throw new RuntimeException(e);
        }

        /*返回日期对象*/
        return date;
    }
}
```



> mvc config

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Resource
    private StringToDateConverter stringToDateConverter;

    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addConverter(stringToDateConverter);
    }
}

```


## 跨域

```java
@Configuration  
public class CorsConfig implements WebMvcConfigurer {  
  
    @Override  
    public void addCorsMappings(CorsRegistry registry) {  
        //添加映射路径  
        registry.addMapping("/**")  
                //是否发送Cookie  
                .allowCredentials(true)  
                //设置放行哪些原始域   SpringBoot2.4.4下低版本使用.allowedOrigins("*")  
                .allowedOriginPatterns("*")  
                //放行哪些请求方式  
                .allowedMethods("GET", "POST", "PUT", "DELETE")  
                //.allowedMethods("*") //或者放行全部  
                //放行哪些原始请求头部信息  
                .allowedHeaders("*")  
                //暴露哪些原始请求头部信息  
                .exposedHeaders("*");  
    }  
}
```