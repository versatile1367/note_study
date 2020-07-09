# JavaScript入门学习笔记

因为已经学习过c/c++,java.所以仅记录一些重点、不熟练、容易遗漏的点。

> 学习教程地址：https://www.runoob.com/js/js-tutorial.html
>
> js推荐使用驼峰法



## js基础

### 变量与作用域

1. 在函数外声明的变量是**全局变量**，网页上的**所有脚本和函数**都能访问它

2. 函数内部的局部变量会在函数运行后被删除，全局变量会在页面关闭后被删除。

3. 如果把值赋给未声明的变量，该变量自动作为window的一个属性，非严格模式下给未声明变量赋值创建的全局变量，是全局对象的可配置属性，可以删除。而正常声明过的属性是不可配置的，无法删除。

4. 如果变量在函数内并且没有声明，即为全局变量。

5. 全局变量是window对象，所有数据变量都属于window对象

6. 变量可以在使用后声明，也就是**先使用再声明**。因为函数及变量的声明都将被提升到函数的最顶部。

7. 只用声明会提升，**初始化不会提升**！

8. 在每个代码块中 JavaScript 不会创建一个新的作用域，一般各个代码块的作用域都是全局的。

   ```js
   for (var i = 0; i < 10; i++) {
       // some code
   }
   return i;  //返回10
   ```
   
9. 变量声明时如果不使用`var` 关键字，那么它就是一个全局变量，即使它在函数内定义。

   

### HTML DOM事件：

> https://www.runoob.com/jsref/dom-obj-event.html

1. 点击元素：`onclick`
2. 在一个元素上移动鼠标：`onmousever` 
3. 从一个元素上移开鼠标：`onmouseout` 
4. 按下键盘按键：`onkeydown`
5. 浏览器完成页面的加载：`onload`
6. 元素改变：`onchange`



### String与对象|typeof|===

