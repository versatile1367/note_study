# swift入门学习笔记-2



### 构造过程

#### 基本

1. 类和结构体在创建实例时，必须为所有存储型属性设置合适的初始值。存储型属性的值不能处于一个未知的状态。

2. 存储型属性分配默认值或者在构造器中设置初始值时，它们的值是被直接设置的，不会触发任何属性观察者。如果一个属性总是使用相同的初始值，那么为其设置一个默认值比每次都在构造器中赋值要好。

3. 调用构造器时，主要通过构造器中形参命名和类型来确定应该被调用的构造器。正因如此，如果在定义构造器时没有提供实参标签，Swift 会为构造器的*每个*形参自动生成一个实参标签。如果不通过实参标签传值，这个构造器是没法调用的。如果构造器定义了某个实参标签，就必须使用**实参标签**，忽略它将导致编译期错误。

4. 如果不希望构造器的某个形参使用实参标签，可以使用下划线（`_`）来代替显式的实参标签来重写默认行为。这样在构造时传参就不要实参标签了。

5. 可选属性类型：当一个允许值为空的存储型属性，类型应为可选类型。可选类型的属性将自动初始化为 `nil`，表示这个属性是特意在构造过程设置为空。

6. 构造过程中的常量属性赋值：可以在构造过程中的任意时间点给常量属性赋值，只要在构造过程结束时它设置成确定的值。一旦常量属性被赋值，它将永远不可更改。常量属性只能在定义它的类的构造过程中修改；不能在子类中修改。

7. 默认构造器

   - 如果结构体或类为所有属性提供了默认值，又没有提供任何自定义的构造器，那么 Swift 会给这些结构体或类提供一个*默认构造器*。这个默认构造器将简单地创建一个所有属性值都设置为它们默认值的实例。
   - **结构体**的逐一成员构造器：结构体如果没有定义任何自定义构造器，它们将自动获得一个*逐一成员构造器（memberwise initializer）*。不像默认构造器，即使存储型属性没有默认值，结构体也能会获得逐一成员构造器。新实例的属性初始值可以通过名字传入逐一成员构造器中。调用一个逐一成员构造器（memberwise initializer）时，可以省略任何一个有默认值的属性。

8. 值类型的构造器代理

   - 如果为某个值类型定义了一个自定义的构造器，将无法访问到默认构造器（如果是结构体，还将无法访问逐一成员构造器）

   - 如果希望默认构造器、逐一成员构造器以及自定义的构造器都能用来创建实例，可以将自定义的构造器写到扩展（`extension`）中，而不是写在值类型的原始定义中。

   - 对于值类型，可以使用 `self.init` 在自定义的构造器中引用相同类型中的其它构造器。并且只能在构造器内部调用 `self.init`。

     ```swift
     struct Rect {
         var origin = Point()
         var size = Size()
         init() {}
     
         init(origin: Point, size: Size) {
             self.origin = origin
             self.size = size
         }
     
         init(center: Point, size: Size) {
             let originX = center.x - (size.width / 2)
             let originY = center.y - (size.height / 2)
             self.init(origin: Point(x: originX, y: originY), size: size)
         }
     }
     ```



#### 类的继承和构造过程

1. 类里面的所有存储型属性——包括所有继承自父类的属性——都*必须*在构造过程中设置初始值。
2. 指定构造器和便利构造器

   - 每一个类都必须至少拥有一个指定构造器。
   - 可以定义便利构造器来调用同一个类中的指定构造器，并为部分形参提供默认值。也可以定义便利构造器来创建一个特殊用途或特定输入值的实例。便利构造器就在`init` 前面加上`convenience`即可
   - 指定构造器和便利构造器之间的调用关系
     - 指定构造器必须调用其直接父类的的指定构造器。
     - 便利构造器必须调用*同*类中定义的其它构造器。
     - 便利构造器最后必须调用指定构造器。
3. 两段式构造过程
   - 类的构造过程包含两个阶段。第一个阶段，类中的每个存储型属性赋一个初始值。当每个存储型属性的初始值被赋值后，第二阶段开始，它给每个类一次机会，在新实例准备使用之前进一步自定义它们的存储型属性。
   - 4个安全检查+2个阶段

4. Swift 中的子类默认情况下**不会继承父类的构造器**。父类的构造器仅会在安全和适当的某些情况下被继承。

