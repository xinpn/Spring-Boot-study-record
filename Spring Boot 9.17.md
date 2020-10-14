# Sping Boot 9.17

- Html中的一些属性

  - ```html
    <input type="text" name="email" />
    ```

    input中的name属性用于对提交到服务器后的表单数据进行标识，或者在客户端通过 JavaScript 引用表单数据。服务器可以通过name属性获取表单域的值。

  - action属性

    表单中的action属性指定表单提交后的动作。

    ```html
    <form class="form-signin" th:action="@{/user/login}">
    ```

    ​	表单提交后跳转到/user/login页面。

- Sping Boot后台获取前端传递的元素

  在Controller类的参数中用@RequestParam指定要获取的变量名即可。

  ```java
  public String Login(
  					@RequestParam("email") String email,
  					@RequestParam("password") String passwd){}
  ```

  @RequestParam中的参数与前端的name属性一致。String email,String passwd即为在前端中获取的值。