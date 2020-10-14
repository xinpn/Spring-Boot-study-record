# Sing Boot 9.19

- 数据库连接问题

![image-20200919164219862](C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20200919164219862.png)

解决方法：@SpringBootApplication(exclude= {DataSourceAutoConfiguration.class})	

​	排除这个类的自动配置。