5. 重写一个父类的**指定构造器**时，总是需要写 `override` 修饰符，即使是为了实现子类的便利构造器；而如果编写了一个和父类**便利构造器**相匹配的子类构造器，由于子类不能直接调用父类的便利构造器，因此，严格意义上来讲，你的子类并未对一个父类构造器提供重写。所以，在子类中“重写”一个父类便利构造器时，不需要加 `override` 修饰符。

   ```
   class Vehicle {
       var numberOfWheels = 0
       var description: String {
           return "\(numberOfWheels) wheel(s)"
       }
   }
   
   class Bicycle: Vehicle {
       override init() {
           super.init()
           numberOfWheels = 2
       }
   }
   ```

   如果子类的构造器没有在阶段 2 过程中做自定义操作，并且父类有一个无参数的指定构造器，你可以在**所有子类的存储属性赋值之后**省略 `super.init()` 的调用。(注意在安全检查1中：指定构造器必须保证它所在类的所有属性都必须先初始化完成，之后才能将其它构造任务向上代理给父类中的构造器。)

   ```swift
   class Hoverboard: Vehicle {
       var color: String
       init(color: String) {
           self.color = color  //应该先处理完类自己的存储属性，再去向上构造！
           // super.init() 在这里被隐式调用
       }
       override var description: String {
           return "\(super.description) in a beautiful \(color)"
       }
   }
   ```

   子类可以在构造过程修改继承来的变量属性，但是不能修改继承来的常量属性。

6. 构造器的自动继承

   - 如果为子类中引入的所有新属性都提供了默认值，那么：
     -  如果子类没有定义任何指定构造器，它将自动继承父类所有的指定构造器。
     -  如果子类提供了所有父类指定构造器的实现——无论是通过规则 1 继承过来的，还是提供了自定义实现——它将自动继承父类所有的便利构造器。

7. Eg:

   ```swift
   class Food {
       var name: String
       init(name: String) {
           self.name = name
       }
   
       convenience init() {
           self.init(name: "[Unnamed]")
       }
   }
   
   class RecipeIngredient: Food {
       var quantity: Int
       init(name: String, quantity: Int) {
           self.quantity = quantity
           super.init(name: name)
       }
       //使用了跟父类中指定构造器相同的形参，由于重写了父类的指定构造，所以加上了override
       override convenience init(name: String) { 
           self.init(name: name, quantity: 1)
       }
   }
   
   let oneMysteryItem = RecipeIngredient()
   let oneBacon = RecipeIngredient(name: "Bacon")
   let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
   
   // 为引入的所有属性都提供了默认值，并且没有定义任何构造器，将自动继承所有父类中的指定构造器和便利构造器。
   class ShoppingListItem: RecipeIngredient {
       var purchased = false
       var description: String {
           var output = "\(quantity) x \(name)"
           output += purchased ? " ✔" : " ✘"
           return output
       }
   }
   ```

   尽管 `RecipeIngredient` 将父类的指定构造器重写为了便利构造器，但是它依然提供了父类的所有指定构造器的实现。因此，`RecipeIngredient` 会自动继承父类的所有便利构造器。

   <img src="img/截屏2020-11-26 下午8.43.30.png" alt="截屏2020-11-26 下午8.43.30" style="zoom:33%;" align="left"/>



#### 可失败构造器

1. 定义一个构造器可失败的类，结构体或者枚举是很有用的。这里所指的“失败” 指的是，如给构造器传入无效的形参，或缺少某种所需的外部资源，又或是不满足某种必要的条件等。其语法为在 `init` 关键字后面添加问号（`init?`）。

2. 可失败构造器的参数名和参数类型，不能与其它非可失败构造器的参数名，及其参数类型相同。

3. 可失败构造器会创建一个类型为自身类型的可选类型的对象。你通过 `return nil` 语句来表明可失败构造器在何种情况下应该 “失败”。

4. 构造器都不支持返回值，因此只是用 `return nil` 表明可失败构造器构造失败，而不要用关键字 `return` 来表明构造成功。

   ```swift
   struct Animal {
       let species: String
       init?(species: String) {
           if species.isEmpty {
               return nil
           }
           self.species = species
       }
   }
   
   let someCreature = Animal(species: "Giraffe")
   // someCreature 的类型是 Animal? 而不是 Animal
   ```

   注意：检查空字符串的值（如 `""`，而不是 `"Giraffe"` ）和检查值为 `nil` 的可选类型的字符串是两个完全不同的概念。

5. 枚举类型的可失败构造器：

   ```swift
   enum TemperatureUnit {
       case Kelvin, Celsius, Fahrenheit
       init?(symbol: Character) {
           switch symbol {
           case "K":
               self = .Kelvin
           case "C":
               self = .Celsius
           case "F":
               self = .Fahrenheit
           default:
               return nil
           }
       }
   }
   ```

