# Spring Boot 9.28

- 分页相关

  ​		先写有关分页的实体类，即Entity下的Pages类。类中的属性与分页相关，如数据总行数，每页数据条数，访问路径，当前页码。其他需要的数据由**Get**方法计算得出，如当前页的第一条数据的编号，总页数（数据总行数/每页数据条数，即rows/limit),页码的范围（即前端页面显示时页码范围，从from到to,![image-20200928144210544](C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20200928144210544.png)即此处的1到3）。

- 前端模板thymeleaf
  - @{} 所需值是链接时使用
  - ${} 存放变量 可以在@{}里嵌套
  - #numbers.sequence(from,to)  生成一个从from到to的数组
  - | |里面表示静态数据，可以嵌套变量 

  ```html
  <li th:class="|page-item ${pages.current==pages.total?'disabled':''}|">
  ```

- 前端访问数据

  ​		前端页面接受Model中的数据，如map,pages。访问其中的属性时用**.**来访问，如pages.path,可以访问到pages对象的path属性，它的实质是调用pages的**getPath()**方法。对于Pages类中没有定义的属性，却有get方法的，也可以访问，比如pages.total，返回的是pages.getTotal()的返回值。