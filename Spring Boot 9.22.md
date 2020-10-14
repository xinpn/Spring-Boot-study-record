# Spring Boot 9.22

- 接口
  1. 接口用来描述类应该做什么，而不指定他们具体应该怎么做
  2. 接口中的所有方法都自动是**public**方法。因此，在接口中声明方法时，不必提供关键字public。

- **Spring** **boot**使用**mybatis**访问数据库时，数据访问只需要写接口（在mapper或者dao的包下）。
- Mybatis使用的.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="comcom.example.Mapper.UserMapper">
    
</mapper>
```

这里面的namespace属性表明这个xml文件是为哪个mapper服务的。要使用comcom.example.Mapper.UserMapper，而不能用comcom/example/Mapper/UserMapper