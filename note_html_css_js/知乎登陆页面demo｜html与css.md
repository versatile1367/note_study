# 知乎登陆页面demo

第一次接触写前端页面，所以进行的比较慢，但是在写的过程中巩固了一下所学的html,css的知识。

代码:https://github.com/versatile1367/zhihu_login_demo

一些需要记录的知识记录在了笔记中.



### 问题解决

1. 对于知乎的logo无法居中：

    知乎的logo没有嵌套在专门的为这个logo服务的div块中，直接在主体块中以img来显示，对于这个img的居中，尝试了`text-align:center;`,和`margin:0 auto;`均不可以。

    **最终解决**：

   ```css
       position: relative;
       left:50%;
       top:100px;
   ```

   **或者：**

   将图片放入div块中，放入div块中，有两种方法可以居中：

   ```html
   <div id="block-logo">
   <img id="img-logo" src="img/logo_zhihu.png" />
   </div>
   ```

   - 方法一：

     ```css
     /*以下是在css中*/
     #block-logo{
         text-align:center;
     }
     
     #img-logo{
         width:128px;
         height:81px;
         margin-bottom: 24px;
     }
     ```

   - 方法二：

     ```css
     #block-logo{
         width:128px;
         margin:0 auto;
     }
     
     #img-logo{
         width:128px;
         height:81px;
         margin-bottom: 24px;
     }
     ```

2. 背景图片的填满:

   刚开始调试时,没有将浏览器放大,背景图片还可以占满浏览器界面,当放大时才发现背景图片并没有随着浏览器放大,而是只占了一小部分界面.

   对于背景图片的设置,应该:

   - 将位置固定
   - 按比例缩放

   ```css
   .root{
       background: url("img/backgroud.png");
       background-repeat: no-repeat;
       background-size: cover;
       background-attachment: fixed;
       background-color: #b8e5f8;
   }
   ```

3. 对于背景颜色和背景图片顺序问题：

   当在一个模块的css中，先设置背景颜色再设置背景图片时，背景颜色会被覆盖，不会显示；

   但是先设置背景图片，再设置背景颜色，会把图片中留白填充，作为背景颜色，同时图片也显示。

4. 对于行内添加图标记得要添加对齐方式：

   利用`vertical-align` 这个属性来设置行内元素盒模型与其行内元素容器垂直对齐方式

5. 在登陆框中要实现两个`button`模块中间的竖线:

   本来想使用让第一个`button`块的`border-right` ,但是发现,因为左右两个div块的格式相近,无法一起调整两个`button` 块中的`margin`和`padding`使得二者一致,并且让竖线居中.同时还要让button的点击的`curser`图标变化不覆盖整个区域,只在居中的位置变化,所以最终另外使用一个`div`块单独实现竖线.

   `<div class="vertical-line"></div>`

   ```css
   .vertical-line{
       box-sizing: border-box;
       width:1px;
       height:25px;
       padding:0;
       margin:0 auto;
       position: absolute;;
       left:196px;
       background-color: #cccccf;
       margin-top:16px;
   }
   ```

5. 对于定位问题:

   ​        在实现主要的登陆框中,把整个框拆分成了从上至下的五个部分,对于定位问题,刚开始使用absolute定位,发现下面的两个部分并没有显示,出现在了登陆框外侧.

   ​       解决:

   ​       改用`relative`定位,`relative`和`absolute`定位的一个主要区别在于是否还占有原来的文档流.`absolute` 会被移出正常的文档流,在父元素中,并没有提前设置`height` 来预留整个框的高度,所以使用`relative` 来进行排版.

   ​      