6. 带原始值的枚举类型的可失败构造器：带原始值的枚举类型会自带一个可失败构造器 `init?(rawValue:)`，该可失败构造器有一个合适的原始值类型的 `rawValue` 形参，选择找到的相匹配的枚举成员，找不到则构造失败。

7. 构造失败的传递：

   - 类、结构体、枚举的可失败构造器可以横向代理到它们自己其他的可失败构造器。类似的，子类的可失败构造器也能向上代理到父类的可失败构造器。

   - 代理到的其他可失败构造器触发构造失败，整个构造过程将立即终止，接下来的任何构造代码不会再被执行。

   - 可失败构造器也可以代理到其它的不可失败构造器。通过这种方式，你可以增加一个可能的失败状态到现有的构造过程中。

     ```swift
     class Product {
         let name: String
         init?(name: String) {
             if name.isEmpty { return nil }
             self.name = name
         }
     }
     
     class CartItem: Product {
         let quantity: Int
         init?(name: String, quantity: Int) {
             if quantity < 1 { return nil }
             self.quantity = quantity
             super.init(name: name)
         }
     }
     ```

8. 重写可失败构造器：

   - 既可以在子类中重写，也可以用子类的非可失败重写父类的可失败。

   - 用子类的非可失败构造器重写父类的可失败构造器时，向上代理到父类的可失败构造器的唯一方式是对父类的可失败构造器的返回值进行强制解包。

   - 可以用非可失败构造器重写可失败构造器，但反过来却不行。

     ```swift
     class Document {
         var name: String?
         // 该构造器创建了一个 name 属性的值为 nil 的 document 实例
         init() {}
         // 该构造器创建了一个 name 属性的值为非空字符串的 document 实例
         init?(name: String) {
             if name.isEmpty { return nil }
             self.name = name
         }
     }
     
     // 重写了所有父类引入的指定构造器,用不可失败重写了父类的可失败
     class AutomaticallyNamedDocument: Document {
         override init() {
             super.init()
             self.name = "[Untitled]"
         }
         override init(name: String) {
             super.init()
             if name.isEmpty {
                 self.name = "[Untitled]"
             } else {
                 self.name = name
             }
         }
     }
     ```

     可以在子类的不可失败构造器中使用强制解包来调用父类的可失败构造器。

     ```swift
     class UntitledDocument: Document {
         override init() {
             super.init(name: "[Untitled]")!
         }
     }
     ```

9. init! 可失败构造器



#### 必要构造器

1. 在类的构造器前添加 `required` 修饰符表明所有该类的子类都必须实现该构造器
2. 在子类重写父类的必要构造器时，必须在子类的构造器前也添加 `required` 修饰符，表明该构造器要求也应用于继承链后面的子类。在重写父类中必要的指定构造器时，不需要添加 `override` 修饰符
3. 如果子类继承的构造器能满足必要构造器的要求，则无须在子类中显式提供必要构造器的实现。



#### 通过闭包或函数设置属性的默认值

1. 如果某个存储型属性的默认值需要一些自定义或设置，你可以使用闭包或全局函数为其提供定制的默认值。每当某个属性所在类型的新实例被构造时，对应的闭包或函数会被调用，而它们的返回值会当做默认值赋值给这个属性。

   ```swift
   class SomeClass {
       let someProperty: SomeType = {
           // 在这个闭包中给 someProperty 创建一个默认值
           // someValue 必须和 SomeType 类型相同
           return someValue
       }()
   }
   ```

   闭包结尾的花括号后面接了一对空的小括号。这用来告诉 Swift 立即执行此闭包。如果忽略了这对括号，相当于将闭包本身作为值赋值给了属性，而不是将闭包的返回值赋值给属性。

2. 在闭包执行时，实例的其它部分都还没有初始化。这意味着不能在闭包里访问其它属性，即使这些属性有默认值。同样，也不能使用隐式的 `self` 属性，或者调用任何实例方法。





### 析构过程

1. 用`deinit` 来表示析构：

   ```swift
   deinit {
       // 执行析构过程
   }
   ```

2. 子类继承了父类的析构器，并且在子类析构器实现的最后，父类的析构器会被自动调用。即使子类没有提供自己的析构器，父类的析构器也同样会被调用。

3. 析构器是在实例释放发生前被自动调用的,不能主动调用析构器

4. 例如可以将某实例（可选类型）置为`nil` ,此时就会触发析构



### 可选链

1. *可选链式调用*是一种可以在当前值可能为 `nil` 的可选值上请求和调用属性、方法及下标的方法。如果可选值有值，那么调用就会成功；如果可选值是 `nil`，那么调用将返回 `nil`。多个调用可以连接在一起形成一个调用链，如果其中任何一个节点为 `nil`，整个调用链都会失败，即返回 `nil`。

