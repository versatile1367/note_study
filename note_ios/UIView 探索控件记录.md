# UIView 探索控件记录



### frame&bounds&center

> 参考学习博客：https://code.imerc.cc/2015/09/02/the-influence-of-changing-bounds/

1. 三个名词的解释：

   - frame：描述当前视图在其父视图中的位置和大小。
   - bounds：描述当前视图在其自身坐标系统中的位置和大小。
   - center：描述当前视图的中心点在其父视图中的位置。

2. frame是设置当前view相对于父view的位置及大小。x和y是指当前view的左上角相对于父坐标系的位置。

3. 通过设置bounds（大小保持不变），代码如下：

   ```swift
    let view = UIView(frame: CGRect(x: 100, y: 100, width: 200, height: 200))
           view.backgroundColor = UIColor.red
           view.alpha = 0.5  
           let subview = UIView(frame: CGRect(x: 0, y: 0, width: 100, height: 100))
           subview.backgroundColor = UIColor.green
           view.addSubview(subview)
           self.view.addSubview(view)
   ```

   <img src="img/截屏2020-12-01 下午5.37.35.png" alt="截屏2020-12-01 下午5.37.35"  align="left" style="zoom:50%;" />

   加上修改bounds的代码：

   `    view.bounds = CGRect(x: -50, y: -50, width: 200, height: 200)`

   视图变为：

   <img src="img/截屏2020-12-01 下午5.40.29.png" alt="截屏2020-12-01 下午5.40.29" style="zoom:50%;"  align="left"/>

   将刚刚的加上的bounds的代码改为：

   `    view.bounds = CGRect(x: 50, y: 50, width: 200, height: 200)`

   视图变为：

   <img src="img/截屏2020-12-01 下午5.43.39.png" alt="截屏2020-12-01 下午5.43.39" style="zoom:50%;" align="left"/>

   

   所以，通过bounds来改变xy时（不改变大小width&height）:

   相当于当我们创建一个view以后，这个view就拥有了一个自己的坐标系,此时坐标系 (0,0)在view的左上角。当我们用bounds来进行设置时，相当于在将这个view的左上角设置为对应新坐标系的哪里，坐标系对应着bounds进行了移动。而这个view并不移动，因为view通过frame已经设定了它相对于父view的位置在哪里，父view的坐标系没有变动，所以这个view的坐标也不会进行改变，但是长宽可以进行相应的变动。

   所以设置bounds可以看做**将view自己的坐标系进行移动使得view的左上角匹配上新设置的bounds中的xy值。**

   所以当前view 的bounds的设定即为**当前view自己坐标系的变动**。

   而这个view的子subview，因为通过frame来进行设置的，所以会根据view的坐标系的变动而变动位置。

4. 在bounds中通过height&width来改变大小：

   - 在bounds中如果height和width不再是原来的大小的话，原本view的位置相对于父view也会发生改变。

     将bounds的代码改为：

     `    view.bounds = CGRect(x: -50, y: -50, width: 150, height: 150)`

     视图变为：

     

     <img src="img/截屏2020-12-01 下午6.25.47.png" alt="截屏2020-12-01 下午6.25.47" style="zoom:50%;" align="left"/>

     可以看出和view除了大小变化以外，左上角的位置也发生了变化。

     将其frame的信息打印出来：

     <img src="img/截屏2020-12-01 下午6.26.47.png" alt="截屏2020-12-01 下午6.26.47" style="zoom:50%;" align="left"/>

     可以看出，view的相对于父view的frame信息从原来的(100,100)变为（125,125）

     > 关于位置移动后的计算可以参考：https://code.imerc.cc/2015/09/02/the-influence-of-changing-bounds/、https://www.cnblogs.com/benbenzhu/p/3615516.html?utm_source=tuicool

     计算公式为：

     首先，anchorPoint的取值范围是[0,1],这个值是由该点在自身坐标系的数值与bounds的大小的比例来确定的，anchorPoint默认值为(0.5, 0.5)。

     变换前求出position的坐标

     ```
     position.x = frame.origin.x + anchorPoint.x * bounds.size.width；  
     position.y = frame.origin.y + anchorPoint.y * bounds.size.height；
     ```

     变换后，position点和anchorPoint点都不会改变，求出新的frame的点

     ```
     frame.origin.x = position.x - anchorPoint.x * bounds.size.width；  
     frame.origin.y = position.y - anchorPoint.y * bounds.size.height；
     ```

     

