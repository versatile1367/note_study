# axios | mock学习笔记

> 用react做页面的需求，需要用axios.
>
> 完整的学习以后再整理。]



## react-axios安装

`npm install react-axios`

同时要安装以下依赖：

```
npm install axios
npm install react
npm install prop-types
```



## 安装mocks

`npm install mockjs`



## 造数据

1. 在项目文件夹中新建文件夹`mock` 

2. 然后在文件中新建文件`dataMock.js` 

3. 在`dataMock.js` 文件中开始造数据：

   > mockjs的文档地址：http://mockjs.com/examples.html#DPD
   >
   > mockjs的语法讲解：https://github.com/nuysoft/Mock/wiki/Syntax-Specification
   >
   > mockjs的常用字段总结：https://www.jianshu.com/p/9dbcfbe6130f

   ```js
   import  Mock from "mockjs";
   
   const dataMock = Mock.mock(
       "test/dataList",
       "get",
       {
           "code": 0,
           "message": "成功",
           "data": {
               "page_ctr": "22_10",
               "total": 30, // 搜索结果总数
               "addon｜30": [  //返回30组
                   {
                       "aid｜+1": 1,
                       "creator_uid": '@integer(0)',
                       "name": '@string("lower",5,20)',
                       "logo": "@image",  //随机图片地址
                       "desc": '@cparagraph(2)',
                       "doc_id": "@string(5,10)",
                       "create_time": '@datetime("yyyy-MM-dd A HH:mm:ss")',
                       "update_time": '@datetime("yyyy-MM-dd A HH:mm:ss")',
                   }
               ]
           }
       }
   )
   ```



## axios获取数据

想把利用axios获取对应接口数据的函数放在一个文档中，然后再在需要用的地方去调用。

> axios文档：http://www.axios-js.com/



## 遇到的问题

### mock数据

在造数据时，有些字段需要从固定的几个数中选取。

例如返回的类型、等级等等，不需要随机生成。

所以利用数组，将固定的内容放入数组，然后从属性值数组中随机选取一个元素作为最终值，如下：

1. `'name|1': array`

   从属性值 `array` 中随机选取 1 个元素，作为最终值。

2. `'name|+1': array`

   从属性值 `array` 中顺序选取 1 个元素，作为最终值。

3. `'name|min-max': array`

   通过重复属性值 `array` 生成一个新数组，重复次数大于等于 `min`，小于等于 `max`。

4. `'name|count': array`

   通过重复属性值 `array` 生成一个新数组，重复次数为 `count`。

想要随机生成很多数组组成列表，但是通过上面的第四个语法，一直没有实现。注意！一定要用**英文的`|`！** 很容易分不清和字符混在一起。**英文英文！**



### axios获取的数据并不能展示出来

​        数据已经获取，在控制台可以显示出数据已经传到存放数据到数组，但是无法在页面中渲染出来。

于是考虑重新封装组件，从函数改为类，同时传递对应的数据。

​        认真学习有关`prop` 和`state` 的内容后，利用`state` 记录状态，在`componentDidMount()` 中进行`axios()` 获取数据成功后在`then()` 中进行`setState()` 更新状态。每次`setState()` 时就会重新渲染页面，于是数据填入。数据加载到页面！

>  教程地址：https://blog.csdn.net/weixin_42442713/article/details/88559861

```ts
componentDidMount(){
        const _this=this;
        axios.get("test/addonList")
        .then(function(response){
            _this.setState({
                rows:response.data.data.addon,
                isLoaded:true
            });
        })
        .catch(function(error){
            console.log(error);
            _this.setState({
                isLoaded:false,
            })
        })
    }
```

然后利用`this.state.rows` 进行数据的传递，可以传递给子组件表格内容的`props`然后进行表格数据的构造和填充。



