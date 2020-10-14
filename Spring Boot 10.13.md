# Spring Boot 10.13

### 前后端交互传递数据：Model

##### 	可以用Model类的实例实现后端向前端的数据传递。

1. ​	用model.addAttribute()向model中添加属性，前端中直接使用
2. 前后端交互的函数中，函数参数有Model，和另一个类（如User类）。Model会自动把User装入，在前端可以用User.username来访问User中的值。
3. 前后端交互的函数中，函数参数有Model，其他参数为基本类型。有两种方法可以完成在前端的访问。
   - ​	把参数用model.addAttribute()函数添加到model中。
   - 在前段中直接用param.username访问。这里实质上是用的request.getparam("username")来完成访问。