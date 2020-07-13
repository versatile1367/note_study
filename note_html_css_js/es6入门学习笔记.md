# es6入门学习笔记

> 学习教程地址：https://www.runoob.com/w3cnote/es6-tutorial.html
>
> 第一遍学习es6,因为时间比较紧，所以第一遍学习仅仅是大概过一遍最基本的知识点。较为详细的学习es6的所有特性后续再进行。
>
> 笔记中仅仅记录学习过程中自己认为的重点、易遗漏的点。



### let和const

1. **ES6** 新增加了两个重要的 JavaScript 关键字: **let** 和 **const**。在 ES6 之前，JavaScript 只有两种作用域： **全局变量** 与 **函数内的局部变量**

2. 新增的**let 声明的变量只在 let 命令所在的代码块内有效**。

3. let 是在代码块内有效，var 是在全局范围内有效（无论var是在哪个代码块中声明，都是全局有效）

4. let只能声明一次，var可以声明多次

   ```js
   for (var i = 0; i < 10; i++) {
     setTimeout(function(){
       console.log(i);
     })
   }
   // 输出十个 10
   for (let j = 0; j < 10; j++) {
     setTimeout(function(){
       console.log(j);
     })
   }
   // 输出 0123456789
   ```

   变量 j 是用 let 声明的，当前的 j 只在本轮循环中有效，每次循环的 j 其实都是一个新的变量.

5. `var` 关键字声明的全局作用域属于window对象，`let` 关键字声明的全局作用域变量不属于window对象。

   ```js
   var carName = "Volvo";
   // 可以使用 window.carName 访问变量
   let carName = "Volvo";
   // 不能使用 window.carName 访问变量
   ```

6. const 用于声明一个或多个常量，声明时**必须进行初始化**，且初始化后值不可再修改。

7. `const`定义常量与使用`let` 定义的变量**相似点**：

   - 二者都是块级作用域
   - 都不能和它所在作用域内的其他变量或函数拥有相同的名称

8. `const`定义常量与使用`let` 定义的变量的**两点区别**：

   - `const`声明的常量必须初始化，而`let`声明的变量不用
   - const 定义常量的值不能通过再赋值修改，也不能再次声明。而 let 定义的变量值可以修改。

9. **const 的本质**: const 定义的变量并非常量，并非不可变，它定义了一个常量引用一个值。使用 **const 定义的对象或者数组，其实是可变的**。

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

10. const 关键字定义的变量则不可以在使用后声明，也就是**const变量需要先声明再使用**;

    let 关键字定义的变量则不可以在使用后声明，也就是**let变量需要先声明再使用。**;

    var 关键字定义的变量可以在使用后声明，也就是变量可以先使用再声明

    ```js
    console.log(a);  //ReferenceError: a is not defined
    let a = "apple";
     
    console.log(b);  //undefined
    var b = "banana";
    ```

    变量 b 用 var 声明存在变量提升，所以当脚本开始运行的时候，b **已经存在**了，但是还**没有赋值**，所以会输出 undefined。变量 a 用 let 声明不存在变量提升，在声明变量 a 之前，a 不存在，所以会报错。

11. 对于`const` 和`var` 变量的重置：

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

12. 关于`let` 与`var` 重置变量：

    - 在相同的作用域或块级作用域中，不能使用 **let** 关键字来重置 **var** 关键字声明的变量
    - 在相同的作用域或块级作用域中，不能使用 **let** 关键字来重置 **let** 关键字声明的变量
    - 在相同的作用域或块级作用域中，不能使用 **var** 关键字来重置 **let** 关键字声明的变量
    - 使用 **var** 关键字声明的变量在任何地方都可以修改。
    - **let** 关键字在**不同作用域，或不同块级**作用域中是**可以重新声明赋值**的

13. ES6 明确规定，代码块内如果存在 let 或者 const，代码块会对这些命令声明的变量从块的开始就形成一个封闭作用域。代码块内，在声明变量 PI 之前使用它会报错。

    ```js
    var PI = "a";
    if(true){
      console.log(PI);  // ReferenceError: PI is not defined
      const PI = "3.1415926";
    }
    ```



### ES6解构赋值

1. 针对数组或者对象进行模式匹配，然后对其中的变量进行赋值。方便了复杂对象中数据字段获取

