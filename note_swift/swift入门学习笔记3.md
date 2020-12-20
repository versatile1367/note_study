# swift入门学习笔记3

> https://swiftgg.gitbook.io/swift/swift-jiao-cheng/22_generics

### 泛型

1. 泛型函数

   ```swift
   func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
       let temporaryA = a
       a = b
       b = temporaryA
   }
   ```

   `swapTwoValues(_:_:)` 函数被调用时，`T` 所代表的类型都会由传入的值的类型推断出来。

   可提供多个类型参数，将它们都写在尖括号中，用逗号分开。

2. 命名类型参数：告诉阅读代码的人这些参数类型与泛型类型或函数之间的关系。始终使用大写字母开头的驼峰命名法（例如 `T` 和 `MyTypeParameter`）来为类型参数命名，以表明它们是占位类型，而不是一个值。

3. 泛型类型：自定义类、结构体和枚举可以适用于*任意类型*

4. 类型约束：

   ```swift
   func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
       // 这里是泛型函数的函数体部分
   }
   ```

5. 关联类型：

   定义一个协议时，声明一个或多个关联类型作为协议定义的一部分将会非常有用。关联类型为协议中的某个类型提供了一个占位符名称，其代表的实际类型在协议被遵循时才会被指定。关联类型通过 `associatedtype` 关键字来指定。

   ```swift
   protocol Container {
       associatedtype Item
       mutating func append(_ item: Item)
       var count: Int { get }
       subscript(i: Int) -> Item { get }
   }
   ```

   使用协议：

   ```swift
   struct Stack<Element>: Container {
       // Stack<Element> 的原始实现部分
       var items = [Element]()
       mutating func push(_ item: Element) {
           items.append(item)
       }
       mutating func pop() -> Element {
           return items.removeLast()
       }
       // Container 协议的实现部分
       mutating func append(_ item: Element) {
           self.push(item)
       }
       var count: Int {
           return items.count
       }
       subscript(i: Int) -> Element {
           return items[i]
       }
   }
   ```

   可以给关联类型添加约束、使用协议

6. 泛型where语句：

   - 对关联类型添加约束通常是非常有用的,可以通过定义一个泛型 `where` 子句来实现.

   - 可以在函数体或者类型的大括号之前添加 `where` 子句。

     ```swift
     func allItemsMatch<C1: Container, C2: Container>
         (_ someContainer: C1, _ anotherContainer: C2) -> Bool
         where C1.Item == C2.Item, C1.Item: Equatable {
     
             // 检查两个容器含有相同数量的元素
             if someContainer.count != anotherContainer.count {
                 return false
             }
     
             // 检查每一对元素是否相等
             for i in 0..<someContainer.count {
                 if someContainer[i] != anotherContainer[i] {
                     return false
                 }
             }
     
             // 所有元素都匹配，返回 true
             return true
     }
     ```

7. 具有泛型where子句的扩展：

   - 可以使用泛型`where` 子句作为扩展的一部分。

     ```swift
     extension Stack where Element: Equatable {
         func isTop(_ item: Element) -> Bool {
             guard let topItem = items.last else {
                 return false
             }
             return topItem == item
         }
     }
     ```

   - 可以使用泛型 `where` 子句去扩展一个协议

8. 包含上下文关系的where分句

   - 可以使用 `where` 分句为泛型添加下标，或为扩展方法添加泛型约束。

   - 可以在对应类型需要实现的对应的方法添加where来约束。只有满足where的条件的类型才有此方法。

     ```swift
     extension Container {
         func average() -> Double where Item == Int {
             var sum = 0.0
             for index in 0..<count {
                 sum += Double(self[index])
             }
             return sum / Double(count)
         }
         func endsWith(_ item: Item) -> Bool where Item: Equatable {
             return count >= 1 && self[count-1] == item
         }
     }
     ```

     也可以用扩展添加约束来实现，但是就需要实现两个扩展了！

9. 具有泛型where子句的关联类型

10. 泛型下标

    ```swift
    extension Container {
        subscript<Indices: Sequence>(indices: Indices) -> [Item]
            where Indices.Iterator.Element == Int {
                var result = [Item]()
                for index in indices {
                    result.append(self[index])
                }
                return result
        }
    }
    ```





### 不透明类型

1. 具有不透明返回类型的函数或方法会隐藏返回值的类型信息。函数不再提供具体的类型作为返回类型，而是根据它支持的协议来描述返回值。

   `some protocal`

   ```swift
   func makeTrapezoid() -> some Shape {
      //......
       return trapezoid
   }
   ```