1. 字符串断行需要使用反斜杠`\` 

   ```js
   var x = "Hello \
   World!";
   ```

2. 通过字符创建字符串，类型是`string`

3. 通过new来创建字符串，类型是`object`

4. 利用typeof 用来返回类型

5. string对象会拖慢速度，产生副作用

6. `===` 为绝对相等，数据类型和值都必须相等。

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





### break和continue可以带有标签

1. continue语句（带有或者不带标签引用）只能用在循环中

2. break语句（不带标签引用），只能用在循环和switch语句中

3. 但是，通过标签引用，break语句可以跳出任何js代码块

   ```js
   break labelname;
   continue labeiname;
   ```



### typeof, undefined,null

1. typeof检测null返回的是object

2. typeof检测undefined返回的是undefined

3. undefined是一个没有设置值的变量（值为undefined，类型是undefined）

   `person=undefined`

4. 如果typeof一个没有值的变量（值为undefined，类型是undefined）会返回undefined。

   `var person`

   `typeof person`

5. undefined与null：

   ```js
   typeof undefined             // undefined
   typeof null                  // object
   null === undefined           // false
   null == undefined            // true
   ```

6. `null` 用于对象，`undefined` 用于变量，属性，方法。对象只有被定义才有可能为 null，否则为 undefined

7. 如果我们想测试对象是否存在，在对象还没定义时将会抛出一个错误。

   ```js
   //错误方式：
   if (myObj !== null && typeof myObj !== "undefined") 
   
   //正确方式：
   if (typeof myObj !== "undefined" && myObj !== null) 
   ```



### 数据类型与类型转换

1. js中所有数据都是以64位浮点型数据来存储。

2. 数据类型：

   在 JavaScript 中有 6 种不同的数据类型：

   - string
   - number
   - boolean
   - object
   - function
   - symbol

   3 种对象类型：

   - Object
   - Date
   - Array

   2 个不包含任何值的数据类型：

   - null
   - undefined

3. 易错的例子：

   ```js
   typeof NaN                    // 返回 number
   typeof [1,2,3,4]              // 返回 object
   typeof {name:'John', age:34}  // 返回 object
   typeof new Date()             // 返回 object
   typeof function () {}         // 返回 function
   typeof myCar                  // 返回 undefined 
   typeof null                   // 返回 object
   ```

4. 类型转换

   - Number() 转换为数字， String() 转换为字符串， Boolean() 转化为布尔值。

   - 将数字转换成字符串：

     - `String()`,`x.toString()`

     - 还有number方法：

       | toExponential() | 把对象的值转换为指数计数法。                         |
       | --------------- | ---------------------------------------------------- |
       | toFixed()       | 把数字转换为字符串，结果的小数点后有指定位数的数字。 |
       | toPrecision()   | 把数字格式化为指定的长度。                           |

   - 将布尔值转换为字符串：`String()`,`true.toString()`

   - 将日期转换为字符串：

     - `String(new Date()) `
     - `obj = new Date()
       obj.toString()`
     - Date方法中更多方法函数：https://www.runoob.com/jsref/jsref-obj-date.html

   - 将字符串转换为数字：

     - `Number("3.14")`
     - Number方法中更多方法函数：https://www.runoob.com/jsref/jsref-obj-number.html

   - 利用 “+”：

     ```js
     var y = "5";      // y 是一个字符串
     var x = + y;      // x 是一个数字
     
     var y = "John";   // y 是一个字符串
     var x = + y;      // x 是一个数字 (NaN)
     ```

     如果变量不能转换，它仍然会是一个数字，但值为 NaN (不是一个数字)

   - 将日期转换为数字：可用`Number` 或者`getTime()`

   - 自动转换：

     ```js
     5 + null    // 返回 5         null 转换为 0
     "5" + null  // 返回"5null"   null 转换为 "null"
     "5" + 1     // 返回 "51"      1 转换为 "1" 
     "5" - 1     // 返回 4         "5" 转换为 5
     ```

   - 自动转换为字符串

     当想要输出一个对象或者一个变量时，javascript会自动调用变量的toSting()方法。

     ```js
     document.getElementById("demo").innerHTML = myVar;
     
     myVar = {name:"Fjohn"}  // toString 转换为 "[object Object]"
     myVar = [1,2,3,4]       // toString 转换为 "1,2,3,4"
     myVar = new Date()      // toString 转换为 "Fri Jul 18 2014 09:08:55 GMT+0200"
     myVar = 123             // toString 转换为 "123"
     myVar = true            // toString 转换为 "true"
     myVar = false           // toString 转换为 "false"
     ```

     

### constructor属性

1. constructor属性返回所有js变量的构造函数：

   ```js
   "John".constructor     // 返回函数 String()  { [native code] }
   (3.14).constructor     // 返回函数 Number()  { [native code] }
   false.constructor      // 返回函数 Boolean() { [native code] }
   [1,2,3,4].constructor  // 返回函数 Array()   { [native code] }
   {name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
   new Date().constructor  // 返回函数 Date()    { [native code] }
   function () {}.constructor // 返回函数 Function(){ [native code] 
   
   ```

2. 使用constructor属性查看对象是否为数组、日期：

   ```js
   function isArray(myArray) {
       return myArray.constructor.toString().indexOf("Array") > -1;
   }
   
   function isDate(myDate) {
       return myDate.constructor.toString().indexOf("Date") > -1;
   }
   ```

   

### 正则表达式

1. 用`search()` 方法检索：

   用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并**返回子串的起始位置**。

   ```js
   var str = "Visit Runoob!"; 
   var n = str.search(/Runoob/i);  //返回6
   
   var str = "Visit Runoob!"; 
   var n = str.search("Runoob");   //返回6
   ```

2. 用`replace()` 方法进行替换：

   用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

   ```js
   var str = document.getElementById("demo").innerHTML; 
   var txt = str.replace(/microsoft/i,"Runoob"); //替换后结果为Visit Runoob!
   
   var str = document.getElementById("demo").innerHTML; 
   var txt = str.replace("Microsoft","Runoob"); //替换后结果为Visit Runoob!
   ```

3. 正则表达式修饰符：

   | 修饰符 | 描述                                                     |
   | :----- | :------------------------------------------------------- |
   | i      | 执行对大小写不敏感的匹配。                               |
   | g      | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
   | m      | 执行多行匹配。                                           |

4. 使用RegExp对象：

   > https://www.runoob.com/jsref/jsref-obj-regexp.html

   - 使用`test()` 方法：

     用于检测一个字符串是否匹配某个模式，如果字符串中含有匹配的文本，则返回 true，否则返回 false。

     ```js
     var patt = /e/;
     patt.test("The best things in life are free!"); //返回true
     
     /e/.test("The best things in life are free!") //返回true
     ```

   - 使用`exec()` 方法：

     用于检索字符串中的正则表达式的匹配。

     该函数返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。

     ```js
     /e/.exec("The best things in life are free!");//返回e
     ```



### try, catch,throw

1. `try……catch……finally` 语句用法大致等同于java

2. 对于**throw**：throw 语句允许我们创建自定义错误。

   正确的技术术语是：创建或**抛出异常**（exception）。如果把 throw 与 try 和 catch 一起使用，那么能够控制程序流，并生成自定义的错误消息。

   ```js
   function myFunction() {
       var message, x;
       message = document.getElementById("message");
       message.innerHTML = "";
       x = document.getElementById("demo").value;
       try { 
           if(x == "")  throw "值为空";
           if(isNaN(x)) throw "不是数字";
           x = Number(x);
           if(x < 5)    throw "太小";
           if(x > 10)   throw "太大";
       }
       catch(err) {
           message.innerHTML = "错误: " + err;
       }
   }
   ```

   

### js的调试

1. 可以用`console.log()` 方法在调试窗口上打印值
2. 可以设置断点
3. 可以用`debugger;` 关键字来停止执行。



### 严格模式

> https://www.runoob.com/js/js-strict.html

1. `"use strict";` 指令。不是一条语句，但是是一个字面量表达式，在 JavaScript 旧版本中会被忽略。严格模式下**不能使用未声明的变量**。**只允许出现在脚本或函数的开头**。

   ```js
   x = 3.14;       // 不报错
   myFunction();
   
   function myFunction() {
      "use strict";
       y = 3.14;   // 报错 (y 未定义)
   }
   ```

2. 严格模式不允许删除变量或对象

3. 不允删除函数

   ```js
   "use strict";
   function x(p1, p2) {};
   delete x;                // 报错 
   ```

4. 不允许变量重名

5. 不允许使用八进制

   ```js
   "use strict";
   var x = 010;             // 报错
   ```

6. 不允许使用转义字符

   ```js
   "use strict";
   var x = \010;            // 报错
   ```

7. 不允许对只读属性赋值

   ```js
   "use strict";
   var obj = {};
   Object.defineProperty(obj, "x", {value:0, writable:false});
   
   obj.x = 3.14;            // 报错
   ```

8. 不允许对一个使用getter方法读取的属性进行赋值

   ```js
   "use strict";
   var obj = {get x() {return 0} };
   
   obj.x = 3.14;            // 报错
   ```

9. 变量名不能使用 "eval" 字符串

10. 变量名不能使用 "arguments" 字符串:

11. 在作用域 eval() 创建的变量不能被调用

    ```js
    "use strict";
    eval ("var x = 2");
    alert (x);               // 报错
    ```

12. 禁止this关键字指向全局对象

    ```js
    function f(){
        return !this;
    } 
    // 返回false，因为"this"指向全局对象，"!this"就是false
    
    function f(){ 
        "use strict";
        return !this;
    } 
    // 返回true，因为严格模式下，this的值为undefined，所以"!this"为true。
    ```

    

### 表单验证

> https://www.runoob.com/js/js-validation.html
>
> https://www.runoob.com/js/js-form-validation.html

1. 可以使用js,也可以使用html自己的约束验证

2. JavaScript 可用来在数据被送往服务器前对 HTML 表单中的这些输入数据进行验证。

   例如：

   ```html
   <form name="myForm" action="demo-form.php" onsubmit="return validateForm();" method="post">
       Email: <input type="text" name="email">
       <input type="submit" value="提交">
   </form>
   ```

   ```js
   function validateForm(){
     var x=document.forms["myForm"]["email"].value;
     var atpos=x.indexOf("@");
     var dotpos=x.lastIndexOf(".");
     if (atpos<1 || dotpos<atpos+2 || dotpos+2>=x.length){
       alert("不是一个有效的 e-mail 地址");
       return false;
     }
   }
   ```

3. html5新增约束验证，表单被提交时浏览器用来实现验证的一种算法。

   HTML 约束验证基于：

   - HTML 输入属性
   - CSS 伪类选择器
   - DOM 属性和方法

4. html5表单约束验证属性：

   | 属性     | 描述                     |
   | :------- | :----------------------- |
   | disabled | 规定输入的元素不可用     |
   | max      | 规定输入元素的最大值     |
   | min      | 规定输入元素的最小值     |
   | pattern  | 规定输入元素值的模式     |
   | required | 规定输入元素字段是必需的 |
   | type     | 规定输入元素的类型       |

   > https://www.runoob.com/html/html5-form-attributes.html

5. css伪类选择器：

   | 选择器    | 描述                                    |
   | :-------- | :-------------------------------------- |
   | :disabled | 选取属性为 "disabled" 属性的 input 元素 |
   | :invalid  | 选取无效的 input 元素                   |
   | :optional | 选择没有"required"属性的 input 元素     |
   | :required | 选择有"required"属性的 input 元素       |
   | :valid    | 选取有效值的 input 元素                 |

   > https://www.runoob.com/css/css-pseudo-classes.html





### js验证API

1. 约束验证DOM的**方法**：

   | Property            | Description                                                  |
   | :------------------ | :----------------------------------------------------------- |
   | checkValidity()     | 如果 input 元素中的数据是合法的返回 true，否则返回 false。   |
   | setCustomValidity() | 设置 input 元素的 validationMessage 属性，用于自定义错误提示信息的方法。使用 setCustomValidity 设置了自定义提示后，validity.customError 就会变成true，则 checkValidity 总是会返回false。 |

2. 对于`setCustomValidity` ,如果要**重新判断**需要**取消自定义提示**，方式如下：

   - `setCustomValidity('') `

   - ` setCustomValidity(null) ` 

   - ` setCustomValidity(undefined)`

3. setCustomValidity的例子：

   ```js
   var inpObj = document.getElementById("id1");
   inpObj.setCustomValidity(''); // 取消自定义提示的方式
   if (inpObj.checkValidity() == false) {
       if(inpObj.value==""){
           inpObj.setCustomValidity("不能为空！");
       }else if(inpObj.value<100 || inpObj.value>300){
           inpObj.setCustomValidity("请重新输入数值（100~300之间）!");
       }
       document.getElementById("demo").innerHTML = inpObj.validationMessage;
   } else {
       document.getElementById("demo").innerHTML = "输入正确";
   }
   ```

4. 约束验证DOM的**属性**：

   | 属性              | 描述                                  |
   | :---------------- | :------------------------------------ |
   | validity          | 布尔属性值，返回 input 输入值是否合法 |
   | validationMessage | 浏览器错误提示信息                    |
   | willValidate      | 指定 input 是否需要验证               |

5. Validity属性：

   | 属性            | 描述                                                       |
   | :-------------- | :--------------------------------------------------------- |
   | customError     | 设置为 true, 如果设置了自定义的 validity 信息。            |
   | patternMismatch | 设置为 true, 如果元素的值不匹配它的模式属性。              |
   | rangeOverflow   | 设置为 true, 如果元素的值大于设置的最大值。                |
   | rangeUnderflow  | 设置为 true, 如果元素的值小于它的最小值。                  |
   | stepMismatch    | 设置为 true, 如果元素的值不是按照规定的 step 属性设置。    |
   | tooLong         | 设置为 true, 如果元素的值超过了 maxLength 属性设置的长度。 |
   | typeMismatch    | 设置为 true, 如果元素的值不是预期相匹配的类型。            |
   | valueMissing    | 设置为 true，如果元素 (required 属性) 没有值。             |
   | valid           | 设置为 true，如果元素的值是合法的。                        |

6. 例子：

   ```js
   <input id="id1" type="number" max="100">
   <button onclick="myFunction()">验证</button>
    
   <p id="demo"></p>
    
   <script>
   function myFunction() {
       var txt = "";
       if (document.getElementById("id1").validity.rangeOverflow) {
          txt = "输入的值太大了";
       }
       document.getElementById("demo").innerHTML = txt;
   }
   </script>
   ```

   ```js
   <input id="id1" type="number" min="100" max="300" required>
   <button onclick="myFunction()">验证</button>
    
   <p id="demo"></p>
    
   <script>
   function myFunction() {
       var inpObj = document.getElementById("id1");
       if (inpObj.checkValidity() == false) {
           document.getElementById("demo").innerHTML = inpObj.validationMessage;
       }
   }
   </script>
   ```



### this关键字

1. `this` 会随着执行环境的变化而改变

   - 在方法中，this 表示该方法所属的对象。

   - 如果单独使用，this 表示全局对象。浏览器中，windows就是该全局对象为**[object window]**;严格模式下，如果单独使用，this 也是指向全局(Global)对象**[object window]**。

   - 在函数中，this 表示全局对象。在浏览器中，window 就是该全局对象为 [**object Window**]

   - 在函数中，在严格模式下，this 是未定义的(undefined)。严格模式下函数是没有绑定到 this 上，这时候 this 是 **undefined**。

     ```js
     "use strict";
     function myFunction() {
       return this;
     }
     ```

   - 在事件中，this 表示接收事件的元素。

     ```js
     <button onclick="this.style.display='none'">
     点我后我就消失了
     </button>
     ```

   - 类似 call() 和 apply() 方法可以将 this 引用到任何对象。

     ​        在 JavaScript 中函数也是对象，对象则有方法，apply 和 call 就是函数对象的方法。这两个方法异常强大，他们允许切换函数执行的上下文环境（context），即 this 绑定的对象。

     ```js
     var person1 = {
       fullName: function() {
         return this.firstName + " " + this.lastName;
       }
     }
     var person2 = {
       firstName:"John",
       lastName: "Doe",
     }
     person1.fullName.call(person2);  // 返回 "John Doe"
     ```



### let和const

1. **ES6** 新增加了两个重要的 JavaScript 关键字: **let** 和 **const**。在 ES6 之前，JavaScript 只有两种作用域： **全局变量** 与 **函数内的局部变量**

2. 新增的**let 声明的变量只在 let 命令所在的代码块内有效**。

3. `var` 关键字声明的全局作用域属于window对象，`let` 关键字声明的全局作用域变量不属于window对象。

   ```js
   var carName = "Volvo";
   // 可以使用 window.carName 访问变量
   let carName = "Volvo";
   // 不能使用 window.carName 访问变量
   ```

4. const 用于声明一个或多个常量，声明时**必须进行初始化**，且初始化后值不可再修改。

5. `const`定义常量与使用`let` 定义的变量**相似点**：

   - 二者都是块级作用域
   - 都不能和它所在作用域内的其他变量或函数拥有相同的名称

6. `const`定义常量与使用`let` 定义的变量的**两点区别**：

   - `const`声明的常量必须初始化，而`let`声明的变量不用
   - const 定义常量的值不能通过再赋值修改，也不能再次声明。而 let 定义的变量值可以修改。

7. **const 的本质**: const 定义的变量并非常量，并非不可变，它定义了一个常量引用一个值。使用 **const 定义的对象或者数组，其实是可变的**。

   ```js
   // 创建常量对象
   const car = {type:"Fiat", model:"500", color:"white"};
    
   // 修改属性:
   car.color = "red";
    
   // 添加属性
   car.owner = "Johnson";
   
   //错误！不能对常量对象重新赋值
   car = {type:"Volvo", model:"EX60", color:"red"};    // 错误
   
   // 创建常量数组
   const cars = ["Saab", "Volvo", "BMW"];
    
   // 修改元素
   cars[0] = "Toyota";
    
   // 添加元素
   cars.push("Audi");
   
   //错误，不能对常量数组重新赋值
   cars = ["Toyota", "Volvo", "Audi"];    // 错误
   ```

8. const 关键字定义的变量则不可以在使用后声明，也就是**const变量需要先声明再使用**;

   let 关键字定义的变量则不可以在使用后声明，也就是**let变量需要先声明再使用。**;

   var 关键字定义的变量可以在使用后声明，也就是变量可以先使用再声明.

9. 对于`const` 和`var` 变量的重置：

   - 使用 **var** 关键字声明的变量在任何地方都可以修改

   - 在相同的作用域或块级作用域中，不能使用 **const** 关键字来重置 **var** 和 **let**关键字声明的变量

   - 在相同的作用域或块级作用域中，不能使用 **const** 关键字来重置 **const** 关键字声明的变量

   - **const** 关键字在不同作用域，或不同块级作用域中是可以重新声明赋值的

     ```js
     const x = 2;       // 合法
     
     {
         const x = 3;   // 合法
     }
     
     {
         const x = 4;   // 合法
     }
     ```

10. 关于`let` 与`var` 重置变量：

    - 在相同的作用域或块级作用域中，不能使用 **let** 关键字来重置 **var** 关键字声明的变量
    - 在相同的作用域或块级作用域中，不能使用 **let** 关键字来重置 **let** 关键字声明的变量
    - 在相同的作用域或块级作用域中，不能使用 **var** 关键字来重置 **let** 关键字声明的变量
    - 使用 **var** 关键字声明的变量在任何地方都可以修改。
    - **let** 关键字在**不同作用域，或不同块级**作用域中是**可以重新声明赋值**的



### JSON

1. 语法规则：

   - 数据为 键/值 对。
   - 数据由逗号分隔。
   - 大括号保存对象
   - 方括号保存数组

2. json对象：

   `{"name":"Runoob", "url":"www.runoob.com"}`

3. json数组，数组可以包含对象：

   ```js
   "sites":[
       {"name":"Runoob", "url":"www.runoob.com"}, 
       {"name":"Google", "url":"www.google.com"},
       {"name":"Taobao", "url":"www.taobao.com"}
   ]
   ```

4. json字符串转换为js对象：

   ```js
   var text = '{ "sites" : [' +
       '{ "name":"Runoob" , "url":"www.runoob.com" },' +
       '{ "name":"Google" , "url":"www.google.com" },' +
       '{ "name":"Taobao" , "url":"www.taobao.com" } ]}';
       
   obj = JSON.parse(text);
   document.getElementById("demo").innerHTML = obj.sites[1].name + " " + obj.sites[1].url;
   ```

5. 相关函数：

   | 函数                                                         | 描述                                           |
   | :----------------------------------------------------------- | :--------------------------------------------- |
   | [JSON.parse()](https://www.runoob.com/js/javascript-json-parse.html) | 用于将一个 JSON 字符串转换为 JavaScript 对象。 |
   | [JSON.stringify()](https://www.runoob.com/js/javascript-json-stringify.html) | 用于将 JavaScript 值转换为 JSON 字符串。       |



### javascript:void(0)含义

1. void(0) 计算为 0，但 Javascript 上没有任何效果

   ```js
   <a href="javascript:void(0)">单击此处什么也不会发生</a>
   ```

2. 用户点击链接后显示警告信息:

   ```js
   <a href="javascript:void(alert('Warning!!!'))">点我!</a>
   ```

3. 参数 a 将返回 undefined

   ```js
   a = void ( b = 5, c = 7 );
   ```

4. 定义一个死链接：

   ```js
   <a href="javascript:void(0);">点我没有反应的!</a>
   ```

5. 对比：页面很长的时候会使用 **#** 来定位页面的具体位置，格式为：**# + id**

   ```js
   <a href="#pos">点我定位到指定位置!</a>
   <br>
   ...
   <br>
   <p id="pos">尾部定位点</p>
   ```




### js函数

1. 函数声明：

   ```js
   function functionName(parameters) {
     执行的代码
   }
   ```

2. 函数表达式存储在变量后变量也可作为一个函数使用：

   ```js
   var x = function (a, b) {return a * b};
   var z = x(4, 3);
   ```

3. 函数也可以通过内置的js函数构造器定义：

   ```js
   var myFunction = new Function("a", "b", "return a * b");
   
   var x = myFunction(4, 3);
   ```

   ```js
   var myFunction = function (a, b) {return a * b};
   
   var x = myFunction(4, 3);
   ```

4. 提升也应用在函数，可以先使用再声明函数

   ```js
   myFunction(5);
   
   function myFunction(y) {
       return y * y;
   }
   ```

5. **函数表达式**可以自调用，自调用表达式会自动调用，如果表达式后面紧跟（），则会自动调用，不能自调用**声明的函数**，通过添加括号，来说明它是一个函数表达式。

   实际上是一个匿名自我调用的函数（无函数名）：

   ```js
   (function () {
       var x = "Hello!!";      // 我将调用自己
   })();
   ```

6. 函数是一个对象：

   - `typeof` 来判断函数类型将返回`function`
   - 函数是一个对象，有属性和方法。
   - `arguments.length` 返回函数调用过程中接收到的参数个数
   - `toString()` 方法将函数作为一个字符串返回

7. 箭头函数（类似lamdba表达式）(es6新增)：

   ```js
   (参数1, 参数2, …, 参数N) => { 函数声明 }
   
   (参数1, 参数2, …, 参数N) => 表达式(单一)
   // 相当于：(参数1, 参数2, …, 参数N) =>{ return 表达式; }
   
   (单一参数) => {函数声明}
   单一参数 => {函数声明}
   
   () => {函数声明}
   ```

   例子：

   ```js
   const x = (x, y) => x * y;
   ```

8. 箭头函数会默认绑定外层的this值，所以箭头函数中this的值和外层的this是一样的，箭头函数**不可以提升**，需要使用前定义。`const` 比`var` 更安全，因为函数表达式始终是一个常量。

   ```js
   const x = (x, y) => { return x * y };
   document.getElementById("demo").innerHTML = x(5, 5); //25
   ```

9. es5中如果函数调用时未提供隐式参数，则参数会默认设置为：`undefined`

   ```js
   function myFunction(x, y) {
       if (y === undefined) {
             y = 0;
       } 
   }
   function myFunction(x, y) {
       y = y || 0;
   }
   ```

10. es6可以自带参数，类似`java`

11. 函数有个内置的对象`arguments` 对象，包含了函数调用的参数数组。`arguments[i]` ,`arguments.length` 

12. 修改对象对象属性在函数外是可见的，而值传递的参数在函数外不可见，不会修改显式参数的在函数外定义的初始值。（类似cpp的值传递和地址传递）

13. 函数调用：

    - 作为函数调用，会自动变为window对象的函数：

      ```js
      function myFunction(a, b) {
          return a * b;
      }
      window.myFunction(10, 2);    // window.myFunction(10, 2) 返回 20
      
      function myFunction() {
          return this;
      }
      myFunction();                // 返回 window 对象
      ```

    - 作为方法调用，函数属于对象，this的值为函数所在的对象：

      ```js
      var myObject = {
          firstName:"John",
          lastName: "Doe",
          fullName: function () {
              return this;
          }
      }
      myObject.fullName();          // 返回 [object Object] (所有者对象)
      ```

    - 使用构造函数调用函数：

      ```js
      // 构造函数:
      function myFunction(arg1, arg2) {
          this.firstName = arg1;
          this.lastName  = arg2;
      }
       
      // This    creates a new object
      var x = new myFunction("John","Doe");
      x.firstName;                             // 返回 "John"
      ```

      构造函数的调用会创建一个新的对象。新对象会继承构造函数的属性和方法。构造函数中 **this** 关键字没有任何的值。**this** 的值在函数调用实例化对象(new object)时创建.

    - 作为函数方法调用函数：

      **call()** 和 **apply()** 是预定义的函数方法。 两个方法可用于调用函数，两个方法的第一个参数必须是对象本身。

      ```js
      function myFunction(a, b) {
          return a * b;
      }
      myObject = myFunction.call(myObject, 10, 2);     // 返回 20
      ```

      ```js
      function myFunction(a, b) {
          return a * b;
      }
      myArray = [10, 2];
      myObject = myFunction.apply(myObject, myArray);  // 返回 20
      ```

      两个方法都使用了对象本身作为第一个参数。 两者的区别在于第二个参数： apply传入的是一个参数数组，也就是将多个参数组合成为一个数组传入，而call则作为call的参数传入（从第二个参数开始）。

      在 JavaScript 严格模式(strict mode)下, 在调用函数时第一个参数会成为 **this** 的值， 即使该参数不是一个对象。

      在 JavaScript 非严格模式(non-strict mode)下, 如果第一个参数的值是 null 或 undefined, 它将使用全局对象替代。

    

### js闭包

1. 变量add指定了函数自我调用返回字值，自我调用函数只执行一次，设置计数器为0。这时add可以作为一个函数使用，可以访问函数上一层作用域的计数器。

   使得**函数拥有私有变量**，计数器受匿名函数的作用域保护，只能通过add修改。

   闭包是一种保护私有变量的机制，在函数执行时形成私有的作用域，保护里面的私有变量不受外界干扰。

   直观的说就是形成一个不销毁的栈环境。

   ```js
   var add = (function () {
       var counter = 0;
       return function () {return counter += 1;}
   })();
    
   add();
   add();
   add();
    
   // 计数器为 3
   ```



## js高级特性

### js对象

1. 所有事物都是对象，对象是带有属性和方法的特殊数据类型

2. 创建直接的实例

   ```js
   person=new Object();
   person.firstname="John";
   person.lastname="Doe";
   person.age=50;
   person.eyecolor="blue";
   
   //等同于：
   person={firstname:"John",lastname:"Doe",age:50,eyecolor:"blue"};
   ```

3. 使用函数来创造对象：

   ```js
   function person(firstname,lastname,age,eyecolor){
   	this.firstname=firstname;
   	this.lastname=lastname;
   	this.age=age;
     this.eyecolor=eyecolor;
   }
   var myFather=new person("John","Doe",50,"blue");
   var myMother=new person("Sally","Rally",48,"green");
   ```

4. 可以把属性添加到js对象：

   ```js
   //假设perspn对象已存在
   person.firstname="John";
   person.lastname="Doe";
   ```

5. 但是一个**已存在的对象构造器**中是不能添加新属性的

6. 把方法添加到js对象，方法是附加在对象上的函数，在构造器函数内部定义对象的方法：

   ```js
   function person(firstname,lastname,age,eyecolor)
   {
       this.firstname=firstname;
       this.lastname=lastname;
       this.age=age;
       this.eyecolor=eyecolor;
   
       this.changeName=changeName;
       function changeName(name)
       {
           this.lastname=name;
       }
   }
   
   myMother.changeName("Doe");
   ```

7. 利用for……in循环遍历对象的属性：

   ```js
   var person={fname:"John",lname:"Doe",age:25}; 
    
   for (x in person)
   {
       txt=txt + person[x];
   }
   ```



### js prototype

1. 所有的 JavaScript 对象都会从一个 prototype（原型对象）中继承属性和方法

   - `Date` 对象从 `Date.prototype` 继承。
   - `Array` 对象从 `Array.prototype` 继承。
   - `Person` 对象从 `Person.prototype` 继承。

2. JavaScript 对象有一个**指向一个原型对象的链**。当试图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾

3. 利用**`prototype` 属性**可以给对象的构造函数添加新的属性：

   ```js
   Person.prototype.nationality = "English";
   ```

4. 利用**`prototype` 属性**可以给对象的构造函数添加新的方法：

   ```js
   Person.prototype.name = function() {
     return this.firstName + " " + this.lastName;
   };
   ```

   

### Number对象

1. 所有数字都是浮点类型，64位

2. 整数最多为15位，小数最大位数是17位，但是浮点运算并不总是100%精准

3. 如果前缀为 0，则 JavaScript 会把数值常量解释为八进制数，如果前缀为 0 和 "x"，则解释为十六进制数。

4. 输入不同进制：

   ```js
   var myNumber=128;
   myNumber.toString(16);   // 返回 80
   myNumber.toString(8);    // 返回 200
   myNumber.toString(2);    // 返回 10000000
   ```

5. 无穷大：

   `Infinity` 和`-Infinity`

6. `NaN` :非数字值，可以Number对象设置为改值。`isNaN()` 来判断

   ```js
   var x = 1000 / "Apple";
   isNaN(x); // 返回 true
   var y = 100 / "1000";
   isNaN(y); // 返回 false
   ```

7. 数字可以是数字或者对象。

   ```js
   var x = 123;
   var y = new Number(123);
   typeof(x) // 返回 Number
   typeof(y) // 返回 Object
   ```

8. Number的属性和方法：https://www.runoob.com/js/js-obj-number.html



### String对象

1. 查找字符串`indexOf（）` 来定位首次出现的位置。

   ```js
   var str="Hello world, welcome to the universe.";
   var n=str.indexOf("welcome");
   ```

   没找到返回-1

2. `match()` 进行查找，找到返回这个字符

3. 其余很多用法和java都很类似

4. 属性和方法：https://www.runoob.com/js/js-obj-string.html



### Date对象

1. 获取日期时间：

   ```js
   var d=new Date();
   document.write(d); //Wed Jul 08 2020 16:48:26 GMT+0800 (中国标准时间)
   ```

2. 获取年份：`getFullYear()`

3. 获取星期：`getDay()`

4. Date对象参考手册：https://www.runoob.com/jsref/jsref-obj-date.html



### Array对象

1. 创建数组：

   ```js
   //方法一
   var myCars=new Array();
   myCars[0]="Saab";      
   myCars[1]="Volvo";
   //方法二
   var myCars=new Array("Saab","Volvo","BMW");
   //方法三
   var myCars=["Saab","Volvo","BMW"];
   ```

2. 可以在**一个数组中有不同的变量类型**！

3. 完整参考手册：https://www.runoob.com/jsref/jsref-obj-array.html

   