2. **解构的源**，解构赋值表达式的右边部分；**解构的目标**，解构赋值表达式的左边部分。

3. **数组模型**的解构：

   ```js
   let [a, b, c] = [1, 2, 3];
   // a = 1,b = 2,c = 3
   
   let [a, [[b], c]] = [1, [[2], 3]];
   // a = 1,b = 2,c = 3
   
   let [a, , b] = [1, 2, 3];
   // a = 1,b = 3 
   
   let [a = 1, b] = []; 
   // a = 1, b = undefined
   
   let [a, ...b] = [1, 2, 3];
   //a = 1, b = [2, 3]
   
   let [a, b, c, d, e] = 'hello';
   // a = 'h',b = 'e',c = 'l',d = 'l',e = 'o'
   
   let [a = 2] = [undefined]; // a = 2
   
   let [a = 3, b = a] = [];     // a = 3, b = 3
   let [a = 3, b = a] = [1];    // a = 1, b = 1
   let [a = 3, b = a] = [1, 2]; // a = 1, b = 2
   ```

4. **对象模型**的解构

   ```js
   let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
   // foo = 'aaa'，bar = 'bbb'
    
   let { baz : foo } = { baz : 'ddd' };
   // foo = 'ddd'
   
   let obj = {p: ['hello', {y: 'world'}] };
   let {p: [x, { y }] } = obj;
   // x = 'hello'， y = 'world'
   
   let obj = {p: ['hello', {y: 'world'}] };
   let {p: [x, {  }] } = obj;
   // x = 'hello'
   
   //不完全解构
   let obj = {p: [{y: 'world'}] };
   let {p: [{ y }, x ] } = obj;
   // x = undefined，y = 'world'
   
   let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40};
   // a = 10
   // b = 20
   // rest = {c: 30, d: 40}
   
   let {a = 10, b = 5} = {a: 3};
   // a = 3; b = 5;
   let {a: aa = 10, b: bb = 5} = {a: 3};
   // aa = 3; bb = 5;
   ```

5. 数组以序列号一一对应，这是一个有序的对应关系。 而对象根据属性名一一对应，这是一个无序的对应关系。 根据这个特性，使用解析结构从对象中获取属性值更加具有可用性。

6. 应用时：

   ```js
   // 首先有这么一个对象
   const props = {
       className: 'tiger-button',
       loading: false,
       clicked: true,
       disabled: 'disabled'
   }
   
   // es6
   const { loading, clicked } = props;
   // 给一个默认值，当props对象中找不到loading时，loading就等于该默认值
   const { loading = false, clicked } = props;
   ```

   ```js
   import React, { Component } from 'react';
   export { default } from './Button';
   const { click, loading } = this.props;
   const { isCheck } = this.state;
   ```

   

### Symbol

1. 新增的原始数据类型，不是对象，表示独一无二的值。

2. 可以用来定义**对象的唯一属性名**

3. 每次创建的`Symbol` 都是唯一不重复的所以别人再创建一个Symbol也不会和原来的冲突，里面的字符串只是用来描述、调试、为了让自己知道这个Symbol是干嘛的。

4. 两种创建方法；

   - 局部的Symbol:

     ```js
     var s = Symbol("description");
     ```

   - 全局的Symbol:

     ```js
     var s = Symbol.for("description");
     ```

5. ==**重点！**==两种创建方法的区别：

   - 直接使用 `Symbol("xx")`，每一次都会创建一个不同的 Symbol，不管传入的描述符是否一样：

     ```js
     var a=Symbol("a"), b=Symbol("a");
     a === b;    //false
     ```

   - 但是如果用 `Symbol.for` 创建全局Symbol，那么当描述符和以前的一个全局Symbol相同的时候，会返回之前的Symbol，所以他们是相等的，如果没有则创建一个新的

     ```js
     var a = Symbol.for("a"), b = Symbol.for("a");
     a === b;    //true
     ```

6. Symbol函数栈不能用new命令，因为symbol是原始数据类型，不是对象。可以接受字符串作为参数来提供描述，用来显示在控制台或者作为字符串时使用，便于区分。

   ```js
   let sy = Symbol("KK");
   console.log(sy);   // Symbol(KK)
   typeof(sy);        // "symbol"
    
   // 相同参数 Symbol() 返回的值不相等
   let sy1 = Symbol("kk"); 
   sy === sy1;       // false
   ```