### UIButton设置图片不显示的问题

UIButton设置图片的代码如下：

```swift
 let image1 = UIImage(named: "img1")
 btn1.setImage(image1, for: .normal)
```

名为`img1`的图片存放路径在`assets` 下

此时button上为蓝色。图片并不显示。

**问题解决**： 

因为创建button时将type设置为了system

`    let btn1 = UIButton(type: UIButton.ButtonType.system)`

如果默认init或者设置为custom则可以设置图片。而如果type设置为custom类型的button的话，setImage被锁住，无法自己设置图片。

将其改为`    let btn1 = UIButton(type: UIButton.ButtonType.custom)` 或者`    let btn1 = UIButton()`





### UIEdgeInsetsMake使用

> 参考学习博客：https://www.jianshu.com/p/0d3dbc30fad5

关于定义：

```swift
typedef struct UIEdgeInsets {
    CGFloat top, left, bottom, right;  // specify amount to inset (positive) for each of the edges. values can be negative to 'outset'
} UIEdgeInsets;
```

<img src="img/截屏2020-12-02 上午10.33.14.png" alt="截屏2020-12-02 上午10.33.14" style="zoom:50%;" align="left"/>

#### 只设置image或者title之一

首先没有设置`UIEdgeInsets` 时，在大的btn中设置title,title默认居中处理，视图如下：

<img src="img/截屏2020-12-02 上午10.59.51.png" alt="截屏2020-12-02 上午10.59.51" style="zoom:50%;" align="left"/>

然后设置`    btn2.titleEdgeInsets = UIEdgeInsets(top: 50, left: 0, bottom: 0, right: 0)`

相当于将现在的内容的区域相对顶top下移50，也就是内容和顶部的距离增加50（注意：**是在现有位置基础上相对移动**）

<img src="img/截屏2020-12-02 上午11.01.52.png" alt="截屏2020-12-02 上午11.01.52" style="zoom:50%;" align="left"/>

同时设置top和left`    btn2.titleEdgeInsets = UIEdgeInsets(top: 50, left: 50, bottom: 0, right: 0)`

结果相当于向下移50和向右移50：

<img src="img/截屏2020-12-02 上午11.03.17.png" alt="截屏2020-12-02 上午11.03.17" style="zoom:50%;" align="left"/>

如果同时设置top和bottom为同样的值x，相当于距离顶部top增加x，相对于底部bottom增加x。结果就是相互抵消冲突，最终没有变化。设置：`    btn2.titleEdgeInsets = UIEdgeInsets(top: 50, left: 0, bottom: 50, right: 0)`

结果如下：

<img src="img/截屏2020-12-02 上午11.05.21.png" alt="截屏2020-12-02 上午11.05.21" style="zoom:50%;" align="left"/>

综上是对于`setTitle` 中的edge设置，`setImage` 同理，但是如果在btn中利用的是`UIImageView` ，然后将其作为subview放入btn中的话，是根据其**设置的frame来设定位置**

#### 同时设置image和title

不定义edge时，仅仅同时`setTitle` &`setImage` 的时候，默认情况为图片居左，标题居右，并排排列。

当我们同时添加时，图片默认会向左偏移button的titlelabel的宽度，而标题会向右偏移图片的宽度。

<img src="img/截屏2020-12-02 下午2.24.57.png" alt="截屏2020-12-02 下午2.24.57" style="zoom:50%;" align="left"/>

