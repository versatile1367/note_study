# ios本地存储方案

> 学习地址：https://juejin.cn/post/6844904199688290318#heading-1
>
> https://www.jianshu.com/p/9b27259c3409



### 沙盒

#### 关于沙盒

应用沙箱是macOS提供的一种访问控制技术，在内核级别执行。如果应用程序受到威胁，它的目的是防止系统和用户数据受到损害。通过Mac应用商店发布的应用必须采用应用沙箱。通过开发者ID在Mac应用商店外签名和分发的应用程序也可以(在大多数情况下应该)使用应用沙箱

mac上沙盒的路径：`~/Library/Containers/applicationName`

沙盒路径下几个重要的文件夹：

- `Documents`: 用来存储需要持久化的数据
- `Library/Caches`: 缓存，内存不足、应用没有运行时会被清除一般存储体积大、不需要备份的非重要数据
- `Library/Preference`: 可以用来存储一些偏好设置



### plist存储

1. 本身是xml文件
2. 主要保存的数据类型为NSString、NSNumber、NSData、NSArray、NSDictionary



### Preference偏好设置

1. 进行一些设置的记录，比如用户名、开关是否打开等。
2. 利用`UserDefaults` 。通过键值对方式记录。
3. NSUserDefaults 是一个单例对象,在整个应用程序的生命周期中都只有一个实例。



### NSKeyedArchiver归档、NSKeyedUnarchiver解档

1. 归档和解档会在写入、读出数据之前进行序列化、反序列化，数据的安全性相对高一些。
2. 需要归档解档的类需要遵守NSCoding协议
3. 实现归档的四个步骤
   - 遵守NSCoding协议
   - 实现encodeWithCoder和initWithCoder方法
   - 归档方法
   - 解档方法



### SQLite使用



### FMDB



### CoreData



### KeyChain

1. 提供了一种安全的保存私密信息（密码，序列号，证书等）的方式。每个iOS程序都有一个独立的KeyChain存储。
2. 当应用程序被删除后，保存到KeyChain里面的数据不会被删除，所以keyChain是保存到沙盒范围以外的地方.
3. 所有数据也都是以key-value的形式存储的