7. 可以作为属性名进行使用：

   ```js
   let sy = Symbol("key1");
    
   // 写法1
   let syObject = {};
   syObject[sy] = "kk";
   console.log(syObject);    // {Symbol(key1): "kk"}
    
   // 写法2
   let syObject = {
     [sy]: "kk"
   };
   console.log(syObject);    // {Symbol(key1): "kk"}
    
   // 写法3
   let syObject = {};
   Object.defineProperty(syObject, sy, {value: "kk"});
   console.log(syObject);   // {Symbol(key1): "kk"}
   ```

   同时要注意，Symbol作为对象属性名不能用`.` 运算符，**要用方括号**。因为**`.` 运算符后面是字符串！**

   ```js
   let syObject = {};
   syObject[sy] = "kk";
    
   syObject[sy];  // "kk"
   syObject.sy;   // undefined
   ```

   Symbol 值作为属性名时，该属性是**公有属性**不是私有属性，可以在类的外部访问。但是**不会出现在** for...in 、 for...of 的循环中，也不会被 Object.keys() 、 Object.getOwnPropertyNames() 返回。如果要读取到一个对象的 Symbol 属性，可以**通过 Object.getOwnPropertySymbols() 和 Reflect.ownKeys() 取到**。

   ```js
   let syObject = {};
   syObject[sy] = "kk";
   console.log(syObject);
    
   for (let i in syObject) {
     console.log(i);
   }    // 无输出
    
   Object.keys(syObject);                     // []
   Object.getOwnPropertySymbols(syObject);    // [Symbol(key1)]
   Reflect.ownKeys(syObject);                 // [Symbol(key1)]
   ```

8. 可以用来定义常量。

9. 用字符串不能保证常量是独特的，比如可以定义两个常量都是等于某一相同的字符串。但是，使用 Symbol 定义常量，这样就可以保证**这一组常量的值都不相等**。

10. `Symbol.for()` :

    类似单例模式，首先会在全局搜索被登记的 Symbol 中是否有该字符串参数作为名称的 Symbol 值，如果有即返回该 Symbol 值，若没有则新建并返回一个以该字符串参数为名称的 Symbol 值，并登记在全局环境中供搜索

    ```js
    let yellow = Symbol("Yellow");
    let yellow1 = Symbol.for("Yellow");
    yellow === yellow1;      // false
     
    let yellow2 = Symbol.for("Yellow");
    yellow1 === yellow2;     // true
    ```

11. `Symbol.keyFor()` :

    Symbol.keyFor() 返回一个已登记的 Symbol 类型值的 key ，用来检测该字符串参数作为名称的 Symbol 值是否已被登记。

    ```js
    let yellow1 = Symbol.for("Yellow");
    Symbol.keyFor(yellow1);    // "Yellow"
    ```



### Map

1. Map和Objects的区别：

   - 一个 Object 的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。
   - Map 中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
   - Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
   - Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

2. 利用`new Map()` 进行创建，利用`set()` 进行添加键值对，利用`get()` 获取对应键的值。

   ```js
   //key 是字符串
   var myMap = new Map();
   var keyString = "a string"; 
   myMap.set(keyString, "和键'a string'关联的值");
   myMap.get(keyString);    // "和键'a string'关联的值"
   myMap.get("a string");   // "和键'a string'关联的值"
   //key 是对象
   var myMap = new Map();
   var keyObj = {};
   myMap.set(keyObj, "和键 keyObj 关联的值");
   myMap.get(keyObj); // "和键 keyObj 关联的值"
   myMap.get({}); // undefined, 因为 keyObj !== {}
   //key 是函数
   var myMap = new Map();
   var keyFunc = function () {}, // 函数
   myMap.set(keyFunc, "和键 keyFunc 关联的值");
   myMap.get(keyFunc); // "和键 keyFunc 关联的值"
   myMap.get(function() {}) // undefined, 因为 keyFunc !== function () {}                     
   ```

3. 作为Map的键来说，**NaN**都是一样的，但是NaN实际上**和任何值甚至和自己都不相等**。但是**对于键来说没区别**。

