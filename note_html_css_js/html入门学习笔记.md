# html学习笔记

这里仅记一些用的时候遗漏的知识

## 标签

#### html5中表示布局的标签





#### `<main>`标签

`<main>`在文档中是唯一的,不应包含在文档中重复出现的内容，比如侧栏、导航栏、版权信息、站点标志、搜索表单。一个文档中不能出现一个以上的`<main>`元素，==不可以==是：**article,aside,footer,header,nav**的后代。

支持全局属性和事件属性



#### `<form>`标签

- 属性invalidate：当提交表单时不对表单数据进行验证,h5新特性

- form下面可以再分div

- 表单中有两个tab交换时：

  



#### `<section>`,`<div>`,`<article>`的区别：

1. `<div>` :无语义，用于标记一块区域的内容，进行样式、视觉的处理。只为了样式化或者方便脚本使用时，使用div;
2. `<section>`：有主题性的进行分组。评判标准：这个元素的内容体现在文章大纲中时，用section比较合适。例如网站页面用section划分为网站介绍、主题内容、新闻、联系信息等部分；
3. `<article>`：是一个特殊的section。独立的、完整的、可以独自被外部引用的内容。通常有它自己的标题、脚注等等。可以嵌套，例如用来呈现评论的article可以被包含在表示整体内容的article元素里面。
4. Div->section->article,语义从无到有，逐渐增强。







