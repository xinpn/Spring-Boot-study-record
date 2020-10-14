# Spring Boot 10.12

- 开发顺序：数据访问层--->业务层

- **${}与#{}取值的区别：i**
  - **#{}**取值是取自己定义的实体类中的值，比如LoginTicket中有属性ticket,可以用#{ticket}获取。
  - **${}**取值取的是配置文件里的值，即application.properties里。
  
- 数据访问层(Mapper)的另一种写法：

  ​	直接在mapper类的方法上添加注解，格式@Insert(),@Update等。在注解里写SQL语句。如下：

  ```java
  @Select({
          "select id,user_id,ticket,status,expired",
          "from login_ticket where ticket=#{ticket}"
  })
  LoginTicket selectByTicket(String ticket);
  
  @Update({
          "update login_ticket set status=#{status} where ticket=#{ticket}"
  })
  int updateStatus(String ticket,int status);
  ```

  ​	**注意：**在注解里写SQL语句时要加{}，语句可以分割，在每段语句后加，隔开。

  设置主键自增：使用@Options注解。

  ```java
  @Insert({
          "insert into login_ticket (user_id,ticket,status,expired) ",
          "values(#{userId},#{ticket},#{status},#{expired})"
  })
  //设置主键自增，keyProperty为主键
  @Options(useGeneratedKeys = true,keyProperty = "id")
  int insertLoginTicket(LoginTicket loginTicket);
  ```

  ​	