4. `for……of` 遍历：

   ```js
   var myMap = new Map();
   myMap.set(0, "zero");
   myMap.set(1, "one");
    
   // 将会显示两个 log。 一个是 "0 = zero" 另一个是 "1 = one"
   for (var [key, value] of myMap) {
     console.log(key + " = " + value);
   }
   for (var [key, value] of myMap.entries()) {
     console.log(key + " = " + value);
   }
   /* 这个 entries 方法返回一个新的 Iterator 对象，它按插入顺序包含了 Map 对象中每个元素的 [key, value] 数组。 */
    
   // 将会显示两个log。 一个是 "0" 另一个是 "1"
   for (var key of myMap.keys()) {
     console.log(key);
   }
   /* 这个 keys 方法返回一个新的 Iterator 对象， 它按插入顺序包含了 Map 对象中每个元素的键。 */
    
   // 将会显示两个log。 一个是 "zero" 另一个是 "one"
   for (var value of myMap.values()) {
     console.log(value);
   }
   /* 这个 values 方法返回一个新的 Iterator 对象，它按插入顺序包含了 Map 对象中每个元素的值。 */
   ```

5. `forEach()` 遍历：

   ```js
   var myMap = new Map();
   myMap.set(0, "zero");
   myMap.set(1, "one");
    
   // 将会显示两个 logs。 一个是 "0 = zero" 另一个是 "1 = one"
   myMap.forEach(function(value, key) {
     console.log(key + " = " + value);
   }, myMap)
   ```

6. 关于map的操作。可以与Array进行转换，克隆，合并。

   ```js
   //1. 和数组的转换
   var kvArray = [["key1", "value1"], ["key2", "value2"]];
   // Map 构造函数可以将一个 二维 键值对数组转换成一个 Map 对象
   var myMap = new Map(kvArray);
   // 使用 Array.from 函数可以将一个 Map 对象转换成一个二维键值对数组
   var outArray = Array.from(myMap);
   
   //2. 克隆
   var myMap1 = new Map([["key1", "value1"], ["key2", "value2"]]);
   var myMap2 = new Map(myMap1);
   console.log(original === clone); 
   // 打印 false。 Map 对象构造函数生成实例，迭代出新的对象。
   
   //3.合并
   var first = new Map([[1, 'one'], [2, 'two'], [3, 'three'],]);
   var second = new Map([[1, 'uno'], [2, 'dos']]);
   // 合并两个 Map 对象时，如果有重复的键值，则后面的会覆盖前面的，对应值即 uno，dos， three
   var merged = new Map([...first, ...second]);
   ```



### Set

1. Set 对象存储的值总是唯一的，所以需要判断两个值是否**恒等**。`+0` 与`-0` 不重复，`undefined` 不重复，`NaN` 不重复

2. Set的创建与添加；

   ```js
   let mySet = new Set();
    
   mySet.add(1); // Set(1) {1}
   mySet.add(5); // Set(2) {1, 5}
   mySet.add(5); // Set(2) {1, 5} 这里体现了值的唯一性
   mySet.add("some text"); 
   // Set(3) {1, 5, "some text"} 这里体现了类型的多样性
   var o = {a: 1, b: 2}; 
   mySet.add(o);
   mySet.add({a: 1, b: 2}); 
   // Set(5) {1, 5, "some text", {…}, {…}} 
   // 这里体现了对象之间引用不同不恒等，即使值相同，Set 也能存储
   ```

3. 与Array的转换：

   ```js
   // Array 转 Set
   var mySet = new Set(["value1", "value2", "value3"]);
   // 用...操作符，将 Set 转 Array
   var myArray = [...mySet];
   String
   // String 转 Set
   var mySet = new Set('hello');  // Set(4) {"h", "e", "l", "o"}
   // 注：Set 中 toString 方法是不能将 Set 转换成 String
   ```

4. 数组去重：

   ```js
   var mySet = new Set([1, 2, 3, 4, 4]);
   [...mySet]; // [1, 2, 3, 4]
   ```

5. 求并集

   ```js
   //求并集
   var a = new Set([1, 2, 3]);
   var b = new Set([4, 3, 2]);
   var union = new Set([...a, ...b]); // {1, 2, 3, 4}
   
   //求交集
   var intersect = new Set([...a].filter(x => b.has(x))); // {2, 3}
   
   //差集
   var difference = new Set([...a].filter(x => !b.has(x))); // {1}
   ```





### 字符串

