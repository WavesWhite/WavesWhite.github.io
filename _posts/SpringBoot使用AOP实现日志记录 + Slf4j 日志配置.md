---
title: SpringBoot使用AOP实现日志记录 + Slf4j 日志配置
top_img: https://w.wallhaven.cc/full/yx/wallhaven-yx7ypl.jpg
cover: https://w.wallhaven.cc/full/85/wallhaven-85xgxo.jpg
abbrlink: SpringBoot使用AOP实现日志记录 + Slf4j 日志配置
tags:
    - JAVA
    - SpringBoot
    - AOP
    - Slf4j
categories:
    - 杂七杂八

date: 2022/11/1 23:11:08 
---

# 下载依赖
~~~xml
<!--fastjson依赖-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
</dependency>
<!--AOP-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
~~~

# 日志打印格式
~~~java
log.info("=======Start=======");
// 打印请求 URL
log.info("URL            : {}",);
// 打印描述信息
log.info("BusinessName   : {}", );
// 打印 Http method
log.info("HTTP Method    : {}", );
// 打印调用 controller 的全路径以及执行方法
log.info("Class Method   : {}.{}", );
// 打印请求的 IP
log.info("IP             : {}",);
// 打印请求入参
log.info("Request Args   : {}",);
// 打印出参
log.info("Response       : {}", );
// 结束后换行
log.info("=======End=======" + System.lineSeparator());
~~~

# SystemLog 自定义切点注解
~~~java
//@Around()   // 跳转到源码处，参考源注解
@Retention(RetentionPolicy.RUNTIME) // 注解保持的阶段
@Target({ElementType.METHOD})   // 注解可以加在哪些东西的上面，该注解可以注解在 方法上
public @interface SystemLog {   // 日志记录 切点注解

    String BusinessName() default "";   // 自定义 描述信息

}
~~~

# LogAspect 日志记录切面类
~~~java
/**
 * 日志记录切面类
 */

@Component  // 注入容器
@Aspect // 标识为切面类
@Slf4j  // 日志
public class LogAspect {

    // 使用注解标识的方法 判断切点
    @Pointcut("@annotation(org.bailang.annotation.SystemLog)")  // @annotation()的参数最好是 完整路径（记得加双引号）
    public void pointCut(){   // 切点

    }

    /**
     * 处理输出日志的 advice
     * @Around 环绕控制，在 join point 前和 joint point 退出后都执行的 advice
     * @param joinPoint 被增强的方法信息封装出来的对象
     * @return
     */
    @Around("pointCut()")   // ”pointCut()“ 所运用的切点的方法名
    public Object printLog(ProceedingJoinPoint joinPoint) throws Throwable {    // 抛出异常，让统一异常处理。否则所有异常都在这里出现，难以定位异常

        Object result = null;
        try {
            handleBefore(joinPoint);    // 目标方法调用前 执行的方法
            result = joinPoint.proceed();   // 相当于调用增强的目标方法，获取目标方法的返回结果
            handleAfter(result);     // 目标方法调用后 执行的方法
        } finally { //  try后必须执行的语句
            // 结束后换行
            log.info("=======End=======" + System.lineSeparator()); // System.lineSeparator()：获取当前所执行程序的系统的换行符
        }

        return result;  // 必须返回 调用目标方法之后返回的的结果
    }

    private void handleBefore(ProceedingJoinPoint joinPoint) {    // 目标方法调用前 执行的方法

        // RequestAttributes requestAttributes = RequestContextHolder.getRequestAttributes();  // 获取当前线程的信息 请求对象

        // 获取当前线程的信息（需将RequestContextHolder接口强转为实现类） 请求对象
        ServletRequestAttributes requestAttributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = requestAttributes.getRequest();    // 获取 请求对象

        // 获取 被增强方法的注解对象
        SystemLog systemLog = getSystemLog(joinPoint);

        log.info("=======Start=======");
        // 打印当前时间
        // log.info("Time           : {}", new SimpleDateFormat("yyyy-MM-dd : HH:mm:ss").format(new Date()));
        // 打印请求 URL
        log.info("URL            : {}", request.getRequestURL());
        // 打印描述信息
        log.info("BusinessName   : {}", systemLog.BusinessName());
        // 打印 Http method
        log.info("HTTP Method    : {}", request.getMethod());
        // 打印调用 controller 的全路径以及执行方法   (通过joinPoint封装的方法信息获取)
        log.info("Class Method   : {}.{}", joinPoint.getSignature().getDeclaringType(), joinPoint.getSignature().getName());
        // 打印请求的 IP
        log.info("IP             : {}", request.getRemoteHost());

        // 过滤目标方法参数， 对 特殊参数 进行特殊封装处理
        Object[] args = filterArgs(joinPoint.getArgs());
        // 打印请求入参   (通过joinPoint封装的方法信息获取)
        // log.info("Request Args   : {}", JSON.toJSONString(joinPoint.getArgs()));    // 将数组转换为JSON输出
        log.info("Request Args   : {}", JSON.toJSONString(args));    // 将数组转换为JSON输出
    }

