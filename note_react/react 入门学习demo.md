# react 入门学习demo

> 学习地址：https://www.ruanyifeng.com/blog/2015/03/react.html



## HTML 模版

> demo1

在head中放入需要加载的库，在body中具体的html代码放入script中。`ReactDOM.render()` 用于将模版转为HTML语言，并插入指定的DOM节点。

```js
<!DOCTYPE html>
<html>
    <head>
        <meta charset='utf-8'>
        <script src="../build/react.js"></script>
        <script src="../build/react.development.js"></script>
        <script src="../build/react-dom.development.js"></script>
        <script src="../build/babel.min.js"></script>
    </head>
    <body>
        <div id='hello'></div>
        <script type="text/babel">   //这里因为是react独有的jsx语法，要加上type="text/babel"
        //代码放在这里！
        ReactDOM.render(  //最基本的用法，用于将模版转为HTML语言，并插入指定的DOM节点
            <h1>Hello,world!</h1>,    //注意这里的逗号！HTML与js混写，不加任何引号，这就是jsx的语法。
            document.getElementById('hello')
        );
        </script>
    </body>
</html>
```





## jsx 语法

> demo_2/index1.html& index2.html

### 基本语法规则

遇到 HTML 标签（以 `<` 开头），就用 HTML 规则解析；遇到代码块（以 `{` 开头），就用 JavaScript 规则解析。

```js
let names=['jiaqi','crassy','jane'];
ReactDOM.render(  //最基本的用法，用于将模版转为HTML语言，并插入指定的DOM节点
<div>
{
		names.map(function(name){
		return <div>Hello,{name}!</div>
})
}
</div>,
document.getElementById('names')
);
```

或者直接插入js变量，会直接展开数组，所以我们可以把html元素放入数组中，然后在模版中直接插入js数组。

```js
let arr=[
  <h1>My first react!</h1>,
  <p>ooooooooooo</p>,
  <h2>let's begin</h2>
];
ReactDOM.render(  //最基本的用法，用于将模版转为HTML语言，并插入指定的DOM节点
  <div>
  {arr}
  </div>,
  document.getElementById('arr')
);
```



## 组件

> demo3/index.html & index2.html

### 基本用法

允许将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件。语法实现有点类似java的类和继承。

```js
class MyClass extends React.Component{
  render(){
    return <h1>自己封装的组建h1, author:{this.props.name}</h1>;
  }
}
ReactDOM.render(
  <MyClass name="jiaqigan"/>,
  document.getElementById('first')
);
```

注意，**组件名称**第一个字母必须大写！同时，只能包含一个顶层标签。可以任意加入**属性**，组件的属性可以在组件类的 `this.props` 对象上获取。添加组件属性， `class` 属性需要写成 `className` ，`for` 属性需要写成 `htmlFor` ，这是**因为 `class` 和 `for` 是 JavaScript 的保留字**。

### 关于子节点

通过 `this.props.children` 读取子节点。**this.props.children 的值有三种可能**：如果当前组件没有子节点，它就是 `undefined` ;如果有一个子节点，数据类型是 `object` ；如果有多个子节点，数据类型就是 `array` 。所以，处理 `this.props.children` 的时候要小心。

```js
class MyList extends React.Component{
  render(){
      return(
        <ol>  
          {
            //注意一定要加中括号！！用中括号把html中加的js代码包括起来！
            React.Children.map(this.props.children,function(child){
          			return <li>{child}</li>;
          })
          }
  			</ol>
);
}
}
ReactDOM.render(
  <MyList>
    <span>apple</span>
    <span>peach</span>
    <span>banana</span>
  </MyList>,
  document.getElementById('first')
```

### 检验属性PropTypes

组件类的`PropTypes`属性，就是用来验证组件实例的属性是否符合要求

```js
let data=333;
class MyClass extends React.Component{
  static propTypes={
    title:propTypes.string.isRequired,
  }
	render(){
  	return <h1>自己封装的组建h1, author:{this.props.name}</h1>;
	}
}
ReactDOM.render(
  <MyClass name={data}/>,
	document.getElementById('first')
);
```

这样的话就显示不出来了。因为在`PropTypes`属性中要求了必须为字符串。二提供的data是一个数值。