1. 子串的识别:

   - **ncludes()**：返回布尔值，判断是否找到参数字符串。
   - **startsWith()**：返回布尔值，判断参数字符串是否在原字符串的头部。
   - **endsWith()**：返回布尔值，判断参数字符串是否在原字符串的尾部。

   可以接受两个参数，需要搜索的字符串，和**可选的搜索起始位置索引**

2. 字符串重复：

   ```js
   console.log("Hello,".repeat(2));  // "Hello,Hello,"
   console.log("Hello,".repeat(3.2));  // "Hello,Hello,Hello,"
   console.log("Hello,".repeat("2"));  // "Hello,Hello,"
   ```

3. 字符串补全：

   ```js
   console.log("h".padStart(5,"o"));  // "ooooh"
   console.log("h".padEnd(5,"o"));    // "hoooo"
   console.log("h".padStart(5));      // "    h"
   
   console.log("hello".padStart(5,"A"));  // "hello"
   console.log("hello".padEnd(10,",world!"));  // "hello,worl"
   console.log("123".padStart(10,"0"));  // "0000000123"
   ```

4. 模版字符串

   - 模板字符串相当于加强版的字符串，用反引号 **`**,除了作为普通字符串，还可以用来定义多行字符串，还可以在字符串中加入变量和表达式。

     ```js
     // es6
     const a = 20;
     const b = 30;
     const string = `${a}+${b}=${a+b}`;
     
     // es5
     var a = 20;
     var b = 30;
     var string = a + "+" + b + "=" + (a + b);
     ```

   - 插入变量

     ```js
     let name = "Mike";
     let age = 27;
     let info = `My Name is ${name},I am ${age+1} years old next year.`
     console.log(info);
     // My Name is Mike,I am 28 years old next year.
     ```

   - 调用函数

     ```js
     function f(){
       return "have fun!";
     }
     let string2= `Game start,${f()}`;
     console.log(string2);  // Game start,have fun!
     ```

   - 模板字符串中的换行和空格都是会被保留的

5. 模版标签：

   - 是一个函数的调用，其中调用的参数是模板字符串。

     ```js
     alert`Hello world!`;
     // 等价于
     alert('Hello world!');
     ```

   - 当模板字符串中带有变量，会将模板字符串参数处理成多个参数。

     ```js
     function f(stringArr,...values){
      let result = "";
      for(let i=0;i<stringArr.length;i++){
       result += stringArr[i];
       if(values[i]){
        result += values[i];
             }
         }
      return result;
     }
     let name = 'Mike';
     let age = 27;
     f`My Name is ${name},I am ${age+1} years old next year.`;
     // "My Name is Mike,I am 28 years old next year."
      
     f`My Name is ${name},I am ${age+1} years old next year.`;
     // 等价于
     f(['My Name is',',I am ',' years old next year.'],'Mike',28);
     
     ```

   - 可以应用在过滤HTML字符串、国际化处理

### 数值

1. 进制：

   - 二进制表示法新写法: 前缀 0b 或 0B 
   - 八进制表示法新写法: 前缀 0o 或 0O 

2. 常量：

   - Number.EPSILON 属性表示 1 与大于 1 的最小浮点数之间的差。它的值接近于 2.2204460492503130808472633361816E-16，或者 2-52。

3. 安全整数：

   安全整数表示在 JavaScript 中能够精确表示的整数，安全整数的范围在 2 的 -53 次方到 2 的 53 次方之间（不包括两个端点），超过这个范围的整数无法精确表示。

   ```js
   Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2; // true
   Number.MAX_SAFE_INTEGER === Number.MAX_SAFE_INTEGER + 1;     // false
   Number.MAX_SAFE_INTEGER - 1 === Number.MAX_SAFE_INTEGER - 2; // false
   
   Number.MIN_SAFE_INTEGER + 1 === Number.MIN_SAFE_INTEGER + 2; // false
   Number.MIN_SAFE_INTEGER === Number.MIN_SAFE_INTEGER - 1;     // false
   Number.MIN_SAFE_INTEGER - 1 === Number.MIN_SAFE_INTEGER - 2; // true
   ```



### 箭头函数

1. 箭头函数（类似lamdba表达式）(es6新增)：

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

2. 箭头函数中，没有this。如果在箭头函数中使用了this，那么该this一定就是外层的this。无从谈起用call/apply/bind来改变this指向。

3. 在ES6中，会默认采用严格模式，因此this也不会自动指向window对象了，而箭头函数本身并没有this，因此this就只能是undefined。所以**想使用this的时候，不要使用箭头函数的写法！**

   ```js
   const person = {
       name: 'tom',
       getName: () => this.name
   }
   
   // 但是编译结果却是
   var person = {
       name: 'tom',
       getName: function getName() {
           return undefined.name;
       }
   };
   ```



### 函数的默认参数

1. 在实际开发中给参数添加适当的默认值，可以让我们对函数的参数类型有一个直观的认知

   ```js
   function add(x = 20, y = 30) {
       return x + y;
   }
   
   console.log(add());
   ```



### 展开运算符

1. 在ES6中用`...`来表示展开运算符，它可以将数组方法或者对象进行展开。

   ```js
   const arr1 = [1, 2, 3];
   const arr2 = [...arr1, 10, 20, 30];
   // 这样，arr2 就变成了[1, 2, 3, 10, 20, 30];
   ```

   ```js
   const obj1 = {
     a: 1,
     b: 2,
     c: 3
   }
   
   const obj2 = {
     ...obj1,
     d: 4,
     e: 5,
     f: 6
   }
   ```

2. 展开运算符还常常运用在解析结构之中，例如在Raect封装组件的时候常常不确定props到底还有多少数据会传进来，就会利用展开运算符来处理剩余的数据。

   ```js
   // 这种方式在react中十分常用
   const props = {
     size: 1,
     src: 'xxxx',
     mode: 'si'
   }
   const { size, ...others } = props;
   console.log(others)
   ```

3. 展开运算符还用在函数的参数中，来表示函数的不定参。只有放在最后才能作为函数的不定参，否则会报错。

   ```js
   // 所有参数之和
   const add = (a, b, ...more) => {
       return more.reduce((m, n) => m + n) + a + b
   }
   
   console.log(add(1, 23, 1, 2, 3, 4, 5)) // 39
   ```

   

### 对象字面量与class

1. ES6针对对象字面量做了许多简化语法的处理。当属性与值的变量同名时，对象字面量写法中的方法也可以有简写方式：

   ```js
   // es6
   const person = {
     name,
     age,
     getName() { // 只要不使用箭头函数，this就还是我们熟悉的this
       return this.name
     }
   }
   
   // es5
   var person = {
     name: name,
     age: age,
     getName: function getName() {
       return this.name;
     }
   };
   ```

2. 那么这种方式在任何地方都可以使用，比如在一个模块对外提供接口时

   ```js
   const getName = () => person.name;
   const getAge = () => person.age;
   
   // commonJS的方式
   module.exports = { getName, getAge }
   
   // ES6 modules的方式
   export default { getName, getAge  }
   ```

3. 在对象字面量中可以使用中括号作为属性，表示属性名也能是一个变量:

   ```js
   const name = 'Jane';
   const age = 20
   
   const person = {
     [name]: true,
     [age]: true
   }
   ```

4. 类：

   ```js
   // ES5
   // 构造函数
   function Person(name, age) {
     this.name = name;
     this.age = age;
   }
   // 原型方法
   Person.prototype.getName = function() {
     return this.name
   }
   
   
   // ES6
   class Person {
     constructor(name, age) {  // 构造函数
       this.name = name;
       this.age = age;
     }
     getName() {  // 原型方法
       return this.name
     }
   }
   ```

5. 详细例子：

   ```js
   class Person {
     constructor(name, age) {  // 构造函数
       this.name = name;
       this.age = age;
     }
   
     getName() {   // 这种写法表示将方法添加到原型中
       return this.name
     }
   
     static a = 20;  // 等同于 Person.a = 20
   
     c = 20;   // 表示在构造函数中添加属性 在构造函数中等同于 this.c = 20
   
   // 箭头函数的写法表示在构造函数中添加方法，在构造函数中等同于this.getAge = function() {}
     getAge = () => this.age   
   
   }
   ```

   箭头函数需要注意的仍然是this的指向问题，因为箭头函数this指向不能被改变的特性，因此在react组件中常常利用这个特性来在不同的组件进行传值会更加方便。

6. 继承extends:

   ```js
   class Person {
     constructor(name, age) {
       this.name = name;
       this.age = age;
     }
   
     getName() {
       return this.name
     }
   }
   
   // Student类继承Person类
   class Student extends Person {
     constructor(name, age, gender, classes) {
       super(name, age);
       this.gender = gender;
       this.classes = classes;
     }
   
     getGender() {
       return this.gender;
     }
   }
   ```

   关于`super` ,对比一下es5:

   ```js
   // 构造函数中
   // es6
   super(name, age);
   
   // es5
   Person.call(this);
   ```

   



### Promise对象

1. 是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

2. 特点：

   - 对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。
   - 一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

3. 无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。

4. 如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。

5. 创造`promise` ：

   ```js
   const promise = new Promise(function(resolve, reject) {
     // ... some code
   
     if (/* 异步操作成功 */){
       resolve(value);
     } else {
       reject(error);
     }
   });
   ```

   - 两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供

6. `Promise` 实例生成以后，可以用`then` 方法指定`resolve` 和`rejected` 状态的回掉函数

   ```js
   promise.then(function(value) {
     // success
   }, function(error) {
     // failure
   });
   ```

7. 例子：

   ```js
   function timeout(ms) {
     return new Promise((resolve, reject) => {
       setTimeout(resolve, ms, 'done');
     });
   }
   
   timeout(100).then((value) => {
     console.log(value);
   });
   ```

   ```js
   let promise = new Promise(function(resolve, reject) {
     console.log('Promise');
     resolve();
   });
   
   promise.then(function() {
     console.log('resolved.');
   });
   
   console.log('Hi!');
   
   // Promise
   // Hi!
   // resolved
   ```

8. 如果调用`resolve`函数和`reject`函数时带有参数，那么它们的参数会被传递给回调函数。`reject`函数的参数通常是`Error`对象的实例，表示抛出的错误；`resolve`函数的参数除了正常的值以外，还可能是另一个 Promise 实例

   ```js
   const p1 = new Promise(function (resolve, reject) {
     setTimeout(() => reject(new Error('fail')), 3000)
   })
   
   const p2 = new Promise(function (resolve, reject) {
     setTimeout(() => resolve(p1), 1000)
   })
   
   p2
     .then(result => console.log(result))
     .catch(error => console.log(error))
   // Error: fail
   ```

9. **注意**，立即 resolved 的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。

   ```js
   new Promise((resolve, reject) => {
     resolve(1);
     console.log(2);
   }).then(r => {
     console.log(r);
   });
   // 2
   // 1
   
   new Promise((resolve, reject) => {
     return resolve(1);
     // 后面的语句不会执行
     console.log(2);
   })
   ```

10. `then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法:

    ```js
    //第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。
    getJSON("/posts.json").then(function(json) {
      return json.post;
    }).then(function(post) {
      // ...
    });
    //第二个例子
    getJSON("/post/1.json").then(function(post) {
      return getJSON(post.commentURL);
    }).then(function (comments) {
      console.log("resolved: ", comments);
    }, function (err){
      console.log("rejected: ", err);
    });
    ```

    第二个例子中，第一个`then`方法指定的回调函数，返回的是另一个`Promise`对象。这时，第二个`then`方法指定的回调函数，就会等待这个新的`Promise`对象状态发生变化。如果变为`resolved`，就调用第一个回调函数，如果状态变为`rejected`，就调用第二个回调函数。