2. 在处理模块和调用代码之间的关系时，隐藏类型信息非常有用，因为返回的底层数据类型仍然可以保持私有。

3. 不同于返回协议类型，不透明类型能保证类型一致性 —— 编译器能获取到类型信息，同时模块使用者却不能获取到。

4. 不透明类型和协议类型的区别：一个不透明类型只能对应一个具体的类型，即便函数调用者并不能知道是哪一种类型；协议类型可以同时对应多个类型，只要它们都遵循同一协议。

5. 有关联类型的协议不能作为返回类型。此时就可以用不透明类型例如`some container` 作为返回类型





### 自动引用计数

1. Weak:弱引用。弱引用不会保持所引用的实例，即使引用存在，实例也有可能被销毁。因此，ARC 会在引用的实例被销毁后自动将其弱引用赋值为 `nil`。并且因为弱引用需要在运行时允许被赋值为 `nil`，所以它们会被定义为可选类型变量，而不是常量。

2. 可以像其他可选值一样，检查弱引用的值是否存在，这样可以避免访问已销毁的实例的引用。

3. 当 ARC 设置弱引用为 `nil` 时，属性观察不会被触发。

4. Unowned:*无主引用*不会牢牢保持住引用的实例。和弱引用不同的是，无主引用在其他实例有相同或者更长的生命周期时使用。你可以在声明属性或者变量时，在前面加上关键字 `unowned` 表示这是一个无主引用。

5. 无主引用通常都被期望拥有值。不过 ARC 无法在实例被销毁后将无主引用设为 `nil`，因为非可选类型的变量不允许被赋值为 `nil`。

6. 使用无主引用，必须确保引用始终指向一个未销毁的实例。在实例被销毁后，访问该实例的无主引用，会触发运行时错误。

7. 场景1：`Person` 和 `Apartment` 的实例两个属性的值都允许为 `nil`，并会潜在的产生循环强引用。这种场景最适合用弱引用来解决。

   场景2：`Customer` 和 `CreditCard` 的实例中一个属性的值允许为 `nil`，而另一个属性的值不允许为 `nil`，这也可能会产生循环强引用。这种场景最适合通过无主引用来解决。

   场景3：两个属性都必须有值，并且初始化完成后永远不会为 `nil`。在这种场景中，需要一个类使用无主属性，而另外一个类使用隐式解包可选值属性。

8. 解决闭包的循环强引用：

   - 只要在闭包内使用 `self` 的成员，就要用 `self.someProperty` 或者 `self.someMethod()`（而不只是 `someProperty` 或 `someMethod()`）。这提醒你可能会一不小心就捕获了 `self`。
   - 在闭包和捕获的实例总是互相引用并且总是同时销毁时，将闭包内的捕获定义为 `无主引用`。
   - 在被捕获的引用可能会变为 `nil` 时，将闭包内的捕获定义为 `弱引用`。弱引用总是可选类型，并且当引用的实例被销毁后，弱引用的值会自动置为 `nil`。这使我们可以在闭包体内检查它们是否存在。
   - 如果被捕获的引用绝对不会变为 `nil`，应该用无主引用，而不是弱引用。



### 内存安全

1. In-out参数的访问冲突：
   - 一个函数会对它所有的 in-out 参数进行长期写访问。in-out 参数的写访问会在所有非 in-out 参数处理完之后开始，直到函数执行完毕为止。如果有多个 in-out 参数，则写访问开始的顺序与参数的顺序一致
   - 不能再访问以 in-out 形式传入后的原变量，即使作用域原则和访问权限允许——任何访问原变量的行为都会造成冲突因为这样会对于同一变量存储地址进行读访问和写访问，冲突
   - 往同一个函数的多个 in-out 参数里传入同一个变量也会产生冲突
2. 关于结构体属性的重叠访问安全：
   - 你访问的是实例的存储属性，而不是计算属性或类的属性
   - 结构体是本地变量的值，而非全局变量
   - 结构体要么没有被闭包捕获，要么只被非逃逸闭包捕获了



### 访问控制