2. 通过在想调用的属性、方法，或下标的可选值后面放一个问号（`?`），可以定义一个可选链。

3. 当可选值为空时可选链式调用只会**调用失败**，然而强制解包将会触发**运行时错误**。

4. 不论这个调用的属性、方法及下标返回的值是不是可选值，它的返回结果都是一个可选值。可以利用这个返回值来判断你的可选链式调用是否调用成功，如果调用有返回值则说明调用成功，返回 `nil` 则说明调用失败。

   ```swift
   class Person {
       var residence: Residence?
   }
   
   class Residence {
       var numberOfRooms = 1
   }
   
   let john = Person()
   
   let roomCount = john.residence!.numberOfRooms
   // 这会引发运行时错误
   
   if let roomCount = john.residence?.numberOfRooms {
       print("John's residence has \(roomCount) room(s).")
   } else {
       print("Unable to retrieve the number of rooms.")
   }
   // 打印“Unable to retrieve the number of rooms.”
   ```

   使用可选链式调用就意味着 `numberOfRooms` 会返回一个 `Int?` 而不是 `Int`

5. 当用可选链式调用来调用方法时，如果方法本来返回的是`Void`,那么在可选链式调用来调用时返回类型会是`Void?`

6. 通过可选链式调用给属性赋值会返回 `Void?`，通过判断返回值是否为 `nil` 就可以知道赋值是否成功

7. 通过可选链式调用访问可选值的下标时，应该将问号放在下标方括号的前面而不是后面。可选链式调用的问号一般直接跟在可选表达式的后面。

8. 如果下标返回可选类型值，比如 Swift 中 `Dictionary` 类型的键的下标，可以在下标的结尾括号后面放一个问号来在其可选返回值上进行可选链式调用

   ```swift
   var testScores = ["Dave": [86, 82, 84], "Bev": [79, 94, 81]]
   testScores["Dave"]?[0] = 91
   testScores["Bev"]?[0] += 1
   testScores["Brian"]?[0] = 72
   // "Dave" 数组现在是 [91, 82, 84]，"Bev" 数组现在是 [80, 94, 81]
   ```

9. 可以通过连接多个可选链式调用在更深的模型层级中访问属性、方法以及下标。然而，多层可选链式调用不会增加返回值的可选层级。通过可选链式调用访问一个 `Int` 值，将会返回 `Int?`，无论使用了多少层可选链式调用;通过可选链式调用访问 `Int?` 值，依旧会返回 `Int?` 值，并不会返回 `Int??`

10. 在方法的可选返回值上进行可选链式调用，在方法的圆括号后面加上问号即可。





### 错误处理

1. 错误用遵循 `Error` 协议的类型的值来表示,这个空协议表明该类型可以用于错误处理。

2. Swift 的枚举类型尤为适合构建一组相关的错误状态，枚举的关联值还可以提供错误状态的额外信息。

   ```swift
   enum VendingMachineError: Error {
       case invalidSelection                     //选择无效
       case insufficientFunds(coinsNeeded: Int) //金额不足
       case outOfStock                             //缺货
   }
   ```

3. 抛出错误使用 `throw` 语句

4. Swift 中的错误处理并不涉及解除调用栈，这是一个计算代价高昂的过程。就此而言，`throw` 语句的性能特性是可以和 `return` 语句相媲美的。

