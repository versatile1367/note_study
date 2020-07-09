# JS HTML DOM学习笔记

> HTML DOM 教程：https://www.runoob.com/htmldom/htmldom-tutorial.html

### DOM

1. DOM为文档对象模型：Document Object Model

2. 通过id,标签名,类名来查找html元素

   ```js
   var x=document.getElementById("intro");
   var x=document.getElementById("main");
   var y=x.getElementsByTagName("p");
   var x=document.getElementsByClassName("intro");
   ```



### 改变html

```
//改变输出流
document.write(Date());
//改变内容
document.getElementById("p1").innerHTML="新文本!";
//改变属性
document.getElementById("image").src="landscape.jpg";
```



### 改变css

```js
//改变html样式
document.getElementById("p2").style.color="blue";
document.getElementById("p2").style.fontFamily="Arial";
document.getElementById("p2").style.fontSize="larger";
//使用事件
<button type="button"
onclick="document.getElementById('id1').style.color='red'">
```



### DOM事件

1. 点击时可以直接改变或者调用一个函数：

   ```js
   //直接改变
   <h1 onclick="this.innerHTML='Ooops!'">点击文本!</h1>
   //通过调用函数
   <script>
   function changetext(id)
   {
       id.innerHTML="Ooops!";
   }
   </script>
   <h1 onclick="changetext(this)">点击文本!</h1>
   ```

2. `onload` 和`onunload` 事件：

   onload 和 onunload 事件会在用户进入或离开页面时被触发。onload 事件可用于检测访问者的浏览器类型和浏览器版本，并基于这些信息来加载网页的正确版本。

   onload 和 onunload 事件可用于**处理 cookie**。

   ```js
   <body onload="checkCookies()">
   ```

3. `onchange` 事件

   结合对输入字段的验证来使用，当用户改变输入字段的内容时，会调用函数

   ```js
   <input type="text" id="fname" onchange="upperCase()">
   ```

4. `onnouseover` 和`onmouseout` 事件：

   可用于在用户的鼠标移至 HTML 元素上方或移出元素时触发函数

5. `onmousedown` ,`onmouseup` ,`onclick` 事件：

   onmousedown, onmouseup 以及 onclick 构成了鼠标点击事件的所有部分。首先当点击鼠标按钮时，会触发 **onmousedown** 事件，当释放鼠标按钮时，会触发 **onmouseup** 事件，最后，当完成鼠标点击时，会触发 **onclick** 事件。

6. `onfocus` 事件；

   当输入字段获得焦点时，改变其背景色。



### EventListener

> HTML DOM事件：runoob.com/jsref/dom-obj-event.html

1. `addEventListener()` 方法

   - 用于向指定元素添加事件句柄，不会覆盖已存在的事件句柄。可以向一个元素**添加多个事件句柄**，可以向同个元素添加**多个同类型**的事件句柄，如：两个 "click" 事件，可以向**任何 DOM 对象**添加事件监听，不仅仅是 HTML 元素。如： window 对象。

   - 使用 addEventListener() 方法时, **JavaScript 从 HTML 标记中分离开来**，可读性更强， 在没有控制HTML标记时也可以添加事件监听

   - 可以使用 removeEventListener() 方法来移除事件的监听

     ```js
     <script>
     document.getElementById("myBtn").addEventListener("click", displayDate);
     function displayDate() {
         document.getElementById("demo").innerHTML = Date();
     }
     </script>
     ```

   - 语法：

     ```js
     element.addEventListener(event, function, useCapture);
     ```

     第一个参数是事件的类型 (如 "click" 或 "mousedown").使用 "click" ,而不是使用 "onclick"

     第二个参数是事件触发后调用的函数。

     第三个参数是个布尔值用于描述事件是冒泡还是捕获。该参数是可选的。

2. 可以向window对象添加时间句柄：

   ```js
   window.addEventListener("resize", function(){
       document.getElementById("demo").innerHTML = sometext;
   });
   ```

