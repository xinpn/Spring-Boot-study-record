# Spring Boot 10.18使用注解检查登陆状态

### 对于一个系统来说，有一些功能需要用户登录之后才能访问，所有在一些功能上需要检查用户是否登陆。

- 新建一个注解，规定注解作用的时间和地点（方法还是类）
- 设置拦截器，在访问controller之前检查方法是否有该注解。如有该注解，检查用户是否登陆。
- 配置拦截器(WebMvcConfig类)。

1. 新建一个com.example下新建一个包Annotation，新建注解LoginRequired。

   ```Java
   package com.example.Annotation;
   
   import java.lang.annotation.ElementType;
   import java.lang.annotation.Retention;
   import java.lang.annotation.RetentionPolicy;
   import java.lang.annotation.Target;
   
   @Target(ElementType.METHOD)
   @Retention(RetentionPolicy.RUNTIME)
   public @interface LoginRequired{
       
   }
   ```

2. 新建拦截器LoginRequiredAnnotation

   ```java
   package com.example.Service.Interceptor;
   
   import com.example.Annotation.LoginRequired;
   import com.example.Util.HostHolder;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Component;
   import org.springframework.web.method.HandlerMethod;
   import org.springframework.web.servlet.HandlerInterceptor;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.lang.reflect.Method;
   
   @Component
   public class LoginRequiredInterceptor implements HandlerInterceptor {
   
       @Autowired
       private HostHolder hostHolder;
   
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           //如果拦截的是方法
           if(handler instanceof HandlerMethod){
               HandlerMethod handlerMethod=(HandlerMethod) handler;
               Method method=handlerMethod.getMethod();
               LoginRequired loginRequired=method.getAnnotation(LoginRequired.class);
               if(loginRequired!=null&&hostHolder.getUser()==null){
                   response.sendRedirect(request.getContextPath()+"/login");
                   return false;
               }
           }
           return true;
       }
   }
   ```

3. 配置

   ```Java
   registry.addInterceptor(loginRequiredInterceptor)
           .excludePathPatterns("/**/*.css", "/**/*.js", "/**/*.png", "/**/*.jpg", "/**/*.jpeg");
   ```

### 