5. 处理错误

   - 用 throwing 函数传递错误

     - 为了表示一个函数、方法或构造器可以抛出错误，在函数声明的参数之后加上 `throws` 关键字。一个标有 `throws` 关键字的函数被称作 *throwing 函数*。如果这个函数指明了返回值类型，`throws` 关键词需要写在返回箭头（`->`）的前面。
     - 只有 throwing 函数可以传递错误。任何在某个非 throwing 函数内部抛出的错误只能在函数内部处理。
     - 在调用throwing函数的地方，要么直接处理这些错误，要么继续传递下去，如果想要继续传递下去，调用的地方加上`try` ,同时定义外围函数为throwing函数，可以在throwing构造器中调用throwing函数

   - 用do-catch处理错误

     - 使用一个 `do-catch` 语句运行一段闭包代码来处理错误。如果在 `do` 子句中的代码抛出了一个错误，这个错误会与 `catch` 子句做匹配，从而决定哪条子句能处理它。
     
       ```swift
       do {
           try expression
           statements
       } catch pattern 1 {
           statements
       } catch pattern 2 where condition {
           statements
       } catch pattern 3, pattern 4 where condition {
           statements
       } catch {
           statements
       }
       ```
     
       如果一条 `catch` 子句没有指定匹配模式，那么这条子句可以匹配任何错误，并且把错误绑定到一个**名字为 `error` 的局部常量**。
     
     - 如果没有错误被抛出，`do` 子句中余下的语句就会被执行。抛出错误后，立刻转到`catch` 子句中判断错误是否要被继续传递。
     
     - 如果所有`catch` 语句中都未处理错误，就会传递到周围的作用域，在不会抛出错误的函数中，必须用`do-catch` 语句处理。
     
     - 能够抛出错误的函数既可以使用 `do-catch` 语句处理，也可以让调用方来处理错误。如果错误传递到了顶层作用域却依然没有被处理，你会得到一个运行时错误。
     
     - 捕获多个相关错误的方式是将它们放在 `catch` 后，通过逗号分隔
     
   - 将错误转换可选值
   
     - 使用 `try?` 通过将错误转换成一个可选值来处理错误。如果是在计算 `try?` 表达式时抛出错误，该表达式的结果就为 `nil`
   
       ```swift
       func someThrowingFunction() throws -> Int {
           // ...
       }
       
       let x = try? someThrowingFunction()
       
       let y: Int?
       do {
           y = try someThrowingFunction()
       } catch {
           y = nil
       }
       ```
   
     - 如果对所有错误都采用同样方式，`try?` 可以写出简介代码。
   
       ```swift
       func fetchData() -> Data? {
           if let data = try? fetchDataFromDisk() { return data }
           if let data = try? fetchDataFromServer() { return data }
           return nil
       }
       ```
   
   - 禁用错误传递：某个 `throwing` 函数实际上在运行时是不会抛出错误的，在这种情况下，可以在表达式前面写 `try!` 来禁用错误传递，这会把调用包装在一个不会有错误抛出的运行时断言中。如果真的抛出了错误，会得到一个运行时错误。
   
     ```swift
     let photo = try! loadImage(atPath: "./Resources/John Appleseed.jpg")
     ```
   
6. 指定清理操作

   - 使用 `defer` 语句在即将离开当前代码块时执行一系列语句
   - 由 `defer` 关键字和要被延迟执行的语句组成。延迟执行的语句不能包含任何控制转移语句，例如 `break`、`return` 语句，或是抛出一个错误。
   - 延迟执行的操作会按照它们声明的顺序从后往前执行
   - 即使没有涉及到错误处理的代码，也可以使用 `defer` 语句。



### 类型转换

1. 用*类型检查操作符*（`is`）来检查一个实例是否属于特定子类型。若实例属于那个子类型，类型检查操作符返回 `true`，否则返回 `false`。

2. 某类型的一个常量或变量可能在幕后实际上属于一个子类。当确定是这种情况时，你可以尝试用*类型转换操作符*（`as?` 或 `as!`）向下转到它的子类型。

3. 条件形式 `as?` 返回一个你试图向下转成的类型的可选值。强制形式 `as!` 把试图向下转型和强制解包转换结果结合为一个操作。

   ```swift
   for item in library {
       if let movie = item as? Movie {
           print("Movie: \(movie.name), dir. \(movie.director)")
       } else if let song = item as? Song {
           print("Song: \(song.name), by \(song.artist)")
       }
   }
   ```

   转换没有真的改变实例或它的值。根本的实例保持不变；只是简单地把它作为它被转换成的类型来使用。

4. 不确定类型提供了两种特殊别名：

   - `Any` 可以表示任何类型，包括函数类型。

   - `AnyObject` 可以表示任何类类型的实例。

     ```swift
     var things = [Any]()
     for thing in things {
         switch thing {
         case 0 as Int:
             print("zero as an Int")
         case 0 as Double:
             print("zero as a Double")
         case let someInt as Int:
             print("an integer value of \(someInt)")
         case let someDouble as Double where someDouble > 0:
             print("a positive double value of \(someDouble)")
         case is Double:
             print("some other double value that I don't want to print")
         case let someString as String:
             print("a string value of \"\(someString)\"")
         case let (x, y) as (Double, Double):
             print("an (x, y) point at \(x), \(y)")
         case let movie as Movie:
             print("a movie called \(movie.name), dir. \(movie.director)")
         case let stringConverter as (String) -> String:
             print(stringConverter("Michael"))
         default:
             print("something else")
         }
     }
     ```

5. `Any` 类型可以表示所有类型的值，包括可选类型。Swift 会在你用 `Any` 类型来表示一个可选值的时候，给你一个警告。如果你确实想使用 `Any` 类型来承载可选值，你可以使用 `as` 操作符显式转换为 `Any`.

   ```swift
   let optionalNumber: Int? = 3
   things.append(optionalNumber)        // 警告
   things.append(optionalNumber as Any) // 没有警告
   ```



