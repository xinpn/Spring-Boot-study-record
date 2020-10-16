# Spring Boot 10.16头像上传

### 上传头像功能

- 请求为Post请求
- 前端form中加入 enctype="multipart/form-data"
- Spring MVC:通过MultipartFile处理上传文件
- **处理过程**：controller(/setting)接受前端传递的数据MultipartFile对象，将文件存入本地目录。更新用户的headUrl变量。controller(/head/{imagename})将本地图片写入到浏览器中。

代码实现：

```Java
/**
 * 上传头像,将图片写入到自己的指定位置
 * @return
 */
@RequestMapping(value = "/setting",method = RequestMethod.POST)
public String UserSetting(MultipartFile headImage, Model model){

    if (headImage==null){
        model.addAttribute("headMsg","请选择图片");
        return "/site/setting";
    }
    String imageName=headImage.getOriginalFilename();
    //图片后缀
    String suffix=imageName.substring(imageName.lastIndexOf("."));
    if (StringUtils.isBlank(suffix)){
        model.addAttribute("headMsg","图片格式不正确");
        return "/site/setting";
    }
    //生成新的随机文件名
    imageName= CommunityUtils.generateUUID()+suffix;
    File dest=new File(upLoadPath+"/"+imageName);
    try {
        headImage.transferTo(dest);
    } catch (IOException e) {
        e.printStackTrace();
    }

    //更新用户的headUrl
    User user=hostHolder.getUser();
    String hedaUrl=domain+contextPath+"/user/header/"+imageName;
    userService.upLoadHead(user.getId(),hedaUrl);

    return "redirect:/index";

}

@RequestMapping("/header/{imageName}")
public void getHeader(@PathVariable("imageName") String imageName, HttpServletResponse response){

    String filename=upLoadPath+"/"+imageName;
    String suffix=imageName.substring(imageName.lastIndexOf("."));
    //响应图片
    response.setContentType("image"+suffix);
    try (
            FileInputStream fis=new FileInputStream(filename);
            OutputStream os=response.getOutputStream();
            ){
        byte[] buffer = new byte[1024];
        int b = 0;
        while ((b = fis.read(buffer)) != -1) {
            os.write(buffer, 0, b);
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }
```