11. `Promise.prototype.catch()`方法

    - 是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

      ```js
      getJSON('/posts.json').then(function(posts) {
        // ...
      }).catch(function(error) {
        // 处理 getJSON 和 前一个回调函数运行时发生的错误
        console.log('发生错误！', error);
      });
      ```

    - 如果 Promise 状态已经变成`resolved`，再抛出错误是无效的。因为 Promise 的状态一旦改变，就永久保持该状态，不会再变了。

      ```js
      const promise = new Promise(function(resolve, reject) {
        resolve('ok');
        throw new Error('test');
      });
      promise
        .then(function(value) { console.log(value) })
        .catch(function(error) { console.log(error) });
      // ok
      ```

    - Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个`catch`语句捕获。

      ```js
      getJSON('/post/1.json').then(function(post) {
        return getJSON(post.commentURL);
      }).then(function(comments) {
        // some code
      }).catch(function(error) {
        // 处理前面三个Promise产生的错误
      });
      ```

    - 如果没有使用`catch()`方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。Promise 内部的错误不会影响到 Promise 外部的代码.
    
12. `Promise.prototype.finally()`

    - 指定不管 Promise 对象最后状态如何，都会执行的操作

      ```js
      promise
      .then(result => {···})
      .catch(error => {···})
      .finally(() => {···});
      ```

