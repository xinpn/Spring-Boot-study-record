# Spring Boot 10.14

1. 浏览器传递用户名，密码，验证码给服务器
2. 服务器端的controller接受参数（用户名，密码，验证码）
3. controller处理验证码，在session中获取验证码，与得到的参数对比
4. controller将用户名，密码传递给service。
5. service验证用户名，密码是否正确，是否存在。
6. 通过验证后生成ticket，存入Loginticket类的实例中，返回给controller
7. controller中，将ticket存入cookie
8. 设置拦截器，在访问前从cookie中获取ticket，查询数据库，得到当前用户
9. 使用threadlocal保持当前用户
10. 在postHandle中获取用户，传递给前端模板，在前段能够访问当前用户