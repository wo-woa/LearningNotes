## AnonymousDeleteMapping

结合AnonymousAccess，其他5个类有相似功能，标注在controller的方法上，表示该类不需要验证token



**@AnonymousAccess**

在注解的上面添加注解AnonymousAccess，表示这是AnonymousDeleteMapping的父类，在SpringSecurityConfig排除AnonymousAccess也会把这个注解包含进去



**@RequestMapping(method = RequestMethod.DELETE)**

表示也是该类的父类，实现了系统的功能，并且指定了一个参数表示是delete方法，就可以只写这么一个注解，省去了写RequestMapping和AnonymousAccess两个注解

不能直接使用@DeleteMapping，因为这个注解的target未method只能用在方法上，而RequestMapping不仅有method还有TYPE



**@AliasFor(annotation = RequestMapping.class)**
表示这个注解继承了RequestMapping的功能，具体可以看@DeleteMapping的源代码，两者非常相似	 

## DataPermission





## Limit

结合LimitAspect使用，实现一个接口或者一个ip在一定时间内访问次数的限制

LimitAspect结合RequestHolder使用，获取当前调用接口的request对象



### 切面的使用方式

@Aspect和@Component切面类必备的两个注解

由于需要使用redis，需要导入redisTemplate，使用构造方法将其导入



**@Pointcut("@annotation(me.zhengjie.annotation.Limit)")**

注明是哪个注解的切面方法，这个方法的名字需要和下面Around注解中的名字系统



**@Around("pointcut()")**

pointcut即为上面Pointcut的方法名，

```java
public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
    HttpServletRequest request = RequestHolder.getHttpServletRequest();
    MethodSignature signature = (MethodSignature) joinPoint.getSignature();
    Method signatureMethod = signature.getMethod();
    Limit limit = signatureMethod.getAnnotation(Limit.class);
    LimitType limitType = limit.limitType();
    String key = limit.key();
    if (StringUtils.isEmpty(key)) {
        if (limitType == LimitType.IP) {
            key = StringUtils.getIp(request);
        } else {
            key = signatureMethod.getName();
        }
    }
    ImmutableList<Object> keys = ImmutableList.of(StringUtils.join(limit.prefix(), "_", key, "_", request.getRequestURI().replaceAll("/","_")));

    String luaScript = buildLuaScript();
    RedisScript<Number> redisScript = new DefaultRedisScript<>(luaScript, Number.class);
    Number count = redisTemplate.execute(redisScript, keys, limit.count(), limit.period());
    if (null != count && count.intValue() <= limit.count()) {
        logger.info("第{}次访问key为 {}，描述为 [{}] 的接口", count, keys, limit.name());
        return joinPoint.proceed();
    } else {
        throw new BadRequestException("访问次数受限制");
    }
}
```

上面是通过RequestHolder获取当前的request对象，通过joinPoint获取添加这注解的方法和注解上的信息，通过Limit注解上的type判断是ip还是方法的限制，来分别获取ip和方法名。

通过ImmutableList.of生成一个list，这里使用这个方法，我个人感觉只是为了创建比较方便快捷，省去了add的操作。

下面创建redis的lua脚本来计数，个人感觉也有点秀技术的感觉。



**RequestHolder**

https://www.jianshu.com/p/e8d33f57373c

获取当前调用接口的request对象

```java
public static HttpServletRequest getHttpServletRequest() {
    return ((ServletRequestAttributes) Objects.requireNonNull(RequestContextHolder.getRequestAttributes())).getRequest();
}
```

通过RequestContextHolder获取当前的RequestAttributes，使用Objects判断是否为空，再获取request对象返回



**Object.requireNonNull 方法说明**

进行了一个简单的判断, 如果所要判断的元素为 `null`, 则返回空指针异常 `NullPointerException`, 否则直接返回对应的对象.



**immutable集合**

用来创建不可变集合

https://www.cnblogs.com/qdhxhz/p/9404654.html

使用java自带的Collections.unmodifiableList(list)创建集合，如果list改变了，那么这个不可变集合也会改变，而使用Guava中的ImmutableList.of则不会。