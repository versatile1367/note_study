# css学习笔记

css还有很多不熟练的地方,每次使用都要查阅资料.

以下是使用中常用到的不熟练的地方总结笔记:

#### box-sizing布局

1. 因为css的盒子模型中因为设置width宽度时是对于内容设置的，还有boder、padding之类的，所以我们希望一个盒子是200px时，还需要减去左右边框和留白，所以不方便。
2. 利用`box-sizing:border-box`,当我们想要一个宽度为200px的盒子后，直接设置宽度200px,当再设置左右边框和左右补白后，内容自动调整。



#### box-shadow

- 用于在元素的框架上添加阴影的效果，可以在同一个元素上设置多个阴影效果，并用逗号隔开。

- 如果元素同时设置了border-radius属性，阴影也会有圆角效果。

- 语法：

  > ```css
  > /* x偏移量 | y偏移量 | 阴影颜色 */
  > box-shadow: 60px -16px teal;
  > 
  > /* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影颜色 */
  > box-shadow: 10px 5px 5px black;
  > 
  > /* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
  > box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
  > 
  > /* 插页(阴影向内) | x偏移量 | y偏移量 | 阴影颜色 */
  > box-shadow: inset 5em 1em gold;
  > 
  > /* 任意数量的阴影，以逗号分隔 */
  > box-shadow: 3px 3px red, -1em 0 0.4em olive;
  > 
  > /* 全局关键字 */
  > box-shadow: inherit;
  > box-shadow: initial;
  > box-shadow: unset;
  > ```

- `inset`值：设置阴影在盒内
- `<offset-x>`,`<offet-y>`：水平偏移正值在元素右边，垂直偏移在元素下方。
- `<blur-radius>`:值越大，模糊面积越大，阴影就越大越淡。不能为负值，默认为0；
- `<spread-radius>`:取正值时，阴影扩大；取负值时，阴影收缩。默认为0，此时阴影和元素同样大。



#### 水平居中

1. 行内元素（父元素为块级元素的行内元素）居中:

    `text-align:center;`

2. 对于单个块级元素

   - 方法1:

     ```css
         width:200px; 
         margin-left:auto;
         margin-right:auto; 
     ```

   - 方法2:

     ```css
         width:200px;
         margin:0 auto;			
     ```

     对于这两种方法都需要指出width的具体值。

3. 对于多个块级元素需要在一行里水平居中

    可以通过设置他们不同的 `display` 属性来达到效果.

    ```css
    .inline-block-center {
      text-align: center;
    }
    .inline-block-center div {
      display: inline-block;
      text-align: left;
    }
    .flex-center {
      display: flex;
      justify-content: center;
    }
    ```



#### position定位：

> 例子引用：https://developer.mozilla.org/zh-CN/docs/Web/CSS/position

1. Static：默认值，没有定位，正常显示出现；top,left,bottom,right等属性无效;

2. Relative：生成**相对其自己正常位置**进行移动，通过left,top,right,bottom进行规定；(原先位置会留下空白，原来的位置还保留)

   举例

   >```html
   ><div class="box" id="one">One</div>
   ><div class="box" id="two">Two</div>
   ><div class="box" id="three">Three</div>
   ><div class="box" id="four">Four</div>
   >```
   >
   >```css
   >.box {
   >  display: inline-block;
   >  width: 100px;
   >  height: 100px;
   >  background: red;
   >  color: white;
   >}
   >
   >#two {
   >  position: relative;
   >  top: 20px;
   >  left: 20px;
   >  background: blue;
   >}
   >```
   >
   ><img src="img/截屏2020-07-01 下午8.25.05.png" alt="截屏2020-07-01 下午8.25.05" style="zoom:50%;" align='left'/>

3. Absolute：生成绝对定位，**相对**于position属性定义为**static之外的第一个父元素**进行定位，位置通过left,top,right,bottom进行规定；会被移出正常文档流；

   例子

   > ```html
   > <div class="box" id="one">One</div>
   > <div class="box" id="two">Two</div>
   > <div class="box" id="three">Three</div>
   > <div class="box" id="four">Four</div>
   > ```
   >
   > ```css
   > .box { 
   >    display: inline-block; 
   >    background: red; 
   >    width: 100px; 
   >    height: 100px; 
   >    float: left; 
   >    margin: 20px; 
   >    color: white; 
   > }
   > 
   > #three { 
   >    position: absolute; 
   >    top: 20px; 
   >    left: 20px; 
   > }
   > ```
   >
   > <img src="img/截屏2020-07-01 下午8.29.17.png" alt="截屏2020-07-01 下午8.29.17" style="zoom:50%;" align='left'/>

4. Fixed：生成绝对定位，相对于浏览器窗口进行定位，通过left,top,right,bottom进行定位；会被移出正常文档流

5. Sticky：在屏幕范围时该元素的位置并不受定位影响（设置的top,left等属性无效）,当该元素要移出偏移范围时，定位会变成fixed，根据设定的left,top等属性成固定位置的效果。



### 背景图片缩放和固定:

- 对于`background-attachment`:

  - fixed
    表示背景相对于窗口固定。

  - local
    背景相对于元素的内容固定，元素内容与背景一起滚动。

  - scroll
    背景相对于元素本身固定，而不是随着它的内容滚动；但如果全窗口滚动，背景仍然会滚动。

- 对于`background-size`:

  - cover
    可以尽可能大的缩放背景图像并保持图像的宽高比例。

  - auto
    以背景图片的比例缩放背景图片。

  - contain
    缩放背景图片以完全装入背景区，可能背景区部分空白。