13. `Promise.all()`

    - 将多个 Promise 实例，包装成一个新的 Promise 实例

      ```js
      const p = Promise.all([p1, p2, p3]);
      ```

    - 方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例

    - 如果参数不是promise实例，就会先调用下面讲到的`Promise.resolve`方法，将参数转为 Promise 实例

    - 关于最终结果的状体：

      （1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

      （2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

14. `Promise.race()` 方法

    - 同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

      ```js
      const p = Promise.race([p1, p2, p3]);
      ```

      只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。

    - 方法的参数与`Promise.all()`方法一样，如果不是 Promise 实例，就会先调用下面讲到的`Promise.resolve()`方法，将参数转为 Promise 实例，再进一步处理。

15. `Promise.allSettled()`方法：

    - 接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。只有等到所有这些参数实例都返回结果，不管是`fulfilled`还是`rejected`，包装实例才会结束。
    - 结束后返回的promise实例状态总是`fulfilled`，不会变成`rejected`。状态变成`fulfilled`后，Promise 的监听函数接收到的参数是一个数组，每个成员对应一个传入`Promise.allSettled()`的 Promise 实例。

16. `Promise.any()`方法：

    - 接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。只要参数实例有一个变成`fulfilled`状态，包装实例就会变成`fulfilled`状态；如果所有参数实例都变成`rejected`状态，包装实例就会变成`rejected`状态。

