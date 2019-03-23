用一句自己的话总结学过的设计模式（要精准）。
设计模式	    类型	   归纳	                     举例
工厂方法模式	创建型	   生产类的对象	             Beanfactory
单例模式	   创建型	  只有一个实例对象	         Calender、ApplicationContext
原型模式	   创建型	  复制对象	                BeanUtils、Cloneable、ArrayList
代理模式	   结构型	  找中介干活	               JdkDynamicAopProxy、CglibAopProxy
委派模式	   行为型	  干活算你的，功劳算我的	    DispatcherServlet
策略模式	   行为型	  不同方法，效果一样	        ResourceLoader
模板模式	   行为型	  我定了一个模板，你具体实现	JdbcTemplate
适配器模式	  结构型	   将两个不兼容的接口连接起来	HandleAdapter
装饰器模式	  结构型	   我会把你包装的更好。 扩展好	jdk中的IO
观察者模式	  行为型	   我在等通知干活	           Listener


SpringAOP:

//声明这是一个组件
@Component
//声明这是一个切面 Bean
@Aspect
@Slf4j
public class AnnotaionAspect {
//配置切入点,该方法无方法体,主要为方便同类中其他方法使用此处配置的切入点
@Pointcut("execution(* com.gupaoedu.vip.pattern.spring.aop.service..*(..))")
public void aspect(){ }
/*
* 配置前置通知,使用在方法 aspect()上注册的切入点
* 同时接受 JoinPoint 切入点对象,可以没有该参数
*/
@Before("aspect()")
public void before(JoinPoint joinPoint){
log.info("before 通知 " + joinPoint);
}
//配置后置通知,使用在方法 aspect()上注册的切入点
@After("aspect()")
public void after(JoinPoint joinPoint){
log.info("after 通知 " + joinPoint);
}
//配置环绕通知,使用在方法 aspect()上注册的切入点
@Around("aspect()")
public void around(JoinPoint joinPoint){
long start = System.currentTimeMillis();
try {
((ProceedingJoinPoint) joinPoint).proceed();
long end = System.currentTimeMillis();
log.info("around 通知 " + joinPoint + "\tUse time : " + (end - start) + " ms!");
} catch (Throwable e) {
long end = System.currentTimeMillis();
log.info("around 通知 " + joinPoint + "\tUse time : " + (end - start) + " ms with exception :
" + e.getMessage());
}
}

//配置后置返回通知,使用在方法 aspect()上注册的切入点
@AfterReturning("aspect()")
public void afterReturn(JoinPoint joinPoint){
log.info("afterReturn 通知 " + joinPoint);
}
//配置抛出异常后通知,使用在方法 aspect()上注册的切入点
@AfterThrowing(pointcut="aspect()", throwing="ex")
public void afterThrow(JoinPoint joinPoint, Exception ex){
log.info("afterThrow 通知 " + joinPoint + "\t" + ex.getMessage());
}
}

