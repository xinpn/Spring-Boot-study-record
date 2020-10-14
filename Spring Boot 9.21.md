# Spring Boot 9.21

- 前端表单提交之**Get**方法和**Post**方法

  ​		在前端的html页面向后台提交数据时默认使用**Get**方法，这种方法的缺点是提交数据之后数据会以参数的形式显示在url中，不安全；而且url的长度有限，数据太多是可能出现错误。**Post方法**是更安全的方法，使用Post方法参数会提交给后台，不显示在url中，因此提交数据时应该使用**Post方法**。

  ​		使用**Post方法**：

  1. 前端表单中**应该使用method指定**（默认方法是Get）。

  ```html
  <form class="form-signin" th:action="@{/user/login}" th:method="Post">
  ```

  ​	2.对应的Controller类中的@RequestMapping注解也应该进行指定。

  ```java
  @RequestMapping(value = "/user/login",method = RequestMethod.POST)
  ```

  ​	或者直接使用@PostMapping注解。