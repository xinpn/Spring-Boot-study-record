# Spring Boot 10.11

### Spring Boot整合Kaptcha实现验证码功能

1. ##### 引入依赖

   引入kaptcha所需要的依赖，在https://mvnrepository.com/上搜索kaptcha。

   ```xml
   <dependency>
       <groupId>com.github.penggle</groupId>
       <artifactId>kaptcha</artifactId>
       <version>2.3.2</version>
   </dependency>
   
   ```

2. ##### 配置类

   用到的类或者接口：

   - **Producer**：次接口有两个方法Kaptcha：生成字符串，生成图片。
   - **DefaultKaptcha**：Producer的实现类。
   - **Properties，Config**：配置DefaultKaptcha对象。

   代码如下：

   ```java
   @Configuration
   public class KaptchaConfig {
   
       @Bean
       public DefaultKaptcha kaptchaProducer(){
   
           DefaultKaptcha defaultKaptcha=new DefaultKaptcha();
           Properties properties=new Properties();
           // 图片宽
           properties.setProperty("kaptcha.image.width", "100");
           // 图片高
           properties.setProperty("kaptcha.image.height", "40");
           // 字体大小
           properties.setProperty("kaptcha.textproducer.font.size", "32");
           //字体颜色
           properties.setProperty("kaptcha.textproducer.font.color", "0,0,0");
           // 验证码长度
           properties.setProperty("kaptcha.textproducer.char.length", "4");
           // 验证码内容
           properties.setProperty("kaptcha.textproducer.char.string", "0123456789QWERTYUIOPASDFGHJKLZXCVBNM");
           //设置噪音 即防破解
           properties.setProperty("kaptcha.noise.impl","com.google.code.kaptcha.impl.NoNoise");
           //Config构造函数需要一个Properties对象
           Config config =new Config(properties);
           defaultKaptcha.setConfig(config);
           return defaultKaptcha;
       }
   }
   ```

   1. 生成DefaultKaptcha对象，对这个对象进行配置，即defaultKaptcha.setConfig(config);
   2. 配置需要一个Config实例，所以在之前要定义一个Config对象。
   3. Config对象实例化需要一个Properties对象，所以在之前要定义。
   4. 对定义的Properties进行配置。

3. ##### Controller类

   设置一个Mapper路径，按路径可以访问到图片。

   ```java
   @RequestMapping("/kaptcha")
       public void getKaptcha(HttpServletResponse response, HttpSession session){
   
           String text=producer.createText();
           BufferedImage image=producer.createImage(text);
           //将字符串保存进session，方便验证
           session.setAttribute("kaptcha",text);
           //将图片输出到浏览器
           //设置输出格式
           response.setContentType("image/png");
           
           try {
               OutputStream os= response.getOutputStream();
               ImageIO.write(image,"png",os);
           } catch (IOException e) {
               e.printStackTrace();
           }
   
       }
   ```

4. ##### 在浏览器输入http://localhost:8080/community/kaptcha即可访问。

### 刷新验证码

在<img>里面设置id，src。在href里设置函数。

```html
<div class="col-sm-4">
   <img th:src="@{/kaptcha}" id="kaptcha" style="width:100px;height:40px;" class="mr-2"/>
   <a href="javascript:refresh_kaptcha();" class="font-size-12 align-bottom">刷新验证码</a>
</div>
```

完成refresh_kaptcha()函数。

```html
<script>
   function refresh_kaptcha(){
      var path=CONTEXT_PATH+"/kaptcha?p="+Math.random();
      ${"#kaptcha"}.attr("src",path);
   }
</script>
```