1. 访问级别：

   - *open* 和 *public* 级别可以让实体被同一模块源文件中的所有实体访问，在模块外也可以通过导入该模块来访问源文件里的所有实体。通常情况下，你会使用 open 或 public 级别来指定框架的外部接口。open 只能作用于类和类的成员，它和 public 的区别主要在于 open 限定的类和成员能够在模块外能被继承和重写
   - *internal* 级别让实体被同一模块源文件中的任何实体访问，但是不能被模块外的实体访问。通常情况下，如果某个接口只在应用程序或框架内部使用，就可以将其设置为 internal 级别。
   - *fileprivate* 限制实体只能在其定义的文件内部访问。如果功能的部分实现细节只需要在文件内使用时，可以使用 fileprivate 来将其隐藏。
   - *private* 限制实体只能在其定义的作用域，以及同一文件内的 extension 访问。如果功能的部分细节只需要在当前作用域内使用时，可以使用 private 来将其隐藏。

2. 访问级别基本原则：实体不能定义在具有更低访问级别（更严格）的实体中

3. 默认访问级别：internal

4. 一个类型的访问级别也会影响到类型*成员*（属性、方法、构造器、下标）的默认访问级别。如果你将类型指定为 `private` 或者 `fileprivate` 级别，那么该类型的所有成员的默认访问级别也会变成 `private` 或者 `fileprivate` 级别。如果你将类型指定为 `internal` 或 `public`（或者不明确指定访问级别，而使用默认的 `internal` ），那么该类型的所有成员的默认访问级别将是 `internal`。

   一个 `public` 类型的所有成员的访问级别默认为 `internal` 级别，而不是 `public` 级别。如果你想将某个成员指定为 `public` 级别，那么你必须显式指定。

5. 元组的访问级别将由元组中访问级别最严格的类型来决定。

6. 函数的访问级别根据访问级别最严格的参数类型或返回类型的访问级别来决定。但是，如果这种访问级别不符合函数定义所在环境的默认访问级别，那么就需要明确地指定该函数的访问级别。

7. 枚举成员的访问级别和该枚举类型相同，你不能为枚举成员单独指定不同的访问级别。枚举定义中的任何原始值或关联值的类型的访问级别至少不能低于枚举类型的访问级别。

8. 嵌套类型的访问级别和包含它的类型的访问级别相同，嵌套类型是 public 的情况除外。在一个 public 的类型中定义嵌套类型，那么嵌套类型自动拥有 `internal` 的访问级别。如果想让嵌套类型拥有 `public` 访问级别，那么必须显式指定该嵌套类型的访问级别为 public。

9. 关于子类

   - 可以继承同一模块中的所有有访问权限的类，也可以继承不同模块中被 open 修饰的类。一个子类的访问级别不得高于父类的访问级别。

   - 在同一模块中，可以在符合当前访问级别的条件下重写任意类成员（方法、属性、构造器、下标等）。在不同模块中，可以重写类中被 open 修饰的成员。

   - 可以通过重写给所继承类的成员提供更高的访问级别。

   - 可以在子类中，用子类成员去访问访问级别更低的父类成员，只要这一操作在相应访问级别的限制范围内

     ```swift
     public class A {
         fileprivate func someMethod() {}
     }
     
     internal class B: A {
         override internal func someMethod() {
             super.someMethod()
         }
     }
     ```

10. 常量、变量、属性不能拥有比它们的类型更高的访问级别。

11. getter和setter：

    - 常量、变量、属性、下标的 `Getters` 和 `Setters` 的访问级别和它们所属类型的访问级别相同

    - `Setter` 的访问级别可以低于对应的 `Getter` 的访问级别，这样就可以控制变量、属性或下标的读写权限

    - 可以为 `Getter` 和 `Setter` 显式指定访问级别。

      ```swift
      public struct TrackedString {
          public private(set) var numberOfEdits = 0
          public var value: String = "" {
              didSet {
                  numberOfEdits += 1
              }
          }
          public init() {}
      }
      ```

12. 自定义构造器的访问级别可以低于或等于其所属类型的访问级别。唯一的例外是 [必要构造器]()，它的访问级别必须和所属类型的访问级别相同。构造器参数的访问级别也不能低于构造器本身的访问级别。

13. 默认构造器的访问级别与所属类型的访问级别相同，除非类型的访问级别是 `public`。如果一个类型被指定为 `public` 级别，那么默认构造器的访问级别将为 `internal`。如果你希望一个 `public` 级别的类型也能在其他模块中使用这种无参数的默认构造器，你只能自己提供一个 `public` 访问级别的无参数构造器。

14. 如果结构体中任意存储型属性的访问级别为 `private`，那么该结构体默认的成员逐一构造器的访问级别就是 `private`。否则，这种构造器的访问级别依然是 `internal`。