17. `Promise.resolve()`方法：

    - 将现有对象转为 Promise 对象

      ```js
      Promise.resolve('foo')
      // 等价于
      new Promise(resolve => resolve('foo'))
      ```

    - 参数有以下四种情况：

      - 参数是一个 Promise 实例

        不做任何修改、原封不动地返回这个实例。

      - 参数是一个`thenable`对象(`thenable`对象指的是具有`then`方法的对象)

        ```javascript
        let thenable = {
          then: function(resolve, reject) {
            resolve(42);
          }
        };
        
        let p1 = Promise.resolve(thenable);
        p1.then(function(value) {
          console.log(value);  // 42
        });
        ```

      - 参数不是具有`then`方法的对象，或根本就不是对象

        ```javascript
        const p = Promise.resolve('Hello');
        
        p.then(function (s){
          console.log(s)
        });
        // Hello
        ```

      - 不带有任何参数

        ```javascript
        const p = Promise.resolve();
        
        p.then(function () {
          // ...
        });
        ```

18. `Promise.reject()` 方法：

    - 返回一个新的 Promise 实例，该实例的状态为`rejected`。

    - `Promise.reject()`方法的参数，会原封不动地作为`reject`的理由，变成后续方法的参数

      ```js
      const p = Promise.reject('出错了');
      // 等同于
      const p = new Promise((resolve, reject) => reject('出错了'))
      
      p.then(null, function (s) {
        console.log(s)
      });
      // 出错了
      ```

19. `Promise.try()`方法：

    - 让同步函数同步执行，异步函数异步执行，并且让它们具有统一的 API 

    ```javascript
    Promise.try(() => database.users.get({id: userId}))
      .then(...)
      .catch(...)
    ```



### Iterator

1. 任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）

2. 默认的 Iterator 接口部署在数据结构的`Symbol.iterator`属性，或者说，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”

3. 例如数组的Symbol.iterator属性：

   ```javascript
   let arr = ['a', 'b', 'c'];
   let iter = arr[Symbol.iterator]();
   
   iter.next() // { value: 'a', done: false }
   iter.next() // { value: 'b', done: false }
   iter.next() // { value: 'c', done: false }
   iter.next() // { value: undefined, done: true }
   ```



### async函数

1. Generator 函数的语法糖。

2. `async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。当函数执行的时候，一旦遇到`await`就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

   ```javascript
   async function getStockPriceByName(name) {
     const symbol = await getStockSymbol(name);
     const stockPrice = await getStockPrice(symbol);
     return stockPrice;
   }
   
   getStockPriceByName('goog').then(function (result) {
     console.log(result);
   });
   ```

   