### 嵌套类型

1. 可以在支持的类型中定义嵌套的枚举、类和结构体。



### 扩展

1. swift中的扩展可以：
   - 添加计算型实例属性和计算型类属性
   - 定义实例方法和类方法
   - 提供新的构造器
   - 定义下标
   - 定义和使用新的嵌套类型
   - 使已经存在的类型遵循（conform）一个协议

2. 扩展可以给一个类型添加新的功能，但是不能重写已经存在的功能。

   ```swift
   extension SomeType {
     // 在这里给 SomeType 添加新的功能
   }
   ```

3. 扩展可以扩充一个现有的类型，给它添加一个或多个协议。协议名称的写法和类或者结构体一样

   ```swift
   extension SomeType: SomeProtocol, AnotherProtocol {
     // 协议所需要的实现写在这里
   }
   ```

4. 对一个现有的类型，如果你定义了一个扩展来添加新的功能，那么这个类型的所有实例都可以使用这个新功能，包括那些在扩展定义之前就存在的实例。

5. 扩展添加计算型属性：

   ```swift
   extension Double {
       var km: Double { return self * 1_000.0 }
       var m: Double { return self }
       var cm: Double { return self / 100.0 }
       var mm: Double { return self / 1_000.0 }
       var ft: Double { return self / 3.28084 }
   }
   let oneInch = 25.4.mm
   print("One inch is \(oneInch) meters")
   // 打印“One inch is 0.0254 meters”
   let threeFeet = 3.ft
   print("Three feet is \(threeFeet) meters")
   // 打印“Three feet is 0.914399970739201 meters”
   let aMarathon = 42.km + 195.m
   print("A marathon is \(aMarathon) meters long")
   // 打印“A marathon is 42195.0 meters long”
   ```

6. 扩展可以添加新的计算属性，但是它们不能添加存储属性，或向现有的属性添加属性观察者。

7. 可以给一个类添加新的便利构造器，但是它们不能给类添加新的指定构造器或者析构器。指定构造器和析构器必须始终由类的原始实现提供。

8. 如果使用扩展给一个值类型添加构造器，而这个值类型已经为所有存储属性提供默认值，且没有定义任何自定义构造器，那么可以在该值类型扩展的构造器中使用默认构造器和成员构造器。如果已经将构造器写在值类型的原始实现中，则不适用于这种情况。

9. 使用扩展给另一个模块中定义的结构体添加构造器，那么新的构造器直到定义模块中使用一个构造器之前，不能访问 `self`。

10. 扩展添加方法：

    ```swift
    extension Int {
        func repetitions(task: () -> Void) {
            for _ in 0..<self {
                task()
            }
        }
    }
    
    3.repetitions {
        print("Hello!")
    }
    ```

11. 扩展添加可变实例方法:

    ```swift
    extension Int {
        mutating func square() {
            self = self * self
        }
    }
    var someInt = 3
    someInt.square()
    // someInt 现在是 9
    ```

12. 扩展添加新下标：

    ```swift
    extension Int {
        subscript(digitIndex: Int) -> Int {
            var decimalBase = 1
            for _ in 0..<digitIndex {
                decimalBase *= 10
            }
            return (self / decimalBase) % 10
        }
    }
    746381295[0]
    // 返回 5
    746381295[1]
    // 返回 9
    746381295[2]
    // 返回 2
    746381295[8]
    // 返回 7
    ```





### 协议

1. *协议* 定义了一个蓝图，规定了用来实现某一特定任务或者功能的方法、属性，以及其他需要的东西。类、结构体或枚举都可以遵循协议，并为协议定义的这些要求提供具体实现。某个类型能够满足某个协议的要求，就可以说该类型*遵循*这个协议。

2. 协议语法：

   ```swift
   protocol SomeProtocol {
       // 这里是协议的定义部分
   }
   
   struct SomeStructure: FirstProtocol, AnotherProtocol {
       // 这里是结构体的定义部分
   }
   
   // 父类名放在遵循的协议名之前
   class SomeClass: SomeSuperClass, FirstProtocol, AnotherProtocol {
       // 这里是类的定义部分
   }
   ```

3. 协议可以要求遵循协议的类型提供特定名称和类型的**实例属性或类型属性**。

   - 协议不指定属性是存储属性还是计算属性，它只指定属性的名称和类型。

   - 协议还指定属性是*可读*的还是可读可写的 ：如果协议要求属性是可读可写的，那么该属性不能是常量属性或只读的计算型属性。如果协议只要求属性是可读的，那么该属性不仅可以是可读的，如果代码需要的话，还可以是可写的。

   - 协议总是用 `var` 关键字来声明变量属性，在类型声明后加上 `{ set get }` 来表示属性是*可读可写*的，*可读*属性则用 `{ get }` 来表示。

     ```swift
     protocol SomeProtocol {
         var mustBeSettable: Int { get set }
         var doesNotNeedToBeSettable: Int { get }
     }
     ```

