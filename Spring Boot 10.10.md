# Spring Boot 10.10

- 代码中的service类记得加注解@Service

- 前端向后端传递数据时，要加上一个th:name,后端才能根据名称获取值；若前端向后端传递的值是某个对象里面的内容，也可以用一个对象接受。

- thymeleaf里的input标签

  - th:value="${}"显示表单的默认值

  - ```html
    <div class="form-group row">
       <label for="username" class="col-sm-2 col-form-label text-right">账号:</label>
       <div class="col-sm-10">
          <input type="text" th:class="|form-control ${usernameMsg!=null?'is-invalid':''}|" id="username"
                th:value="${user!=null?user.username:''}"
                placeholder="请输入您的账号!" required th:name="username">
          <div class="invalid-feedback" th:text="${usernameMsg}">
             该账号已存在!
          </div>
       </div>
    </div>
    ```

    要使第二个<div>标签的值显示，需要在input的标签中的class属性加入is-invalid。

    ```html
    th:class="|form-control ${usernameMsg!=null?'is-invalid':''}|"
    ```

- 邮件激活

  - 写一个接口，存放激活状态：CommunityConst
  - 在service中判断用户是否激活，返回激活状态
  - 根据激活状态，在controller中进行不同的页面跳转