当设置标题的偏移`    btn2.titleEdgeInsets = UIEdgeInsets(top: 0, left: 0, bottom: 0, right: btn2.currentImage?.size.width ?? 0)`

效果如下：
<img src="img/截屏2020-12-02 下午2.34.00.png" alt="截屏2020-12-02 下午2.34.00" style="zoom:50%;" align="left"/>













然后再设置图片的偏移`    btn2.imageEdgeInsets = UIEdgeInsets(top: 0, left: btn2.titleLabel?.intrinsicContentSize.width ?? 0, bottom: 0, right: 0)`

效果如下：

<img src="img/截屏2020-12-02 下午2.35.20.png" alt="截屏2020-12-02 下午2.35.20" style="zoom:50%;" align="left"/>

至此，图片和标题就重合了。

继续修改偏移至图片在上，标题在下：

```swift
btn2.titleEdgeInsets = UIEdgeInsets(top: btn2.currentImage?.size.height ?? 0, left: 0, bottom: 0, right: btn2.currentImage?.size.width ?? 0)

btn2.imageEdgeInsets = UIEdgeInsets(top: -(btn2.titleLabel?.intrinsicContentSize.height ?? 0), left: btn2.titleLabel?.intrinsicContentSize.width ?? 0, bottom: 0, right: 0)
        
```

效果如下：

<img src="img/截屏2020-12-02 下午2.41.59.png" alt="截屏2020-12-02 下午2.41.59" style="zoom:50%;" align="left"/>

类似，可以在此继续上继续调整整体的图片和标题的位置。



### 图片的获取|UIImageView

1. `UIImageView` 需要添加`UIImage` 类型的图片。

   ```swift
   // 直接创建
   let imageView2 = UIImageView(image: UIImage(named: "img1"))
   
   // 先创建，再添加图片
   let imageView2 = UIImageView()
   imageView2.image = UIImage(named: "img1")
   ```

2. 而对于UIImage的创建，图片的获取方式：

   - 利用`named` 方式，传入图片名称字符串，将图片放入`Assets` 文件夹。如上所示

   - 从文件目录、bundle中获取

     > bundle创建以及使用：https://cloud.tencent.com/developer/article/1368379、
     >
     > https://www.jianshu.com/p/fad9b407762c

     - 第一种方式：直接在工程文件目录下直接添加图片，因为我们的整个工程其实是一个bundle,即mainBundle。

       ```swift
        let path = Bundle.main.path(forResource: "img111", ofType: "png")
        imageView2.image = UIImage(contentsOfFile: path!)
       ```

     - 第二种方式：当有很多静态资源时，需要将这些内容都放入bundle中进行层级管理，然后再工程中使用。

       > 根据此博客进行工程中直接创建bundle:https://www.jianshu.com/p/fad9b407762c

       ```swift
       let myBundlePath = Bundle.main.path(forResource: "MyBundle", ofType: "bundle")!
       let myBundle = Bundle.init(path: myBundlePath)
       let path = myBundle?.path(forResource: "img111", ofType: "png", inDirectory: "img")
       imageView2.image = UIImage(contentsOfFile: path!)
       ```

   - 从网络地址获取图片

     ```swift
     let url = URL(string: "https://www.google.com.hk/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png")
     let data = try! Data(contentsOf: url!)
     imageView2.image = UIImage(data: data)
     ```

3. UIImageView的使用

   > https://www.jianshu.com/p/10f32174fb2d





### UIScrollView内容拖拽不下去

> 基本使用：https://blog.csdn.net/qq_29846663/article/details/75627769

**问题**：

已经设置了是否拖拽、进度条等等的设置，以及添加了数张图片，但是下面的内容无法拖拽到。

**解决**：
一定要设置内容范围大小：

```swift
scrollView.contentSize = CGSize(width: scrollView.frame.size.width, height: scrollView.frame.size.height*3)
```

