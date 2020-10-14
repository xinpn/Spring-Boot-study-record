# Spring Boot 9.29

- 邮件发送

  ​	邮件发送用到的类：JavaMailSender(接口，用里面的send方法发送邮件，send的对象是MimeMessage)，MimeMessage, MimeMessageHelper(借助MimeMessageHelper来构建MimeMessage)。

  **开始发送邮件：**

  1. 设置邮箱（发送邮件的邮箱，即from），开启SMTP服务，获得授权码。
  2. 配置。在Spring Boot项目里进行配置，此处用的时候application.properties文件。

  ```properties
  #服务器地址
  spring.mail.host=smtp.163.com
  #端口号
  spring.mail.port=587
  #发送邮件的邮箱
  spring.mail.username=xin__p@163.com
  #授权码或密码
  spring.mail.password=VXDUKEHFKHMCKHRT
  #协议
  spring.mail.protocol=smtps
  spring.mail.properties.mail.smtp.ssl.enable=true
  spring.mail.properties.mail.smtp.auth=true
  spring.mail.properties.mail.smtp.starttls.enable=true
  spring.mail.properties.mail.smtp.starttls.required=true
  ```

  ​	3.主要代码

  ```java
  @Autowired
  private JavaMailSender mailSender;
  //获取邮箱
  @Value("${spring.mail.username}")
  private String from;
  
  public void SendMail(String to,String subject,String content){
      try {
          MimeMessage mimeMessage=mailSender.createMimeMessage();
          MimeMessageHelper mimeMessageHelper=new MimeMessageHelper(mimeMessage);
          //用MimeMessageHelper构建MimeMessage
          mimeMessageHelper.setSubject(subject);
          mimeMessageHelper.setText(content,true);
          mimeMessageHelper.setFrom(from);
          mimeMessageHelper.setTo(to);
          //发送
          mailSender.send(mimeMessage);
      } catch (MessagingException e) {
          e.printStackTrace();
      }
  }
  ```

- 前端复用

  ​	在前端中复用某一部分的代码，在被复用部分的标签里加入 th:fragment即可。

  ```html
  <!-- fragment里面的内容为自己定义-->
  <header class="bg-dark sticky-top" th:fragment="header">
  ```

  ​	在需要复用的标签里加入 th:replace替换；也可以用 th:insert插如。

  ```html
  <!-- repacle里的内容 index表示在哪个页面复用 header即为复用的部分 与fragment一致 -->
  <header class="bg-dark sticky-top" th:replace="index :: header">
  ```

- 发送Html邮件

  ​	用到的类 TemplateEngine（模板工具，调用procss方法，返回html页面的字符串），Context（向模板传递数据的类，作为TemplateEngine.procss()的一个参数）。

  ```java
  @Autowired
  private TemplateEngine templateEngine;
  @Test
  public void testHtmlMail(){
      Context context=new Context();
      //demo模板中需要一个叫username的参数，此处传递过去
      context.setVariable("username","xin");
  
      //返回html页面的字符串
      String content=templateEngine.process("/mail/demo",context);
  
      //content会被浏览器解析
      mailClient.SendMail("1977886864@qq.com","Hello HTML",content);
  }
  ```

