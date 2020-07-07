# JavaScript学习笔记

### 变量

1. 在函数外声明的变量是**全局变量**，网页上的所有脚本和函数都能访问它
2. 函数内部的局部变量会在函数运行后被删除，全局变量会在页面关闭后被删除。
3. 如果把值赋给未声明的变量，该变量自动作为window的一个属性，非严格模式下给未声明变量赋值创建的全局变量，是全局对象的可配置属性，可以删除。而正常声明过的属性是不可配置的，无法删除。
4. 如果变量在函数内并且没有声明，即为全局变量。
5. 全局变量是window对象，所有数据变量都属于window对象



### HTML DOM事件：

> https://www.runoob.com/jsref/dom-obj-event.html

1. 点击元素：`onclick`
2. 在一个元素上移动鼠标：`onmousever` 
3. 从一个元素上移开鼠标：`onmouseout` 
4. 按下键盘按键：`onkeydown`
5. 浏览器完成页面的加载：`onload`
6. 元素改变：`onchange`



### String与对象|typeof|===

1. 通过字符创建字符串，类型是`string`

2. 通过new来创建字符串，类型是`object`

3. 利用typeof 用来返回类型

4. string对象会拖慢速度，产生副作用

5. `===` 为绝对相等，数据类型和值都必须相等。

   ```js
   var x = "John";              // x 是字符串
   var y = new String("John");  // y 是一个对象
   document.getElementById("demo").innerHTML = x===y; //false
   document.getElementById("demo").innerHTML =typeof x +typeof y //string object
   ```



### 字符串属性和方法

> https://www.runoob.com/js/js-strings.html

1. 原始值字符串没有属性和方法（因为不是对象）

2. 但是原始值字符串可以使用jacascript的属性和方法

3. 字符串属性：

   | 属性        | 描述                       |
   | :---------- | :------------------------- |
   | constructor | 返回创建字符串属性的函数   |
   | length      | 返回字符串的长度           |
   | prototype   | 允许您向对象添加属性和方法 |

4. 字符串方法：https://www.runoob.com/jsref/jsref-obj-string.html



### for/in循环：

1. 循环遍历对象的==属性==：

   ```js
   var x;
   	var txt="";
   	var person={fname:"Bill",lname:"Gates",age:56}; 
   	for (x in person){
   		txt=txt + person[x];
   	}
   	document.getElementById("demo").innerHTML=txt;
   	//结果：
   	//BillGates56
   ```

   

   

   