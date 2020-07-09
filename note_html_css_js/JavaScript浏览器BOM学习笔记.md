# JavaScript浏览器BOM

### Window浏览器对象模型

1. 所有浏览器都支持 window 对象。它表示浏览器窗口。

   所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。

2. window的尺寸：(三种方法)

   ```js
   var w=window.innerWidth
   || document.documentElement.clientWidth
   || document.body.clientWidth;
   
   var h=window.innerHeight
   || document.documentElement.clientHeight
   || document.body.clientHeight;
   ```

3. 其他window的方法：

   - window.open() - 打开新窗口
   - window.close() - 关闭当前窗口
   - window.moveTo() - 移动当前窗口
   - window.resizeTo() - 调整当前窗口的尺寸



### Window Screen

1. window.screen 对象包含有关用户屏幕的信息。**window.screen**对象在编写时可以不使用 window 这个前缀
2. 属性：
   - screen.availWidth - 可用的屏幕宽度
   - screen.availHeight - 可用的屏幕高度



### Window Location

1. window.location 对象用于获得当前页面的地址 (URL)，并把浏览器重定向到新的页面

2. 属性：

   - location.hostname 返回 web 主机的域名
   - location.pathname 返回当前页面的路径和文件名
   - location.port 返回 web 主机的端口 （80 或 443）
   - location.protocol 返回所使用的 web 协议（http: 或 https:）
   - location.href返回（当前页面的）整个 URL

3. 用`assign()` 方法加载新的文档

   ```js
   window.location.assign("https://www.runoob.com")
   ```



### Window History

1. 包含浏览器的历史

2. 方法：

   - history.back() - 与在浏览器点击后退按钮相同
   - history.forward() - 与在浏览器中点击向前按钮相同

   ```js
   function goForward()
   {
       window.history.forward()
   }
   ```



### Window Navigator

1. 包含有关访问者浏览器的信息

2. 属性使用如下：

   ```js
   <script>
   txt = "<p>浏览器代号: " + navigator.appCodeName + "</p>";
   txt+= "<p>浏览器名称: " + navigator.appName + "</p>";
   txt+= "<p>浏览器版本: " + navigator.appVersion + "</p>";
   txt+= "<p>启用Cookies: " + navigator.cookieEnabled + "</p>";
   txt+= "<p>硬件平台: " + navigator.platform + "</p>";
   txt+= "<p>用户代理: " + navigator.userAgent + "</p>";
   txt+= "<p>用户代理语言: " + navigator.systemLanguage + "</p>";
   document.getElementById("example").innerHTML=txt;
   </script>
   ```

3. 但是！来自 navigator 对象的信息具有误导性，不应该被用于检测浏览器版本！



### 弹窗

1. 警告框

   ```js
   window.alert("sometext");
   ```

2. 确认框：

   ```js
   window.confirm("sometext");
   ```

3. 提示框；

   `window.confirm("*sometext*");`

   ```js
   var person=prompt("请输入你的名字","Harry Potter");
   if (person!=null && person!="")
   {
       x="你好 " + person + "! 今天感觉如何?";
       document.getElementById("demo").innerHTML=x;
   }
   ```

4. 弹窗中的换行：`\n`



### 计时事件

1. HTML DOM Window对象的两个计时方法：
   - setInterval() - 间隔指定的毫秒数不停地执行指定的代码。
   - setTimeout() - 在指定的毫秒数后执行指定代码。

2. `setInterval()`方法：

   `window.setInterval("javascript function",milliseconds);`

   ```js
   setInterval(function(){alert("Hello")},3000);
   
   <script>
   var myVar=setInterval(function(){myTimer()},1000);
   function myTimer(){
   	var d=new Date();
   	var t=d.toLocaleTimeString();
   	document.getElementById("demo").innerHTML=t;
   }
   </script>
   ```

3. clearInterval() 方法用于停止 setInterval() 方法执行的函数代码。

   `window.clearInterval(intervalVariable)`

   **set方法中必须使用全局变量**

   ```js
   <p id="demo"></p>
   <button onclick="myStopFunction()">停止</button>
   <script>
   var myVar=setInterval(function(){myTimer()},1000);
   function myTimer(){
       var d=new Date();
       var t=d.toLocaleTimeString();
       document.getElementById("demo").innerHTML=t;
   }
   function myStopFunction(){
       clearInterval(myVar);
   }
   </script>
   ```

4. `setTimeOut()`方法：

   `myVar= window.setTimeout("javascript function", milliseconds);`

   ```js
   setTimeout(function(){alert("Hello")},3000);
   ```

5. clearTimeout() 方法用于停止执行setTimeout()方法的函数代码。

   `window.clearTimeout(timeoutVariable)`

   **set方法中必须使用全局变量**

   ```js
   var myVar;
    
   function myFunction()
   {
       myVar=setTimeout(function(){alert("Hello")},3000);
   }
    
   function myStopFunction()
   {
       clearTimeout(myVar);
   }
   ```

   

### Cookie

1. Cookie 是一些数据, 存储于你电脑上的文本文件中。

   当 web 服务器向浏览器发送 web 页面时，在连接关闭后，服务端不会记录用户的信息。

   - 当用户访问 web 页面时，他的名字可以记录在 cookie 中。
   - 在用户下一次访问该页面时，可以在 cookie 中读取用户访问记录。

2. Cookie 以名/值对形式存储

   `username=John Doe`

3. 使用js创建cookie

   ```js
   document.cookie="username=John Doe; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";  //path表示路径，告诉浏览器cookie的路径
   ```

4. 用js读取cookie:

   `var x = document.cookie;`

   将以字符串的方式返回所有的 cookie `cookie1=value; cookie2=value;`

5. 用js删除cookie:

   `document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT";`

   设置 expires 参数为以前的时间即可

6. 设置、获取、检测cookie的函数：

   ```js
   function setCookie(cname,cvalue,exdays){
       var d = new Date();
       d.setTime(d.getTime()+(exdays*24*60*60*1000));
       var expires = "expires="+d.toGMTString();
       document.cookie = cname+"="+cvalue+"; "+expires;
   }
   function getCookie(cname){
       var name = cname + "=";
       var ca = document.cookie.split(';');
       for(var i=0; i<ca.length; i++) {
           var c = ca[i].trim();
           if (c.indexOf(name)==0) { return c.substring(name.length,c.length); }
       }
       return "";
   }
   function checkCookie(){
       var user=getCookie("username");
       if (user!=""){
           alert("欢迎 " + user + " 再次访问");
       }
       else {
           user = prompt("请输入你的名字:","");
             if (user!="" && user!=null){
               setCookie("username",user,30);
           }
       }
   }
   ```

   