4. 在协议中定义类型属性时，总是使用 `static` 关键字作为前缀。当类类型遵循协议时，除了 `static` 关键字，还可以使用 `class` 关键字来声明类型属性

5. 协议可以要求遵循协议的类型实现某些指定的**实例方法或类方法**。在协议定义中的方法不需要大括号和方法体，可以在协议中定义具有可变参数的方法，不支持为协议中的方法提供默认参数。

   ```swift
   protocol SomeProtocol {
       static func someTypeMethod()
     	func random() -> Double
   }
   ```

6. 异变方法要求：如果你在协议中定义了一个实例方法，该方法会改变遵循该协议的类型的实例，那么在定义协议时需要在方法前加 `mutating` 关键字。这使得结构体和枚举能够遵循此协议并满足此方法要求。（结构体和枚举需要加`mutating` ，类不需要）

7. 构造器要求：

   - 在遵循协议的类中实现构造器，无论是作为指定构造器，还是作为便利构造器。无论哪种情况，都必须为构造器实现标上 `required` 修饰符。使用 `required` 修饰符可以确保所有子类也必须提供此构造器实现，从而也能遵循协议

   - 如果类已经被标记为 `final`，那么不需要在协议构造器的实现中使用 `required` 修饰符，因为 `final` 类不能有子类。

   - 如果一个子类重写了父类的指定构造器，并且该构造器满足了某个协议的要求，那么该构造器的实现需要同时标注 `required` 和 `override` 修饰符

     ```swift
     protocol SomeProtocol {
         init()
     }
     
     class SomeSuperClass {
         init() {
             // 这里是构造器的实现部分
         }
     }
     
     class SomeSubClass: SomeSuperClass, SomeProtocol {
         // 因为遵循协议，需要加上 required
         // 因为继承自父类，需要加上 override
         required override init() {
             // 这里是构造器的实现部分
         }
     }
     ```

   - 可失败构造器要求

     - 协议还可以为遵循协议的类型定义可失败构造器要求
     - 遵循协议的类型可以通过可失败构造器（`init?`）或非可失败构造器（`init`）来满足协议中定义的可失败构造器要求。协议中定义的非可失败构造器要求可以通过非可失败构造器（`init`）或隐式解包可失败构造器（`init!`）来满足。

8. 协议作为类型：

   - 协议可以像其他普通类型一样使用，使用场景如下：
     - 作为函数、方法或构造器中的参数类型或返回值类型
     - 作为常量、变量或属性的类型
     - 作为数组、字典或其他容器中的元素类型

9. 委托

   - 是一种设计模式，它允许类或结构体将一些需要它们负责的功能委托给其他类型的实例
   - 委托模式的实现：定义协议来封装那些需要被委托的功能，这样就能确保遵循协议的类型能提供这些功能。委托模式可以用来响应特定的动作，或者接收外部数据源提供的数据，而无需关心外部数据源的类型。

10. 在扩展里添加协议遵循：通过扩展令已有类型遵循并符合协议时，该类型的所有实例也会随之获得协议中定义的各项功能。

11. 有条件地遵循协议：泛型类型可能只在某些情况下满足一个协议的要求，比如当类型的泛型形式参数遵循对应协议时。你可以通过在扩展类型时列出限制让泛型类型有条件地遵循某协议。在你采纳协议的名字后面写泛型 `where` 分句。

    ```swift
    extension Array: TextRepresentable where Element: TextRepresentable
    ```

12. 在扩展里声明采纳协议：当一个类型已经遵循了某个协议中的所有要求，却还没有声明采纳该协议时，可以通过空的扩展来让它采纳该协议。

    ```swift
    struct Hamster {
        var name: String
           var textualDescription: String {
            return "A hamster named \(name)"
        }
    }
    extension Hamster: TextRepresentable {}
    ```

    从现在起，`Hamster` 的实例可以作为 `TextRepresentable` 类型使用

    即使满足了协议的所有要求，类型也不会自动遵循协议，必须**显式地遵循协议**。

