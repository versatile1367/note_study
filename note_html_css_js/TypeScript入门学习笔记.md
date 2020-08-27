# typescript入门学习笔记

> 直接开始上手做react的页面了，在过程中不会的、不熟练的、重点的知识临时总结在这里。
>
> 后续再进行整理。



## export

用于从文件（或者模块）中导出函数、对象或者基础类型。

比如我们在component文件夹下面封装好的组件，想要在container中利用。就需要用export。

记得封装的组件要**开头字母大写**

### 命名的导出

导出已经声明的函数或者变量。也可以在函数前直接加上。

如果想要在别的文件中`import`的话，记得加`{}` 

```ts
export { name1, name2, …, nameN };
export { variable1 as name1, variable2 as name2, …, nameN };
export let name1, name2, …, nameN; // also var
export let name1 = …, name2 = …, …, nameN; // also var, const

// 在源文件中
export function cube(x: number): number {
    return x * x * x;
}
const foo: number = Math.PI * Math.sqrt(2);
export { foo }

//在目标文件中
import { cube, foo } from './mylib';  //加大括号！！
```

### 默认的导出

**一个文件（模块）默认的导出只能有一个**，可以是类、函数、对象等。

默认的导出在使用的文件中`import` 的话，不用加大括号。

```ts
export default expression;
export default function (…) { … } // also class, function*
export default function name1(…) { … } // also class, function*
export { name1 as default, … };

//源文件中
export default function (x: number): number {
    return x * x * x;
}

//目标文件中
import cube from './mylib';
```





## import

与上文的`export` 相对应。用于导入

导入命名的导出的时候加中括号`{}` ,导入默认的导出不用加！

```ts
//导入整个模块的内容， 在当前作用域插入 myModule 变量， 包含 my-module.ts 文件中全部导出的绑定
import * as myModule from 'my-module';

//导入模块的某一个导出成员， 在当前作用域插入 myMember 变量
import { myMember } from 'my-module';

//导入模块的多个导出成员， 在当前作用域插入 foo 和 bar 变量：
import {foo, bar} from 'my-module';

//导入模块的成员， 并使用一个更好用的名字
import {reallyReallyLongModuleMemberName as shortName} from 'my-module';

//导入模块的默认导出
import myDefault from 'my-module';

//混合导入
import myDefault, {foo, bar} from 'my-module';
```





## 接口(描述对象)

本来以为接口和java中的接口概念差不多，其实不然。ts中的接口是一个非常灵活的概念，除了可以对类的一部分行为进行抽象以外，也常常用于对对象的形状进行描述。

建议接口名称加上前缀`I` 

### 基本用法

让某个变量类型是接口定义的形状。约束变量的形状必须和接口一致。不能多也不能少。

```
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

### 可选属性

属性后加`?` 表示此属性是可选的，不是必须的。但是仍然不允许添加未定义的属性。

```ts
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```

### 任意属性

可以定义取某类型的属性，用中括号括起来。

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```

**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集!!**

如下就会报错

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,  //不是string的子集，报错
    gender: 'male'
};
```

一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
```

### 只读属性

用`readonly` 定义只读属性，表示只能在**创建**的时候**赋值**

```ts
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}
```







