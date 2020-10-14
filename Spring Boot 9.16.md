# Sping Boot 9.16

- 程序报错

  ![image-20200916193306411](C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20200916193306411.png)

  解决方法：把密码写对！！！

- 连接不上数据库

  解决方法：

  ```java
  @SpringBootApplication(exclude= {DataSourceAutoConfiguration.class})
  ```

- 实现登陆页面跳转

  对于一个登陆表单，只需要指定他的动作(th:action="@{/user/login}")即可跳转到指定页面。

  ```html
  <form class="form-signin" th:action="@{/user/login}">
  ```

- 