3. 传递参数：

   利用“匿名函数“调用带参数的函数：

   ```js
   element.addEventListener("click", function(){ myFunction(p1, p2); });
   ```

4. 事件冒泡或事件捕获：

   在 **冒泡** 中，内部元素的事件会先被触发，然后再触发外部元素;在 **捕获** 中，外部元素的事件会先被触发

   默认值为 false, 即冒泡传递，当值为 true 时, 事件使用捕获传递。

   ```js
   document.getElementById("myDiv").addEventListener("click", myFunction, true);
   ```

5. 移除`addEventListener` 添加的事件句柄：

   ```js
   element.removeEventListener("mousemove", myFunction);
   ```

   

### 添加和移除DOM元素（节点）

1. `appendChild` :要将元素添加到尾部

   要创建新的HTML元素需要先创建一个元素，然后在已存在的元素中添加它。

   ```js
   <script>
   var para = document.createElement("p");
   var node = document.createTextNode("这是一个新的段落。");
   para.appendChild(node);
    
   var element = document.getElementById("div1");
   element.appendChild(para);
   </script>
   ```

2. `insertBefore()`:要将元素添加到开始的位置

   ```js
   <div id="div1">
   <p id="p1">这是一个段落。</p>
   <p id="p2">这是另外一个段落。</p>
   </div>
    
   <script>
   var para = document.createElement("p");
   var node = document.createTextNode("这是一个新的段落。");
   para.appendChild(node);
    
   var element = document.getElementById("div1");
   var child = document.getElementById("p1");
   element.insertBefore(para, child);
   </script>
   ```

3. 移除已存在的元素，需要知道该元素的父元素：

   ```js
   <div id="div1">
   <p id="p1">这是一个段落。</p>
   <p id="p2">这是另外一个段落。</p>
   </div>
    
   <script>
   var parent = document.getElementById("div1");
   var child = document.getElementById("p1");
   parent.removeChild(child);
   </script>
   ```

4. 替换元素：

   ```js
   <div id="div1">
   <p id="p1">这是一个段落。</p>
   <p id="p2">这是另外一个段落。</p>
   </div>
    
   <script>
   var para = document.createElement("p");
   var node = document.createTextNode("这是一个新的段落。");
   para.appendChild(node);
    
   var parent = document.getElementById("div1");
   var child = document.getElementById("p1");
   parent.replaceChild(para, child);
   </script>
   ```



### HTMLCollection

1. 例如`getElementsByName()` 方法返回HTMLCollection对象。类似包含HTML元素的一个数组

   ```js
   var x = document.getElementsByTagName("p");/获取文档所有的 <p> 元素
   y = x[1];
   ```

2. `HTMLCollection` 对象的`length` **属性**定义了集合中元素的数量：

   ```js
   var myCollection = document.getElementsByTagName("p");
   document.getElementById("demo").innerHTML = myCollection.length;
   ```

3. **HTMLCollection 不是一个数组！**HTMLCollection 看起来可能是一个数组，但其实不是。

   可以像数组一样，**使用索引**来获取元素,但是HTMLCollection 无法使用数组的方法： valueOf(), pop(), push(), 或 join() 。



### NodeList

1. 类似HTMLCollection对象

2. 所有浏览器的 **childNodes** 属性返回的是 NodeList 对象。

   大部分浏览器的 **querySelectorAll()** 返回 NodeList 对象。

3. `HTMLCollection` 和`NodeList` 的区别；

   - HTMLCollection是 HTML 元素的集合。NodeList 是一个文档节点的集合。
   - HTMLCollection 元素可以通过 name，id 或索引来获取。NodeList **只能通过索引**来获取。
   - 只有 NodeList 对象有包含属性节点和文本节点。

4. 相似点：

   - 可以使用索引 (0, 1, 2, 3, 4, ...) 来获取元素。
   - 都有 length 属性

   

