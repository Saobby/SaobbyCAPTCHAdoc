# SaobbyCAPTCHA文档
## 简介
SaobbyCAPTCHA是一个**人机验证**服务，它可以准确识别机器人爬虫，不让烦人的机器人程序给你的网络表单提交垃圾信息。 

![SaobbyCAPTCHA截图](https://s.saobby.cf/i/saobbycaptcha/p0.PNG)  

使用SaobbyCAPTCHA较为简单，您只需要稍稍修改一下您的前端代码，添加验证码的窗口和JS代码，并在后端验证前端传来的密钥即可。  
您可以去[SaobbyCAPTCHA主页](https://captcha.saobby.cf/)查看示例，如果您觉得我做的验证码窗口太丑(事实上大部分人都是这么认为的)，您也可以利用下文中的API自己做一个验证码窗口。
## API文档
### 获取验证码图片(前端使用)
#### 请求
* API URL: **`https://captcha.saobby.cf/api/get_image`**
* 请求方式: **`POST`**
* 请求载荷: **`无`**
#### 响应
* 响应数据类型: **`JSON`**
* 响应包含参数: 
  * **`captcha_id`**: 类型:**`字符串`**: 验证码唯一ID,获取验证码token时会用到
  * **`captcha_img`**: 类型:**`字符串`**: 验证码图片Base64值,可直接塞进`<img>`标签的`src`属性中
  * **`captcha_lens`**: 类型:**`整型数`**: 验证码的字数
* 响应示例: 
  *  {
  *    "captcha_id":"GBXiOvIfJ30Kn9Stn0H5DxCgT6d3aVTAXysMhF9D91n7NGDQyRUcf3t1CYRE4leu", 
  *    "captcha_img":"data:image/png;base64,iVBORw0KGgoAAAAN......",
  *    "captcha_lens":5
  *  }
##### 注意:每张验证码图片的有效期只有5分钟,若5分钟内未完成验证码,则该验证码作废
### 获取验证码密钥(前端使用)
#### 请求
* API URL: **`https://captcha.saobby.cf/api/get_token`**
* 请求方式: **`POST`**
* 请求载荷类型: **`表单数据`**
* 请求载荷参数: 
  * **`id`**: 上文中API返回的验证码的唯一ID
  * **`pos`**: 用户点击坐标(图片**左上角**为坐标原点,坐标1单位长度对应图片中1像素),格式:**`第1个字的x坐标,第1个字的y坐标,第2个字的x坐标,第2个字的y坐标,...第n个字的x坐标,第n个字的y坐标,`**(注意每个坐标值之间用英文逗号隔开)
* 请求载荷示例: `id=bMeH04C5aXb5v2JpDwzQrA7AM7iOYlSJ1tQJNoX7WaKKIKqRHUHfiLX0usHPsfTq&pos=218,105,155,115,117,137,160,182,214,152,245,143,245,65,`
#### 响应
* 响应数据类型: **`JSON`**
* 响应包含参数: 
  * **`validity`**: 类型: **`布尔`**: 传来的信息是否有效?有效为`true`,否则为`false`
  * **`token`**: 类型:**`字符串`**: 验证码密钥
  * **`message`**: 类型:**`字符串`**: 展示给用户的消息
* 响应示例: 
  * {
  *   "validity": false, 
  *   "message": "\u9a8c\u8bc1\u7801\u9519\u8bef", 
  *   "token": null
  * }
##### 注意:**每个验证码只有一次验证机会(不论对了还是错了)!**
##### 注意:**每个密钥的有效时间只有10分钟,超时即作废**
### 检查验证码密钥是否有效(后端使用)
* API URL: **`https://captcha.saobby.cf/api/check_token`**
* 请求方式: **`POST`**
* 请求载荷类型: **`表单数据`**
* 请求载荷参数: 
  * **`s-captcha-token`**: 前端传来的验证码密钥
* 请求载荷示例: `s-captcha-token=ovr7XEpDMlYE8j6svwJueqJHna1BV9ozwWdjA1WI1OzwPBQb2uOnIgu29LiJekr5`
#### 响应
* 响应数据类型: **`JSON`**
* 响应包含参数: 
  * **`validity`**: 类型: **`布尔`**: 传来的密钥是否有效?有效为`true`,否则为`false`
  * **`message`**: 类型:**`字符串`**: 消息
* 响应示例:
  * {
  *   "validity": true, 
  *   "message": null
  * }
##### 注意:每个密钥都是一次性的,验证过一次之后立即作废