    private Object[] filterArgs(Object[] args) {    // 过滤目标方法参数， 对 特殊参数 进行特殊封装处理
        Object[] result = new Object[args.length];

        for(int i = 0;i < args.length;++i){
            if(args[i] instanceof MultipartFile){   // 过滤 MultipartFile类 参数， 对该参数进行特殊封装处理， 以便序列化为JSON
                MultipartFile multipartFile = (MultipartFile)args[i];
                // 对 MultipartFile类参数 进行特殊封装， 封装为：MultipartFileLogArgVo类
                MultipartFileLogArgVo multipartFileLogArgVo = new MultipartFileLogArgVo(multipartFile.getName(), multipartFile.getOriginalFilename(), multipartFile.getContentType(), multipartFile.getSize());
                // 存入 返回结果参数数组
                result[i] = multipartFileLogArgVo;
            }
            else{
                result[i] = args[i];
            }
        }

        return result;
    }

    private void handleAfter(Object result) {   // 目标方法调用后 执行的方法
        // 打印出参
        log.info("Response       : {}", JSON.toJSONString(result));    // 进行JSON的序列化，转换为JSON格式输出
    }

    private SystemLog getSystemLog(ProceedingJoinPoint joinPoint) {    // 获取 被增强方法的注解对象

        // joinPoint.getSignature() 相当于把 注解了切点注解的整段方法代码块 封装成了一个对象
        MethodSignature methodSignature = (MethodSignature) joinPoint.getSignature();    // 强转为子接口MethodSignature，获取 被增强方法的方法对象
        SystemLog systemLog = methodSignature.getMethod().getAnnotation(SystemLog.class);   // getAnnotation()参数：传入所需获取注解的字节码

        return systemLog;
    }
}
~~~

## 示例：标注方法为切点
~~~java
@RestController
@RequestMapping("/article")
public class  ArticleController {

    @Autowired
    private ArticleService articleService;

    // @CrossOrigin // 跨域注解
    @GetMapping("/hotArticleList")
    @SystemLog(BusinessName = "热门文章列表查询")
    public ResponseResult hotArticleList(){ // 热门文章列表查询
        //查询热门文章，封装成ResponseResult返回
        ResponseResult result = articleService.hotArticleList();

        return result;
    }

    @GetMapping("/articleList")
    @SystemLog(BusinessName = "分页查询文章列表")
    public ResponseResult articleList(Long categoryId, Integer pageNum, Integer pageSize){  // 分页查询文章列表

        ResponseResult result = articleService.articleList(categoryId, pageNum, pageSize);

        return result;
    }

    // @RequestMapping(value = "{id}", method = RequestMethod.GET)
    @GetMapping("{id}")
    @SystemLog(BusinessName = "获取文章详情")
    public ResponseResult getArticleDetail(@PathVariable("id") Long id){    // 获取文章详情

        ResponseResult result = articleService.getArticleDetail(id);

        return result;
    }

    //TODO 更新文章浏览次数
}
~~~

# Slf4j 日志配置
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="false">
    <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径-->
    <!--该路径为：当前项目文件夹下/Logs/vue-admin/log-->
    <property name="LOG_HOME" value="./Logs/vue-admin/log" />

    <!-- 彩色日志 -->
    <!-- 彩色日志依赖的渲染类 -->
    <conversionRule conversionWord="clr" converterClass="org.springframework.boot.logging.logback.ColorConverter" />
    <conversionRule conversionWord="wex" converterClass="org.springframework.boot.logging.logback.WhitespaceThrowableProxyConverter" />
    <conversionRule conversionWord="wEx" converterClass="org.springframework.boot.logging.logback.ExtendedWhitespaceThrowableProxyConverter" />
    <!-- 彩色日志格式 -->
    <property name="CONSOLE_LOG_PATTERN" value="${CONSOLE_LOG_PATTERN:-%clr(%d{yyyy-MM-dd HH:mm:ss.SSS}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %clr(---){faint} %clr([%t]){faint} %clr(%logger{}){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>

    <!-- 控制台输出 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${log.pattern}</pattern>
        </encoder>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期、%thread表示线程名、%-5level：级别从左显示5个字符宽度、%logger{}：调用类名称，{}里面是字符数限制、%msg：日志消息，%n是换行符-->
            <pattern>${CONSOLE_LOG_PATTERN}</pattern>
        </encoder>
    </appender>

    <!--按照每天生成日志文件-->
    <appender name="FILE"  class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
            <FileNamePattern>${LOG_HOME}/admin.%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--<FileNamePattern>${LOG_HOME}/blog.%d{yyyy-MM-dd_HH-mm-ss}.log</FileNamePattern>-->

            <!--日志文件保留天数-->
            <MaxHistory>60</MaxHistory>
        </rollingPolicy>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期、%thread表示线程名、%-5level：级别从左显示5个字符宽度、%logger{}：调用类名称，{}里面是字符数限制、%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS}---[%thread]--- %-5level--- %logger{}  -  %msg%n</pattern>
        </encoder>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>50MB</MaxFileSize>
        </triggeringPolicy>
    </appender>


    <!--myibatis log configure-->
    <!--<logger name="com.apache.ibatis" level="DEBUG"/>
    <logger name="java.sql.Connection" level="DEBUG"/>
    <logger name="java.sql.Statement" level="DEBUG"/>
    <logger name="java.sql.PreparedStatement" level="DEBUG"/>-->

    <!--<logger name="com.guardlbt.mapper" level="INFO"></logger>-->

    <!-- 日志输出级别 -->
    <root level="INFO">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </root>

</configuration>
~~~