13. 使用合成协议来采纳协议：

    - Swift 可以自动提供一些简单场景下遵循 `Equatable`、`Hashable` 和 `Comparable` 协议的实现。在使用这些合成实现之后，无需再编写重复的代码来实现这些协议所要求的方法。
    - Swift 为以下几种自定义类型提供了 `Equatable` 协议的合成实现: (在包含类型原始声明的文件中声明对 `Equatable` 协议的遵循，可以得到 `==` 操作符的合成实现，且无需自己编写任何关于 `==` 的实现代码。`Equatable` 协议同时包含 `!=` 操作符的默认实现。)
      - 遵循 `Equatable` 协议且只有存储属性的结构体。
      - 遵循 `Equatable` 协议且只有关联类型的枚举
      - 没有任何关联类型的枚举
    - Swift 为以下几种自定义类型提供了 `Hashable` 协议的合成实现： (在包含类型原始声明的文件中声明对 `Hashable` 协议的遵循，可以得到 `hash(into:)` 的合成实现，且无需自己编写任何关于 `hash(into:)` 的实现代码。)
      - 遵循 `Hashable` 协议且只有存储属性的结构体。
      - 遵循 `Hashable` 协议且只有关联类型的枚举
      - 没有任何关联类型的枚举
    - Swift 为没有原始值的枚举类型提供了 `Comparable` 协议的合成实现。如果枚举类型包含关联类型，那这些关联类型也必须同时遵循 `Comparable` 协议。在包含原始枚举类型声明的文件中声明其对 `Comparable` 协议的遵循，可以得到 `<` 操作符的合成实现，且无需自己编写任何关于 `<` 的实现代码。`Comparable` 协议同时包含 `<=`、`>` 和 `>=` 操作符的默认实现。

14. 协议的继承：协议能够*继承*一个或多个其他协议，可以在继承的协议的基础上增加新的要求。协议的继承语法与类的继承相似，多个被继承的协议间用逗号分隔

15. 类专属的协议：通过添加 `AnyObject` 关键字到协议的继承列表，就可以限制协议只能被类类型采纳（以及非结构体或者非枚举的类型）

16. 协议合成：协议组合使用 `SomeProtocol & AnotherProtocol` 的形式。你可以列举任意数量的协议，用和符号（`&`）分开。除了协议列表，协议组合也能包含类类型，这许你标明一个需要的父类。

17. 检查协议的一致性：

    - `is` 用来检查实例是否遵循某个协议，若遵循则返回 `true`，否则返回 `false`；
    - `as?` 返回一个可选值，当实例遵循某个协议时，返回类型为协议类型的可选值，否则返回 `nil`；
    - `as!` 将实例强制向下转换到某个协议类型，如果强转失败，将触发运行时错误。

18. 可选的协议要求：

    - 协议可以定义*可选要求*，遵循协议的类型可以选择是否实现这些要求。在协议中使用 `optional` 关键字作为前缀来定义可选要求。
    - 可选要求用在你需要和 Objective-C 打交道的代码中。协议和可选要求都必须带上 `@objc` 属性。标记 `@objc` 特性的协议只能被继承自 Objective-C 类的类或者 `@objc` 类遵循，其他类以及结构体和枚举均不能遵循这种协议。
    - 使用可选要求时（例如，可选的方法或者属性），它们的类型会自动变成可选的。比如，一个类型为 `(Int) -> String` 的方法会变成 `((Int) -> String)?`。需要注意的是整个函数类型是可选的，而不是函数的返回值。
    - 协议中的可选要求可通过可选链式调用来使用，因为遵循协议的类型可能没有实现这些可选要求。类似 `someOptionalMethod?(someArgument)` 这样，你可以在可选方法名称后加上 `?` 来调用可选方法。

19. 协议扩展：

    - 协议可以通过扩展来为遵循协议的类型提供属性、方法以及下标的实现。通过这种方式，你可以基于协议本身来实现这些功能，而无需在每个遵循协议的类型中都重复同样的实现，也无需使用全局函数。

    - 通过协议扩展，所有遵循协议的类型，都能自动获得这个扩展所增加的方法实现而无需任何额外修改

    - 协议扩展可以为遵循协议的类型增加实现，但不能声明该协议继承自另一个协议。协议的继承只能在协议声明处进行指定。

    - 提供默认实现：可以通过协议扩展来为协议要求的方法、计算属性提供默认的实现。如果遵循协议的类型为这些要求提供了自己的实现，那么这些自定义实现将会替代扩展中的默认实现被使用。

    - 为协议扩展添加限制条件：

      - 在扩展协议的时候，可以指定一些限制条件，只有遵循协议的类型满足这些限制条件时，才能获得协议扩展提供的默认实现。这些限制条件写在协议名之后，使用 `where` 子句来描述

        ```swift
        extension Collection where Element: Equatable {
            func allEqual() -> Bool {
                for element in self {
                    if element != self.first {
                        return false
                    }
                }
                return true
            }
        }
        ```





