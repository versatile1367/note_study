# swift5|简单实现scrollView图片缩放

scrollView中已经有可以处理缩放的内容，所以不需要自己再添加gestureRecgonizer来实现。

实现的机制：当我们在scrollView上使用捏合手势时，ScrollView会给代理发送一条信息，询问的是哪个控件要缩放，在代理的对应函数`func viewForZooming(in scrollView: UIScrollView) -> UIView? ` 中返回要缩放的view即可。

所以需要设置scrollView的代理以及缩放的大小

```swift
scrolleView.delegate = self;
scrolleView.maximumZoomScale = 3;
scrolleView.minimumZoomScale = 0.2;
```

同时为了解决缩放后不居中的问题，还可以在`func scrollViewDidZoom(_ scrollView: UIScrollView)`中进行缩放完毕后的位置处理

```swift
let offsetX = max((scrollView.bounds.size.width - scrollView.contentInset.left - scrollView.contentInset.right - scrollView.contentSize.width)*0.5, 0.0)
        let offsetY = max((scrollView.bounds.size.height - scrollView.contentInset.top - scrollView.contentInset.bottom - scrollView.contentSize.height)*0.5, 0.0)
        scrollView.subviews.first?.center = CGPoint(x: scrollView.contentSize.width*0.5 + offsetX, y: scrollView.contentSize.height*0.5 + offsetY)
```



代码如下：

```swift
class ViewController: UIViewController, UIScrollViewDelegate{
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let scrollView = UIScrollView()
        scrollView.frame = self.view.bounds
        self.view.addSubview(scrollView)
      
        let imageView = UIImageView()
        imageView.image = UIImage(named: "img")
        imageView.frame = CGRect(x: 0, y: 0, width: scrollView.frame.size.width, height: 400)
        scrollView.addSubview(imageView)
        imageView.center = scrollView.center
        imageView.isUserInteractionEnabled = true
        scrollView.isUserInteractionEnabled = true
        scrollView.delegate = self
        scrollView.maximumZoomScale = 4.0
        scrollView.minimumZoomScale = 0.2
        scrollView.bounces = false
        scrollView.contentSize = imageView.image!.size
        
        
    }
    
    // 在此决定scrollView中的哪个控件放大缩小
    func viewForZooming(in scrollView: UIScrollView) -> UIView? {
        return scrollView.subviews.first
    }
    
    func scrollViewDidZoom(_ scrollView: UIScrollView) {
        print("scrollViewDIdZoom")
        // 在此解决缩放不居中的问题
        let offsetX = max((scrollView.bounds.size.width - scrollView.contentInset.left - scrollView.contentInset.right - scrollView.contentSize.width)*0.5, 0.0)
        let offsetY = max((scrollView.bounds.size.height - scrollView.contentInset.top - scrollView.contentInset.bottom - scrollView.contentSize.height)*0.5, 0.0)
        scrollView.subviews.first?.center = CGPoint(x: scrollView.contentSize.width*0.5 + offsetX, y: scrollView.contentSize.height*0.5 + offsetY)
    }
}
```