15. 关于协议：

    - 协议中的每个方法或属性都必须具有和该协议相同的访问级别。你不能将协议中的方法或属性设置为其他访问级别。这样才能确保该协议的所有方法或属性对于任意遵循者都可用。
    - 如果你定义了一个 `public` 访问级别的协议，那么该协议的所有实现也会是 `public` 访问级别。
    - 如果定义了一个继承自其他协议的新协议，那么新协议拥有的访问级别最高也只能和被继承协议的访问级别相同。
    - 一个类型可以遵循比它级别更低的协议。遵循协议时的上下文级别是类型和协议中级别最小的那个。

16. Extension:

    - Extension 的新增成员具有和原始类型成员一致的访问级别。
    - 可以通过修饰语重新指定 extension 的默认访问级别（例如，`private`），从而给该 extension 中的所有成员指定一个新的默认访问级别。这个新的默认访问级别仍然可以被单独成员指定的访问级别所覆盖。
    - 使用 extension 来遵循协议的话，就不能显式地声明 extension 的访问级别。extension 每个 protocol 要求的实现都默认使用 protocol 的访问级别。
    - 扩展同一文件内的类，结构体或者枚举，extension 里的代码会表现得跟声明在原类型里的一模一样。
      - 在类型的声明里声明一个私有成员，在同一文件的 extension 里访问。
      - 在 extension 里声明一个私有成员，在同一文件的另一个 extension 里访问。
      - 在 extension 里声明一个私有成员，在同一文件的类型声明里访问。

17. 泛型类型或泛型函数的访问级别取决于泛型类型或泛型函数本身的访问级别，还需结合类型参数的类型约束的访问级别，根据这些访问级别中的最低访问级别来确定。

18. 定义的任何类型别名都会被当作不同的类型，以便于进行访问控制。类型别名的访问级别不可高于其表示的类型的访问级别。也适用于为满足协议遵循而将类型别名用于关联类型的情况。





### 高级运算符

1. 溢出运算符：在数值溢出的时候采取截断处理，而非报错。

   - 溢出加法 `&+`
   - 溢出减法 `&-`
   - 溢出乘法 `&*`

2. 运算符函数（运算符重载）：类和结构体可以为现有的运算符提供自定义的实现。

   ```swift
   struct Vector2D {
       var x = 0.0, y = 0.0
   }
   
   extension Vector2D {
       static func + (left: Vector2D, right: Vector2D) -> Vector2D {
           return Vector2D(x: left.x + right.x, y: left.y + right.y)
       }
   }
   ```

3. 前缀和后缀运算符：实现前缀或者后缀运算符，需要在声明运算符函数的时候在 `func` 关键字之前指定 `prefix` 或者 `postfix` 修饰符。

   ```swift
   extension Vector2D {
       static prefix func - (vector: Vector2D) -> Vector2D {
           return Vector2D(x: -vector.x, y: -vector.y)
       }
   }
   ```

4. 复合赋值运算符：在实现的时候，需要把运算符的左参数设置成 `inout` 类型，因为这个参数的值会在运算符函数内直接被修改。

   ```swift
   extension Vector2D {
       static func += (left: inout Vector2D, right: Vector2D) {
           left = left + right
       }
   }
   ```

5. 等价运算符：增加对标准库 `Equatable` 协议的遵循.如果你已经实现了“相等”运算符，通常情况下你并不需要自己再去实现“不等”运算符（`!=`）。标准库对于“不等”运算符提供了默认的实现，它简单地将“相等”运算符的结果进行取反后返回。

   ```swift
   extension Vector2D: Equatable {
       static func == (left: Vector2D, right: Vector2D) -> Bool {
           return (left.x == right.x) && (left.y == right.y)
       }
   }
   ```

6. 自定义运算符：新的运算符要使用 `operator` 关键字在全局作用域内进行定义，同时还要指定 `prefix`、`infix` 或者 `postfix` 修饰符

   ```swift
   prefix operator +++
   extension Vector2D {
       static prefix func +++ (vector: inout Vector2D) -> Vector2D {
           vector += vector
           return vector
       }
   }
   ```

7. 自定义中缀运算符优先级：

   - 每个自定义中缀运算符都属于某个优先级组。优先级组指定了这个运算符相对于其他中缀运算符的优先级和结合性。
   - 没有明确放入某个优先级组的自定义中缀运算符将会被放到一个默认的优先级组内，其优先级高于三元运算符。