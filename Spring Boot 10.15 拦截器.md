# Spring Boot 10.15 拦截器

##### 	在实现用户登录，保持用户状态时需要用到拦截器。实现拦截器需要：

1. 实现HandlerInterceptor接口，或继承HandlerInterceptor的实现类。
2. 实现WebMvcConfigurer接口，或继承其实现类。

- HandlerInterceptor接口有三个方法：default boolean preHandle() 返回为false时不在向下执行;default void postHandle();default void afterCompletion.这三个方法分别在请求controller之前，请求controller之后，渲染视图完成之后调用。

- 实现WebMvcConfigurer接口，**这个类是拦截器的配置类**，重写addInterceptors(InterceptorRegistry registry)方法。添加需要拦截的路径。

  ##### 完成用户登陆并保持用户信息

- 保持对象：使用ThreadLocal类。

  ThreadLocal而是一个线程内部的存储类，可以在指定线程内存储数据，数据存储以后，只有指定线程可以得到存储数据。ThreadLocal提供了线程内存储变量的能力，这些变量不同之处在于每一个线程读取的变量是对应的互相独立的。

  1. **set()方法**：将用户user存入线程中。
  2. **get()方法**：获取存入的变量，即user。
  3. **remove()方法**：将线程变量清空。

- 在用户访问所有页面时都要检查当前用户是谁，是否已经登陆，所以就要用到拦截器的**preHandle方法**。

  用户的标识是用户登陆产生的ticket（存放在loginTicket对象中，设置好了expired），ticket存放在cookie中，访问页面之前通过HttpServletRequest对象获取ticket，并查询到持有此ticket的对象。使用ThreadLocal的set()方法将用户存入线程变量。

- 前端需要用到当前用户的信息来显示时，在postHandle()中，使用ThreadLocal的get()方法获取当前用户，将获取到的用户user存入到modelAndView对象中，就可以在前端使用了。

- 渲染视图结束之后，需要afterCompletion()方法中调用ThreadLocal的remove()方法。