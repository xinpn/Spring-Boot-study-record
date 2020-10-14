# Spring Boot 9.23

- 关于mapper的xml配置

  1. 选择语句

  ```xml
  <select id="selectById" resultType="User">
      select <include refid="selectFields"></include>
      from user
      where id = #{id}
  </select>
  ```

  ​	id为对应mapper类的方法名，表明这个select区域为该方法服务；resultType为该方法的返回值，如果是比较简单的**int,String**类型，可以省略。

  ​	select标签中间即为sql语句。

  ```xml
  <include refid="selectFields"></include>表示此部分替代selectFields域的代码。
  在select部分前面定义了：
  <sql id="selectFields">
      id,username,password,salt,email,status,activationcode,headurl,create_time
  </sql>
  所以此处的代码就相当于：
  <select id="selectById" resultType="User">
      select	id,username,password,salt,email,status,activationcode,headurl,create_time
      from user
      where id = #{id}
  </select>
  ```

  **提示：**对于所有的select语句都要写resultType属性！！！！

  ​	2.insert语句
  
  ```xml
  <insert id="insertUser" parameterType="User" keyProperty="id">
      insert into user(<include refid="insertFields"/>)
      values (#{username},#{password},#{salt},#{email},#{type},#{status},#{activationCode},#{headUrl},#{createTime})
</insert>
  ```

  ​	parameterType表示所对应方法的（即id指定的insertUser方法）参数类型。keyProperty表明数据库表的主键，即id，插入数据时，id一般不插入且自增。

  ​	3.update语句
  
  ```xml
  <update id="updateStatus" >
      update user set status=#{status} where id=#{id}
</update>
  ```
  
  **提示：**在xml中写sql语句时，#{}中的变量是函数传递进来的参数，外面的是数据库表的字段。
  
  