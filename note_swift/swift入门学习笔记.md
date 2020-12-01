# swift入门学习笔记-1

> 学习教程：https://swiftgg.gitbook.io/swift/swift-jiao-cheng

本文仅记录个人学习过程中对比已经学过语言的不同以及遗漏处。

### 变量和常量

1. let是常量，var是变量

2. 字符串插值：`\()`

4. 不同类型的字面量可以互相运算，因为字面量没有类型；但是不同类型的常量和变量就需要先类型转换为相同的再继续进行

5. 类型别名：`typealias`



### 元组

1. 元组中可以为多个不同类型的

2. 可用`_` 来忽略不想要的部分

3. 通过`.0` `.1` ，通过下标来访问单个元素

4. 可以在元组中给其中单个变量起名

   `let http200Status = (statusCode: 200, description: "OK")`



### 可选类型

1. `Int?` `String?`

2. 没有值时类型为`nil` ，如果没有直接说是什么，就是`nil`

3. 是一个确定的值，用来表示值缺失

4. 强制解析：当你确定可选类型确实包含值之后，你可以在可选的名字后面加一个感叹号（`!`）来获取值。这个惊叹号表示“我知道这个可选有值，请使用它。”这被称为可选值的*强制解析（forced unwrapping）*

5. 可选绑定：在 `if` 条件语句中使用常量和变量来创建一个可选绑定，仅在 `if` 语句的句中（`body`）中才能获取到值。相反，在 `guard` 语句中使用常量和变量来创建一个可选绑定，仅在 `guard` 语句外且在语句后才能获取到值

6. 隐式解析可选类型：一个隐式解析可选类型其实就是一个普通的可选类型，但是可以被当做非可选类型来使用，并不需要每次都使用解析来获取可选值

   ```swift
   let possibleString: String? = "An optional string."
   let forcedString: String = possibleString! // 需要感叹号来获取值
   
   let assumedString: String! = "An implicitly unwrapped optional string."
   let implicitString: String = assumedString  // 不需要感叹号
   ```

7. 如果一个变量之后可能变成 `nil` 的话请不要使用隐式解析可选类型。如果你需要在变量的生命周期中判断是否是 `nil` 的话，请使用普通可选类型。



### 断言和先决条件

1. 断言和先决条件是在运行时所做的检查。你可以用他们来检查在执行后续代码之前是否一个必要的条件已经被满足了。如果断言或者先决条件中的布尔条件评估的结果为 true（真），则代码像往常一样继续执行。如果布尔条件评估结果为 false（假），程序的当前状态是无效的，则代码执行结束，应用程序中止

2. 可以调用 Swift 标准库的 `assert(_:_:file:line:)` 函数来写一个断言。向这个函数传入一个结果为 `true` 或者 `false` 的表达式以及一条信息，当表达式的结果为 `false` 的时候这条信息会被显示.只有 `age >= 0` 为 `true` 时，即 `age` 的值非负的时候，代码才会继续执行。如果 `age` 的值是负数，就像代码中那样，`age >= 0` 为 `false`，断言被触发，终止应用

   ```swift
   let age = -3
   assert(age >= 0, "A person's age cannot be less than zero")
   // 因为 age < 0，所以断言会触发
   ```

3. 可以使用 `assertionFailure(_:file:line:)` 函数来表明断言失败了

4. 可以使用全局 `precondition(_:_:file:line:)` 函数来写一个先决条件。向这个函数传入一个结果为 `true` 或者 `false` 的表达式以及一条信息，当表达式的结果为 `false` 的时候这条信息会被显示

   ```swift
   // 在一个下标的实现里...
   precondition(index > 0, "Index must be greater than zero.")
   ```

5. 可以调用 `preconditionFailure(_:file:line:)` 方法来表明出现了一个错误，例如，switch 进入了 default 分支，但是所有的有效值应该被任意一个其他分支（非 default 分支）处理。



### 运算符

1. 可以利用元组赋值

   ```swift
   let (x, y) = (1, 2)
   // 现在 x 等于 1，y 等于 2
   ```

2. 赋值等于号不返回任何值，通过将 if x = y 标记为无效语句，Swift 能帮你避免把 （==）错写成（=）这类错误的出现

3. 对负数 `b` 求余时，`b` 的符号会被忽略。这意味着 `a % b` 和 `a % -b` 的结果是相同的。同时，-9%4=-1

4. 元组可以用来比较：如果两个元组的元素相同，且长度相同的话，元组就可以被比较。比较元组大小会按照从左到右、逐值比较的方式，直到发现有两个值不等时停止。如果所有的值都相等，那么这一对元组我们就称它们是相等的

5. bool不能被比较

6. Swift 标准库只能比较七个以内元素的元组比较函数

7. 空合运算符：`a ?? b` : *空合运算符*（`a ?? b`）将对可选类型 `a` 进行空判断，如果 `a` 包含一个值就进行解包，否则就返回一个默认值 `b`。表达式 `a` 必须是 Optional 类型。默认值 `b` 的类型必须要和 `a` 存储值的类型保持一致。

8. 闭区间运算符：`a...b`:

   ```swift
   for index in 1...5{
   		print("\(index)*5=\(index*5)")
   }
   ```

9. 半开区间：半开区间的实用性在于当你使用一个从 0 开始的列表（如数组）时，非常方便地从0数到列表的长度。

   ```swift
   let names=["Anna", "ALex", "TOM"]
   let count=names.count
   for i in 0..<count{
   		print("第\(i+1)个人叫\(names[i])")
   }
   ```

10. 单侧区间：`...a` , `a...` , `..<a` 

    - 可以在数组下标中使用，例如：`names[...2]` , `names[2...]` , `names[..<2]` .

    - 可以查看单侧区间是否包含某个特定的值：

      ```swift
      let range = ...5
      range.contains(7)   // false
      range.contains(4)   // true
      range.contains(-1)  // true
      ```

11. 逻辑与和逻辑或都是短路计算



### 字符串和字符

1. 多行字符串字面量：一个多行字符串字面量包含了所有的在开启和关闭引号（`"""`）中的行。这个字符从开启引号（`"""`）之后的第一行开始，到关闭引号（`"""`）之前为止。这就意味着字符串开启引号之后（`"""`）或者结束引号（`"""`）之前都没有换行符号。

   ```swift
   let singleLineString = "These are the same."
   let multilineString = """
   These are the same.
   """
   ```

   这两个是一样的！

2. 关闭引号（`"""`）之前的空白字符串告诉 Swift 编译器其他各行多少空白字符串需要忽略。然而，如果你在某行的前面写的空白字符串超出了关闭引号（`"""`）之前的空白字符串，则超出部分将被包含在多行字符串字面量中。

3. 如果在字面量中有换行，那么最终显示的字符串就有换行。有空行最终显示就有空行。如果想增加可读性，可以利用`\` 进行续行。反斜杠作为续行符。

4. 单行字符串中使用双引号需要转义：`\"`, 而多行字符串字面量中国呢可以直接使用`"`,使用`"""` 时需要使用至少一个转义符

5. 将字符串文字放在扩展分隔符`#`中，这样字符串中的特殊字符将会被直接包含而非转义后的效果。扩展分隔符可以用在单行字符串中，也可以用在多行字符串中。

6. 如果需要字符串文字中字符的特殊效果，请匹配转义字符（`\`）后面添加与起始位置个数相匹配的 `#` 符。 例如，如果字符串是 `#"Line 1 \nLine 2"#` 并且您想要换行，则可以使用 `#"Line 1 \#nLine 2"#` 来代替。 同样，`###"Line1 \###nLine2"###` 也可以实现换行效果。

7. String 是值类型，值拷贝

8. 字符串可以通过一个值类型为`Character` 的数组作为自变量来初始化

   ```swift
   let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
   let catString = String(catCharacters)
   print(catString)
   // 打印输出：“Cat!🐱”
   ```

9. `String` 和 `Character` 类型是完全兼容 Unicode 标准的。Unicode标量：`\u{n}` 

10. 可以在字符串插值`\() ` 进行计算

11. 想要获得一个字符串中 `Character` 值的数量，可以使用 `count` 属性（不是方法，不用加`()`）

12. 通过 `count` 属性返回的字符数量并不总是与包含相同字符的 `NSString` 的 `length` 属性相同。`NSString` 的 `length` 属性是利用 UTF-16 表示的十六位代码单元数字，而不是 Unicode 可扩展的字符群集。不同的字符以及相同字符的不同表示方式可能需要不同数量的内存空间来存储。所以 Swift 中的字符在一个字符串中并不一定占用相同的内存空间数量。因此在没有获取字符串的可扩展的字符群的范围时候，就不能计算出字符串的字符数量。

13. 字符串索引来取character要注意：不能用整数做索引。

    属性：`startIndex` , `endIndex` 

    方法：`index(before:)` , `index(after:)`, `index(_:offsetBy:)`

    ```swift
    let greeting = "Guten Tag!"
    greeting[greeting.startIndex]
    // G
    greeting[greeting.index(before: greeting.endIndex)]
    // !
    greeting[greeting.index(after: greeting.startIndex)]
    // u
    let index = greeting.index(greeting.startIndex, offsetBy: 7)
    greeting[index]
    // a
    ```

14. 使用`indices` 属性创建一个包含全部索引的范围,来访问单个字符

15. 插入和删除：

    - 调用 `insert(_:at:)` 方法可以在一个字符串的指定索引插入一个字符，调用 `insert(contentsOf:at:)` 方法可以在一个字符串的指定索引插入一个段字符串。

    - 调用 `remove(at:)` 方法可以在一个字符串的指定索引删除一个字符，调用 `removeSubrange(_:)` 方法可以在一个字符串的指定索引删除一个子字符串。

      ```swift
      let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
      welcome.removeSubrange(range)
      // welcome 现在等于 "hello"
      ```

16. 子字符串：`String` 和 `Substring` 的区别在于性能优化上，`Substring` 可以重用原 `String` 的内存空间，或者另一个 `Substring` 的内存空间（`String` 也有同样的优化，但如果两个 `String` 共享内存的话，它们就会相等）。这一优化意味着你在修改 `String` 和 `Substring` 之前都不需要消耗性能去复制内存。就像前面说的那样，`Substring` 不适合长期存储 —— 因为它重用了原 `String` 的内存空间，原 `String` 的内存空间必须保留直到它的 `Substring` 不再被使用为止。

    ```swift
    let greeting = "Hello, world!"
    let index = greeting.firstIndex(of: ",") ?? greeting.endIndex
    let beginning = greeting[..<index]
    // beginning 的值为 "Hello"
    
    // 把结果转化为 String 以便长期存储。
    let newString = String(beginning)
    ```

17. 前缀和后缀：通过调用字符串的 `hasPrefix(_:)`/`hasSuffix(_:)` 方法来检查字符串是否拥有特定前缀/后缀，两个方法均接收一个 `String` 类型的参数，并返回一个布尔值。

18. 字符串可以用属性`utf8` , `utf16` , `unicodeScalars` 来访问它的utf-8表示、utf-16表示、unicode标量表示。

    - 遍历 `String` 值的 `unicodeScalars` 属性来访问它的 Unicode 标量表示。其为 `UnicodeScalarView` 类型的属性，`UnicodeScalarView` 是 `UnicodeScalar` 类型的值的集合。

    - 每一个 `UnicodeScalar` 拥有一个 `value` 属性，可以返回对应的 21 位数值

      ```swift
      for scalar in dogString.unicodeScalars {
          print("\(scalar.value) ", terminator: "")
      }
      // 68 111 103 8252 128054
      
      // unicodeScalar直接返回原本的字符
      for scalar in dogString.unicodeScalars {
          print("\(scalar) ")
      }
      // D
      // o
      // g
      // ‼
      // 🐶
      ```



### 集合类型

三种基本的集合类型：Array, Set, Dictionary

#### Array

1. 完整写法为 `Array<Element>` 或者`[Element]`

   `var myArray = [Int]()`

2. 创建带有默认值数组：`var three = Array(repeating: 0.0, count: 3)`

3. 可用`+`, `+=`组合数组

4. 属性：`count` ,`isEmpty` 

5. 可以利用下标改变一系列：`shoppingList[4...6] = ["Bananas", "Apples"]`

6. 利用`enumerated()`方法来遍历获取index和value

   ```swift
   for (index, value) in shoppingList.enumerated() {
       print("Item \(String(index + 1)): \(value)")
   }
   ```



#### Set

1.  `Set<Element>`

2. 可以用数组字面量来创建集合，如果上下文提供了类型，可以通过空的数组字面量创建一个空集合：`[]`

3. 一个集合类型不能从数组字面量中被直接推断出来，因此 `Set` 类型必须显式声明。然而，由于 Swift 的类型推断功能，如果你想使用一个数组字面量构造一个集合并且与该数组字面量中的所有元素类型相同，那么无须写出集合的具体类型。

   `var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]`

4. 可以通过调用集合的 `remove(_:)` 方法去删除一个元素，如果它是该集合的一个元素则删除它并且返回它的值，若该集合不包含它，则返回 `nil`。另外，集合可以通过 `removeAll()` 方法删除所有元素。

5. 可用for来进行遍历，但是无序。Swift 的 `Set` 类型没有确定的顺序，为了按照特定顺序来遍历一个集合中的值可以使用 `sorted()` 方法，它将返回一个有序数组，这个数组的元素排列顺序由操作符 `<` 对元素进行比较的结果来确定。

6. 集合基本操作：

   - 使用 `intersection(_:)` 方法根据两个集合的交集创建一个新的集合。
   - 使用 `symmetricDifference(_:)` 方法根据两个集合不相交的值创建一个新的集合。
   - 使用 `union(_:)` 方法根据两个集合的所有值创建一个新的集合。
   - 使用 `subtracting(_:)` 方法根据不在另一个集合中的值创建一个新的集合。

7. 集合成员关系和相等：

   - 使用“是否相等”运算符（`==`）来判断两个集合包含的值是否全部相同。
   - 使用 `isSubset(of:)` 方法来判断一个集合中的所有值是否也被包含在另外一个集合中。
   - 使用 `isSuperset(of:)` 方法来判断一个集合是否包含另一个集合中所有的值。
   - 使用 `isStrictSubset(of:)` 或者 `isStrictSuperset(of:)` 方法来判断一个集合是否是另外一个集合的子集合或者父集合并且两个集合并不相等。
   - 使用 `isDisjoint(with:)` 方法来判断两个集合是否不含有相同的值（是否没有交集）。



#### Dictionary

1. `Dictionary<Key, Value>` 定义，简化： `[Key: Value]`

2. `[:]`如果上下文已经提供了类型信息，你可以使用空字典字面量来创建一个空字典，记作 `[:]` 

3. 用字典字面量创建字典：`var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]`

4. 可以用下标来添加或者改变特定键对应值，或者用 `updateValue(_:forKey:)`(不存在时创建，并且返回旧值或nil,**返回值类型的可选类型**)

   ```swift
   if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
       print("The old value for DUB was \(oldValue).")
   }
   ```

5. 或者使用下标来检索，返回值或者nil;使用下标语法通过将某个键的对应值赋值为 `nil` 来从字典里移除一个键值对

   ```swift
   if let airportName = airports["DUB"] {
       print("The name of the airport is \(airportName).")
   } else {
       print("That airport is not in the airports dictionary.")
   }
   ```

6. 移除：`removeValue(forKey:)` 方法也可以用来在字典中移除键值对。这个方法在键值对存在的情况下会移除该键值对并且返回被移除的值或者在没有对应值的情况下返回 `nil`

   ```swift
   if let removedValue = airports.removeValue(forKey: "DUB") {
       print("The removed airport's name is \(removedValue).")
   } else {
       print("The airports dictionary does not contain a value for DUB.")
   }
   ```

7. 遍历时：可以直接`(keyName,valueNanme)` 直接遍历，或者利用属性`keys`, `values` 来直接遍历键或者值



### 控制流

#### for循环

1. `for...in` 循环中：如果不需要区间序列内每一项的值，可以使用下划线（`_`）替代变量名来忽略这个值

   ```swift
   for _ in 1...power{
   		answer *= base;
   }
   ```

2. 为了跳过不需要的内容，使用 `stride(from:to:by:)` 函数（开区间）跳过不需要的标记；在闭区间使用 `stride(from:through:by:)` 起到同样作用



#### while循环

1. `while` 和`repeat...while`
2. 先执行每次最后再判断循环条件：`repeat...while`



#### switch

1. 不需要在case分支中显式使用`break` ，当匹配的 case 分支中的代码执行完毕后，程序会终止 `switch` 语句，而不会继续执行下一个 case 分支。

2. 每一个 case 分支都*必须*包含至少一条语句。

3. 在case中可以同时匹配多个，符合匹配，用逗号分开

4. 可以在case中写区间例如：`1..<5` 这样进行区间匹配

5. switch中也可以来匹配元组。元组中的元素可以是值，也可以是区间。另外，使用下划线（`_`）来匹配所有可能的值。

6. 值绑定：case 分支允许将匹配的值声明为临时常量或变量，并且在 case 分支体内使用 —— 这种行为被称为*值绑定*（value binding），因为匹配的值在 case 分支体内，与临时的常量或变量绑定。

   ```swift
   let anotherPoint = (2, 0)
   switch anotherPoint {
   case (let x, 0):
       print("on the x-axis with an x value of \(x)")
   case (0, let y):
       print("on the y-axis with a y value of \(y)")
   case let (x, y):
       print("somewhere else at (\(x), \(y))")
   }
   // 输出“on the x-axis with an x value of 2”
   ```

7. 使用 `where` 语句来判断额外的条件:

   ```swift
   let yetAnotherPoint = (1, -1)
   switch yetAnotherPoint {
   case let (x, y) where x == y:
       print("(\(x), \(y)) is on the line x == y")
   case let (x, y) where x == -y:
       print("(\(x), \(y)) is on the line x == -y")
   case let (x, y):
       print("(\(x), \(y)) is just some arbitrary point")
   }
   // 输出“(1, -1) is on the line x == -y”
   ```

8. 复合型cases：

   - 将这几种可能放在同一个 `case` 后面，并且用逗号隔开。当 case 后面的任意一种模式匹配的时候，这条分支就会被匹配。并且，如果匹配列表过长，还可以分行书写。
   - 复合匹配同样可以包含值绑定。复合匹配里所有的匹配模式，都必须包含相同的值绑定。并且每一个绑定都必须获取到相同类型的值。这保证了，无论复合匹配中的哪个模式发生了匹配，分支体内的代码，都能获取到绑定的值，并且绑定的值都有一样的类型。



#### 控制转移语句

​	`continue` , `break` , `fallthrough` , `return` , `throw` 

1. 需要 C 风格的贯穿的特性，你可以在每个需要该特性的 case 分支中使用 `fallthrough` 关键字。

2. 可以使用标签（*statement label*）来标记一个循环体或者条件语句，对于一个条件语句，你可以使用 `break` 加标签的方式，来结束这个被标记的语句。对于一个循环语句，你可以使用 `break` 或者 `continue` 加标签，来结束或者继续这条被标记语句的执行。

   

#### 提前退出guard

- `guard` 的执行取决于一个表达式的布尔值。我们可以使用 `guard` 语句来要求条件必须为真时，以执行 `guard` 语句后的代码。不同于 `if` 语句，一个 `guard` 语句总是有一个 `else` 从句，如果条件不为真则执行 `else` 从句中的代码。
- 相比于可以实现同样功能的 `if` 语句，按需使用 `guard` 语句会提升我们代码的可读性。它可以使你的代码连贯的被执行而不需要将它包在 `else` 块中，它可以使你在紧邻条件判断的地方，处理违规的情况。



#### 检测API可用性

```swift
if #available(平台名称 版本号, ..., *) {
    APIs 可用，语句将执行
} else {
    APIs 不可用，语句将不执行
}
```



### 函数

1. 函数可无返回类型，不写`->`，相当于返回一个`void` 类型特殊值，给只为一个空元组，写成`()`

2. 返回类型可以是一个元祖，元组的成员不需要在元组从函数中返回时命名，因为它们的名字已经在函数返回类型中指定了。

3. 隐式返回的函数：如果一个函数的整个函数体是一个单行表达式，这个函数可以隐式地返回这个表达式。任何一个可以被写成一行 `return` 语句的函数都可以忽略 `return`。

4. 每个函数参数都有一个*参数标签（argument label）*以及一个*参数名称（parameter name）*。参数标签在调用函数的时候使用；调用的时候需要将函数的参数标签写在对应的参数前面。参数名称在函数的实现中使用。默认情况下，函数参数使用参数名称来作为它们的参数标签。

5. 忽略参数标签：不希望为某个参数添加一个标签，可以使用一个下划线（`_`）来代替一个明确的参数标签。调用时直接写实参即可，不用再写参数标签，此时参数名称不是参数标签，参数标签被忽略了，不用写。

6. 默认参数值：直接给参数赋值即可。将不带有默认值的参数放在函数参数列表的最前。一般来说，没有默认值的参数更加的重要，将不带默认值的参数放在最前保证在函数调用时，非默认参数的顺序是一致的，同时也使得相同的函数在不同情况下调用时显得更为清晰。

7. 可变参数：在变量类型名后面加入（`...`）的方式来定义可变参数。可变参数的传入值在函数体中变为此类型的一个数组。一个函数最多只能拥有一个可变参数

8. 输入输出参数

   - 函数参数默认是常量。试图在函数体中更改参数值将会导致编译错误。

   - 定义一个输入输出参数时，在参数定义前加 `inout` 关键字。一个 `输入输出参数`有传入函数的值，这个值被函数修改，然后被传出函数，替换原来的值。

   - 函数调用传参时，当传入的参数作为输入输出参数时，需要在参数名前加 `&` 符，表示这个值可以被函数修改。

     ```swift
     func swapTwoInts(_ a: inout Int, _ b: inout Int) {
         let temporaryA = a
         a = b
         b = temporaryA
     }
     
     var someInt = 3
     var anotherInt = 107
     swapTwoInts(&someInt, &anotherInt)
     ```

9. 函数类型：

   - 由参数类型和返回类型组成。如`(Int,Int）->Int`  , `()->Void`类型等等

   - 可以定义一个类型为函数的常量或变量，并将适当的函数赋值给它。

     `var mathFunction: (Int, Int) -> Int = addTwoInts`

   - 有相同匹配类型的不同函数可以被赋值给同一个变量

   - 赋值一个函数给常量或变量时，可以让 Swift 来推断其函数类型而不用自己写明此函数变量或者常量的函数类型

   - 函数类型作为参数类型：用 `(Int, Int) -> Int` 这样的函数类型作为另一个函数的参数类型。这样你可以将函数的一部分实现留给函数的调用者来提供。

     ```swift
     func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
         print("Result: \(mathFunction(a, b))")
     }
     printMathResult(addTwoInts, 3, 5)
     // 打印“Result: 8”
     ```

   - 函数类型作为返回类型

10. 嵌套函数：嵌套函数是对外界不可见的，但是可以被它们的外围函数（enclosing function）调用。一个外围函数也可以返回它的某一个嵌套函数，使得这个函数可以在其他域中被使用。



### 闭包

1. 全局和嵌套函数也是特殊的闭包：

   - 全局函数是一个有名字但不会捕获任何值的闭包
   - 嵌套函数是一个有名字并可以捕获其封闭函数域内值的闭包
   - 闭包表达式是一个利用轻量级语法所写的可以捕获其上下文中变量或常量值的匿名闭包

2. 闭包表达式语法：

   ```swift
   { (parameters) -> return type in
       statements
   }
   ```

   Eg:

   ```swift
   reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
       return s1 > s2
   })
   ```

3. 根据上下文推断类型：通常通过内联闭包表达式构造的闭包作为参数传递给函数或者方法时，总是能够推断出闭包的参数和返回值类型。这意味着闭包作为函数或者方法的参数时，几乎不需要利用完整格式构造内联闭包。

   ```swift
   reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
   ```

4. 单表达式闭包的隐式返回：单行表达式闭包可以通过省略 `return` 关键字来隐式返回单行表达式的结果。

   ```swift
   reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
   ```

5. 参数名称缩写：直接通过 `$0`，`$1`，`$2` 来顺序调用闭包的参数，以此类推。

   ```swift
   reversedNames = names.sorted(by: { $0 > $1 } )
   ```

6. 运算符方法：

   ```swift
   reversedNames = names.sorted(by: >)
   ```

7. 尾随闭包：

   - 尾随闭包是一个书写在函数圆括号之后的闭包表达式，函数支持将其作为最后一个参数调用。在使用尾随闭包时，你不用写出它的参数标签

     ```swift
     func someFunctionThatTakesAClosure(closure: () -> Void) {
         // 函数体部分
     }
     
     // 以下是不使用尾随闭包进行函数调用
     someFunctionThatTakesAClosure(closure: {
         // 闭包主体部分
     })
     
     // 以下是使用尾随闭包进行函数调用
     someFunctionThatTakesAClosure() {
         // 闭包主体部分
     }
     ```

     Eg:

     ```swift
     reversedNames = names.sorted() { $0 > $1 }
     
     // 如果闭包表达式是函数或方法的唯一参数，则当使用尾随闭包时，甚至可以把 () 省略掉：
     reversedNames = names.sorted { $0 > $1 }
     ```

8. 值捕获：

   - 闭包可以在其被定义的上下文中*捕获*常量或变量。即使定义这些常量和变量的原作用域已经不存在，闭包仍然可以在闭包函数体内引用和修改这些值。

     ```swift
     func makeIncrementer(forIncrement amount: Int) -> () -> Int {
         var runningTotal = 0
         func incrementer() -> Int {
             runningTotal += amount
             return runningTotal
         }
         return incrementer
     }
     
     //接下来使用这个函数的例子：
     let incrementByTen = makeIncrementer(forIncrement: 10)
     incrementByTen()
     // 返回的值为10
     incrementByTen()
     // 返回的值为20
     incrementByTen()
     // 返回的值为30
     
     //创建另一个新的独立
     let incrementBySeven = makeIncrementer(forIncrement: 7)
     incrementBySeven()
     // 返回的值为7
     
     //再调用原来的，保持！
     incrementByTen()
     // 返回的值为40
     ```

   - 如果你将闭包赋值给一个类实例的属性，并且该闭包通过访问该实例或其成员而捕获了该实例，你将在闭包和该实例间创建一个循环强引用。Swift 使用捕获列表来打破这种循环强引用。

     - [ ] **学习这个循环强引用问题**

9. 闭包是引用类型

   - 函数和闭包都是*引用类型*

   - 无论你将函数或闭包赋值给一个常量还是变量，实际上都是将常量或变量的值设置为**对应函数或闭包的引用**。

   - 如果将闭包赋值给了两个不同的常量或变量，两个值都会指向同一个闭包

     ```swift
     let alsoIncrementByTen = incrementByTen
     alsoIncrementByTen()
     // 返回的值为50
     ```

10. 逃逸闭包

    - 当一个闭包作为参数传到一个函数中，但是这个闭包在函数返回之后才被执行，我们称该闭包从函数中*逃逸*。当你定义接受闭包作为参数的函数时，你可以在参数名之前标注 `@escaping`，用来指明这个闭包是允许“逃逸”出这个函数的。

    - 能使闭包“逃逸”出函数的方法是，将这个闭包保存在一个函数外部定义的变量中能使闭包“逃逸”出函数的方法是，将这个闭包保存在一个函数外部定义的变量中

      ```swift
      var completionHandlers: [() -> Void] = []
      func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
          completionHandlers.append(completionHandler)
      }
      ```

    - 将一个闭包标记为 `@escaping` 意味着你必须在闭包中显式地引用 `self`。

      ```swift
      func someFunctionWithNonescapingClosure(closure: () -> Void) {
          closure()
      }
      
      class SomeClass {
          var x = 10
          func doSomething() {
              someFunctionWithEscapingClosure { self.x = 100 }
              someFunctionWithNonescapingClosure { x = 200 }
          }
      }
      
      let instance = SomeClass()
      instance.doSomething()
      print(instance.x)
      // 打印出“200”
      
      completionHandlers.first?()
      print(instance.x)
      // 打印出“100”
      ```

11. 自动闭包

    - *自动闭包*是一种自动创建的闭包，用于包装传递给函数作为参数的表达式。这种闭包不接受任何参数，当它被调用的时候，会返回被包装在其中的表达式的值。这种便利语法让你能够省略闭包的花括号，用一个普通的表达式来代替显式的闭包。

    - 自动闭包让你能够延迟求值，因为直到你调用这个闭包，代码段才会被执行。

    - 传递显示闭包：

      ```swift
      // customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
      func serve(customer customerProvider: () -> String) {
          print("Now serving \(customerProvider())!")
      }
      serve(customer: { customersInLine.remove(at: 0) } )
      // 打印出“Now serving Alex!”
      ```

      自动闭包：

      ```swift
      // customersInLine is ["Ewa", "Barry", "Daniella"]
      func serve(customer customerProvider: @autoclosure () -> String) {
          print("Now serving \(customerProvider())!")
      }
      serve(customer: customersInLine.remove(at: 0))
      // 打印“Now serving Ewa!”
      ```

12. 逃逸闭包和自动闭包同时使用的eg:

    ```swift
    // customersInLine i= ["Barry", "Daniella"]
    var customerProviders: [() -> String] = []
    func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
        customerProviders.append(customerProvider)
    }
    collectCustomerProviders(customersInLine.remove(at: 0))
    collectCustomerProviders(customersInLine.remove(at: 0))
    
    print("Collected \(customerProviders.count) closures.")
    // 打印“Collected 2 closures.”
    for customerProvider in customerProviders {
        print("Now serving \(customerProvider())!")
    }
    // 打印“Now serving Barry!”
    // 打印“Now serving Daniella!”
    ```



### 枚举

1. 用`enum` 来创建枚举：

   ```swift
   enum CompassPoint {
       case north
       case south
       case east
       case west
   }
   ```

   Swift 的枚举成员在被创建时不会被赋予一个默认的整型值,相反，这些枚举成员本身就是完备的值，这些值的类型是已经明确定义好的 `CompassPoint` 类型。

2. 多个成员可以出现在同一行，用逗号隔开：

   ```swift
   enum Planet {
       case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
   }
   ```

3. 将枚举类型赋值给变量常量：

   ```swift
   var directionToHead = CompassPoint.west
   directionToHead = .east  //之后再次赋值可以省略枚举类型名
   ```

4. 可以用switch匹配枚举值。记得在枚举成员前加`.` 

5. 枚举成员的遍历：令枚举遵循 `CaseIterable` 协议。Swift 会生成一个 `allCases` 属性，用于表示一个包含枚举所有成员的**集合**。

   ```swift
   enum Beverage: CaseIterable {
       case coffee, tea, juice
   }
   let numberOfChoices = Beverage.allCases.count
   print("\(numberOfChoices) beverages available")
   // 打印“3 beverages available”
   ```

6. 关联值：定义 Swift 枚举来存储任意类型的关联值

   ```swift
   enum Barcode {
       case upc(Int, Int, Int, Int)
       case qrCode(String)
   }
   var productBarcode = Barcode.upc(8, 85909, 51226, 3)
   productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
   
   switch productBarcode {
   case .upc(let numberSystem, let manufacturer, let product, let check):
       print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
   case .qrCode(let productCode):
       print("QR code: \(productCode).")
   }
   // 打印“QR code: ABCDEFGHIJKLMNOP.”
   ```

   如果一个枚举成员的所有关联值全都被提取为常量或者全都被提取为变量，可以只在前面加个`let` 或者`var` ，而不用像上面`switch` 中每个都加`let` 

7. 原始值：

   - 作为关联值的替代选择，枚举成员可以被默认值（称为*原始值*）预填充，这些原始值的类型必须相同。每个原始值在枚举声明中必须是唯一的。

   - 对于一个特定的枚举成员，它的**原始值始终不变**。关联值是创建一个基于枚举成员的常量或变量时才设置的值，枚举成员的**关联值可以变化**。

   - 原始值的隐式赋值：

     - 在使用原始值为整数或者字符串类型的枚举时，不需要显式地为每一个枚举成员设置原始值，Swift 将会自动为你赋值。

     - 当使用整数作为原始值时，隐式赋值的值依次递增 `1`。如果第一个枚举成员没有设置原始值，其原始值将为 `0`。

       ```swift
       enum Planet: Int {
           case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
       }
       ```

     - 当使用字符串作为枚举类型的原始值时，每个枚举成员的隐式原始值为该枚举成员的名称。

   - 使用枚举成员的 `rawValue` 属性可以访问该枚举成员的原始值.

   - 使用原始值初始化枚举实例

     - 如果在定义枚举类型的时候使用了原始值，那么将会自动获得一个初始化方法，这个方法接收一个叫做 `rawValue` 的参数，参数类型即为原始值类型，返回值则是枚举成员或 `nil`。

       ```swift
       let possiblePlanet = Planet(rawValue: 7)
       // possiblePlanet 类型为 Planet? 值为 Planet.uranus
       ```

     - 原始值构造器是一个可失败构造器 

8. 递归枚举

   - *递归枚举*是一种枚举类型，它有一个或多个枚举成员使用该枚举类型的实例作为关联值。使用递归枚举时，编译器会插入一个间接层。你可以在枚举成员前加上 `indirect` 来表示该成员可递归。

     ```swift
     enum ArithmeticExpression {
         case number(Int)
         indirect case addition(ArithmeticExpression, ArithmeticExpression)
         indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
     }
     
     //或者
     indirect enum ArithmeticExpression {
         case number(Int)
         case addition(ArithmeticExpression, ArithmeticExpression)
         case multiplication(ArithmeticExpression, ArithmeticExpression)
     }
     
     // 应用
     let five = ArithmeticExpression.number(5)
     let four = ArithmeticExpression.number(4)
     let sum = ArithmeticExpression.addition(five, four)
     let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
     ```

   - 要操作具有递归性质的数据结构，可以使用递归函数

     ```swift
     func evaluate(_ expression: ArithmeticExpression) -> Int {
         switch expression {
         case let .number(value):
             return value
         case let .addition(left, right):
             return evaluate(left) + evaluate(right)
         case let .multiplication(left, right):
             return evaluate(left) * evaluate(right)
         }
     }
     
     print(evaluate(product))
     // 打印“18”
     ```

     

### 类和结构体

1. 不需要为自定义的结构体和类的接口与实现代码分别创建文件。只需在单一的文件中定义一个结构体或者类，系统将会自动生成面向其它代码的外部接口。

2. 定义结构体和类：`struct` , `class` . 每次定义新的结构体和类时，都是新定义了类型，开头大写的驼峰写法；属性方法用小写开头的驼峰

   ```swift
   struct Resolution {
       var width = 0
       var height = 0
   }
   class VideoMode {
       var resolution = Resolution()
       var interlaced = false
       var frameRate = 0.0
       var name: String?
   }
   ```

3. 结构体类型的成员逐一构造器

   ```swift
   let vga = Resolution(width: 640, height: 480)
   ```

4. 结构体和枚举是值类型。当它被赋值给一个变量、常量或者被传递给一个函数的时候，其值会被*拷贝*。Swift 中所有的基本类型：整数（integer）、浮点数（floating-point number）、布尔值（boolean）、字符串（string)、数组（array）和字典（dictionary），都是值类型，其底层也是使用结构体实现的。

5. 类是引用类型：*引用类型*在被赋予到一个变量、常量或者被传递到一个函数时，其值不会被拷贝。因此，使用的是已存在实例的引用，而不是其拷贝。尽管引用了类的实例的常量是常量，但是依然可以改变类实例中的成员变量。这是因为引用类实例的常量的值并未改变，它们并不“存储”这个 实例，而仅仅是对实例的引用。所以，改变的是底层实例的属性，而不是指向常量引用的值。

6. 使用恒等运算符`===` , `!==` 来检测两个常量或者变量是否引用了同一个实例

7. “相同”（用三个等号表示，`===`）与“等于”（用两个等号表示，`==`）的不同。“相同”表示两个类类型（class type）的常量或者变量引用同一个类实例。“等于”表示两个实例的值“相等”或“等价”。





### 属性

1. 存储属性：一个存储属性就是存储在特定类或结构体实例里的一个常量或变量。

2. 由于结构体属于*值类型*。当值类型的实例被声明为常量的时候，它的所有属性也就成了常量。如果创建了一个结构体实例并将其赋值给一个常量，则无法修改该实例的任何属性。**这里注意区分和类的区别，类是引用类型，赋值给常量后，仍然可以修改该实例的可变属性**

3. 延时加载存储属性：*延时加载存储属性*是指当第一次被调用的时候才会计算其初始值的属性。在属性声明前**使用 `lazy` 来标示一个延时加载存储属性**。必须将延时加载属性声明成变量，因为属性的初始值可能在实例构造完成之后才会得到。而常量属性在构造过程完成之前必须要有初始值，因此无法声明成延时加载。如果一个被标记为 `lazy` 的属性在没有初始化时就同时被多个线程访问，则无法保证该属性只会被初始化一次。

4. 计算属性

   - 计算属性不直接存储值，而是提供一个 getter 和一个可选的 setter，来间接获取和设置其他属性或变量的值。

     ```swift
     struct Point {
         var x = 0.0, y = 0.0
     }
     struct Size {
         var width = 0.0, height = 0.0
     }
     struct Rect {
         var origin = Point()
         var size = Size()
         var center: Point {
             get {
                 let centerX = origin.x + (size.width / 2)
                 let centerY = origin.y + (size.height / 2)
                 return Point(x: centerX, y: centerY)
             }
             set(newCenter) {
                 origin.x = newCenter.x - (size.width / 2)
                 origin.y = newCenter.y - (size.height / 2)
             }
         }
     }
     var square = Rect(origin: Point(x: 0.0, y: 0.0),
         size: Size(width: 10.0, height: 10.0))
     let initialSquareCenter = square.center
     square.center = Point(x: 15.0, y: 15.0)
     print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
     // 打印“square.origin is now at (10.0, 10.0)”
     ```

   - 如果计算属性的 setter 没有定义表示新值的参数名，则可以使用默认名称 `newValue`。

   - 如果整个 getter 是单一表达式，getter 会隐式地返回这个表达式结果。

     ```swift
     struct CompactRect {
         var origin = Point()
         var size = Size()
         var center: Point {
             get {
                 Point(x: origin.x + (size.width / 2),
                       y: origin.y + (size.height / 2))
             }
             set {
                 origin.x = newValue.x - (size.width / 2)
                 origin.y = newValue.y - (size.height / 2)
             }
         }
     }
     ```

   - 只读计算属性：

     - 只有 getter 没有 setter 的计算属性叫*只读计算属性*。只读计算属性总是返回一个值，可以通过点运算符访问，但不能设置新的值。
     - 只读计算属性的声明可以去掉 `get` 关键字和花括号
     - **必须使用 `var` 关键字定义计算属性**，包括只读计算属性，因为它们的值不是固定的。

5. 属性观察器

   - 属性观察器监控和响应属性值的变化，每次属性被设置值的时候都会调用属性观察器，即使新值和当前值相同的时候也不例外。

   - 可以在这些位置添加属性监控器：

     - 自定义的存储属性
     - 继承的存储属性
     - 继承的计算属性

   - 两个观察器：

     - `willSet` 在新的值被设置之前调用:会将新的属性值作为常量参数传入，默认名称`newValue`

     - `didSet` 在新的值被设置之后调用:会将旧的属性值作为参数传入,默认名称`oldValue` .

       如果在 `didSet` 方法中再次对该属性赋值，那么新值会覆盖旧的值，同时不会再次触发`didSet`

       ```swift
       class StepCounter {
           var totalSteps: Int = 0 {
               willSet(newTotalSteps) {
                   print("将 totalSteps 的值设置为 \(newTotalSteps)")
               }
               didSet {
                   if totalSteps > oldValue  {
                       print("增加了 \(totalSteps - oldValue) 步")
                   }
               }
           }
       }
       let stepCounter = StepCounter()
       stepCounter.totalSteps = 200
       // 将 totalSteps 的值设置为 200
       // 增加了 200 步
       ```

     - 如果将带有观察器的属性通过 in-out 方式传入函数，`willSet` 和 `didSet` 也会调用。这是因为 in-out 参数采用了拷入拷出内存模式：即在函数内部使用的是参数的 copy，函数结束后，又对参数重新赋值。

6. 属性包装器

   - 属性包装器在管理属性如何存储和定义属性的代码之间添加了一个分隔层。

   - 定义一个属性包装器，需要创建一个定义 `wrappedValue` 属性的结构体、枚举或者类。同时需要注明`@propertyWrapper`

     ```swift
     @propertyWrapper
     struct TwelveOrLess {
         private var number: Int
         init() { self.number = 0 }
         var wrappedValue: Int {
             get { return number }
             set { number = min(newValue, 12) }
         }
     }
     ```

     ```swift
     struct SmallRectangle {
         @TwelveOrLess var height: Int
         @TwelveOrLess var width: Int
     }
     
     var rectangle = SmallRectangle()
     print(rectangle.height)
     // 打印 "0"
     
     rectangle.height = 10
     print(rectangle.height)
     // 打印 "10"
     
     rectangle.height = 24
     print(rectangle.height)
     // 打印 "12"
     ```

   - 设置被包装属性的初始值：

     - 为了支持设定一个初始值或者其他自定义操作，属性包装器需要添加构造器Eg：

       ```swift
       @propertyWrapper
       struct SmallNumber {
           private var maximum: Int
           private var number: Int
       
           var wrappedValue: Int {
               get { return number }
               set { number = min(newValue, maximum) }
           }
       
           init() {
               maximum = 12
               number = 0
           }
           init(wrappedValue: Int) {
               maximum = 12
               number = min(wrappedValue, maximum)
           }
           init(wrappedValue: Int, maximum: Int) {
               self.maximum = maximum
               number = min(wrappedValue, maximum)
           }
       }
       
       //没有设定初始值时，使用init()来设置包装器
       struct ZeroRectangle {
           @SmallNumber var height: Int
           @SmallNumber var width: Int
       }
       var zeroRectangle = ZeroRectangle()
       print(zeroRectangle.height, zeroRectangle.width)
       // 打印 "0 0"
       
       // 设置指定初始值时
       struct UnitRectangle {
           @SmallNumber var height: Int = 1
           @SmallNumber var width: Int = 1
       }
       var unitRectangle = UnitRectangle()
       print(unitRectangle.height, unitRectangle.width)
       // 打印 "1 1"
       
       //符合第三个构造器：
       struct NarrowRectangle {
           @SmallNumber(wrappedValue: 2, maximum: 5) var height: Int
           @SmallNumber(wrappedValue: 3, maximum: 4) var width: Int
       }
       var narrowRectangle = NarrowRectangle()
       print(narrowRectangle.height, narrowRectangle.width)
       // 打印 "2 3"
       narrowRectangle.height = 100
       narrowRectangle.width = 100
       print(narrowRectangle.height, narrowRectangle.width)
       // 打印 "5 4"
       ```

     - 当包含属性包装器实参时，你也可以使用赋值来指定初始值。Swift 将赋值视为 `wrappedValue` 参数，且使用接受被包含的实参的构造器。

   - 从属性包装器呈现一个值

     - 除了被包装值，属性包装器可以通过定义被呈现值暴露出其他功能

     - 除了以货币符号（$）开头，被呈现值的名称和被包装值是一样的

       ```swift
       @propertyWrapper
       struct SmallNumber {
           private var number: Int
           var projectedValue: Bool
           init() {
               self.number = 0
               self.projectedValue = false
           }
           var wrappedValue: Int {
               get { return number }
               set {
                   if newValue > 12 {
                       number = 12
                       projectedValue = true
                   } else {
                       number = newValue
                       projectedValue = false
                   }
               }
           }
       }
       struct SomeStructure {
           @SmallNumber var someNumber: Int
       }
       var someStructure = SomeStructure()
       
       someStructure.someNumber = 4
       print(someStructure.$someNumber)
       // 打印 "false"
       
       someStructure.someNumber = 55
       print(someStructure.$someNumber)
       // 打印 "true"
       ```

     - 属性包装器可以返回任何类型的值作为它的**被呈现值**

     - 也可以在类型的一部分代码中访问被呈现值。可以在属性名称之前省略 `self.`。直接用`$name`

   - 全局变量和局部变量

     - 在全局或局部范围都可以定义*计算型变量*和为存储型变量定义观察器。计算型变量跟计算属性一样，返回一个计算结果而不是存储值，声明格式也完全一样。
     - 全局的常量或变量都是延迟计算的，跟 [延时加载存储属性]() 相似，不同的地方在于，全局的常量或变量不需要标记 `lazy` 修饰符。
     - 局部范围的常量和变量从不延迟计算。

   - 类型属性

     - 实例属性属于一个特定类型的实例；而**类型属性**类似于c++中的静态成员，为类型本身定义属性，无论创建了多少个该类型的实例，这些属性都只有唯一一份。这种属性就是*类型属性*。
     - 必须给存储型类型属性指定默认值，因为类型本身没有构造器，也就无法在初始化过程中使用构造器给类型属性赋值。
     - 存储型类型属性是延迟初始化的，它们只有在第一次被访问的时候才会被初始化。即使它们被多个线程同时访问，系统也保证只会对其进行一次初始化，并且不需要对其使用 `lazy` 修饰符。
     - 使用关键字 `static` 来定义类型属性。
     - 在为**类**定义**计算型类型属性**时，可以改用关键字 `class` 来支持子类对父类的实现进行重写（把`static` 改为`class`）。
     - 类型属性是通过*类型*本身来访问,通过点运算符访问（与c++一致）



### 方法

1. 类、结构体、枚举都可以定义实例方法，也可以定义类型方法

2. self属性

   - 类型的每一个实例都有一个隐含属性叫做 `self`，`self` 完全等同于该实例本身。
   - 只要在一个方法中使用一个已知的属性或者方法名称，如果你没有明确地写 `self`，Swift 假定你是指当前实例的属性或者方法。
   - 使用这条规则的主要场景是**实例方法的某个参数名称与实例的某个属性名称相同**的时候。在这种情况下，参数名称享有优先权，并且在引用属性时必须使用一种更严格的方式。这时你可以使用 `self` 属性来区分参数名称和属性名称。

3. 在实例方法中修改值类型

   - 结构体和枚举是*值类型*。默认情况下，值类型的属性不能在它的实例方法中被修改。

   - 需要在某个特定的方法中修改结构体或者枚举的属性，你可以为这个方法选择 `可变（mutating）`行为，然后就可以从其方法内部改变它的属性；并且这个方法做的任何改变都会在方法执行结束时写回到原始结构中。

     ```swift
     struct Point {
         var x = 0.0, y = 0.0
         mutating func moveBy(x deltaX: Double, y deltaY: Double) {
             x += deltaX
             y += deltaY
         }
     }
     var somePoint = Point(x: 1.0, y: 1.0)
     somePoint.moveBy(x: 2.0, y: 3.0)
     print("The point is now at (\(somePoint.x), \(somePoint.y))")
     // 打印“The point is now at (3.0, 4.0)”
     ```

   - 方法还可以给它隐含的 `self` 属性赋予一个全新的实例，这个新实例在方法结束时会替换现存实例。

     ```swift
     struct Point {
         var x = 0.0, y = 0.0
         mutating func moveBy(x deltaX: Double, y deltaY: Double) {
             self = Point(x: x + deltaX, y: y + deltaY)
         }
     }
     ```

   - 不能在结构体类型的常量（a constant of structure type）上调用可变方法，因为其属性不能被改变，即使属性是变量属性.

4. 类型方法

   - 定义在类型本身上调用的方法，这种方法就叫做*类型方法*。在方法的 `func` 关键字之前加上关键字 `static`，来指定类型方法。类还可以用关键字 `class` 来指定，从而允许子类重写父类该方法的实现。
   - 类型方法的方法体（body）中，`self` 属性指向这个类型本身，而不是类型的某个实例。这意味着你可以用 `self` 来消除类型属性和类型方法参数之间的歧义
   - 一个类型方法可以直接通过类型方法的名称调用本类中的其它类型方法，而无需在方法名称前面加上类型名称。



### 下标

1. 下标允许你通过在实例名称后面的方括号中传入一个或者多个索引值来对实例进行查询。它的语法类似于实例方法语法和计算型属性语法。定义下标使用 `subscript` 关键字，与定义实例方法类似，都是指定一个或多个输入参数和一个返回类型。与实例方法不同的是，下标可以设定为读写或只读。这种行为由 getter 和 setter 实现，类似计算型属性。

   ```swift
   subscript(index: Int) -> Int {
       get {
         // 返回一个适当的 Int 类型的值
       }
       set(newValue) {
         // 执行适当的赋值操作
       }
   }
   ```

   eg：

   ```swift
   struct TimesTable {
       let multiplier: Int
       subscript(index: Int) -> Int {
           return multiplier * index
       }
   }
   let threeTimesTable = TimesTable(multiplier: 3)
   print("six times three is \(threeTimesTable[6])")
   // 打印“six times three is 18”
   ```

2. 与函数一样，下标可以接受任意数量、任意类型的参数，并且为这些参数提供默认值。下标的返回值也可以是任意类型。但是下标不能使用 in-out 参数

3. 类型下标：定义一种在这个类型自身上调用的下标。这种下标被称作*类型下标*。你可以通过在 `subscript` 关键字之前写下 `static` 关键字的方式来表示一个类型下标。类类型可以使用 `class` 关键字来代替 `static`，它允许子类重写父类中对那个下标的实现。



### 继承

1. 类可以调用和访问超类的方法、属性和下标，并且可以重写这些方法，属性和下标来优化或修改它们的行为。Swift 会检查你的重写定义在超类中是否有匹配的定义，以此确保你的重写行为是正确的。

2. 可以为类中继承来的属性添加属性观察器，这样一来，当属性值改变时，类就会被通知到。可以为任何属性添加属性观察器，无论它原本被定义为存储型属性还是计算型属性。

3. 创建一个继承自超类的子类：

   ```swift
   class SomeClass: SomeSuperclass {
       // 这里是子类的定义
   }
   ```

4. 重写

   - 要加`override` 关键字。否则编译时会出错

   - 可以通过使用 `super` 前缀来访问超类版本的方法，属性或下标

     - 在方法 `someMethod()` 的重写实现中，可以通过 `super.someMethod()` 来调用超类版本的 `someMethod()` 方法。
     - 在属性 `someProperty` 的 getter 或 setter 的重写实现中，可以通过 `super.someProperty` 来访问超类版本的 `someProperty` 属性。
     - 在下标的重写实现中，可以通过 `super[someIndex]` 来访问超类版本中的相同下标。

   - 重写属性：

     - 子类并不知道继承来的属性是存储型的还是计算型的，它只知道继承来的属性会有一个名字和类型。你在重写一个属性时，必须将它的名字和类型都写出来。

     - 可以将一个继承来的只读属性重写为一个读写属性，只需要在重写版本的属性里提供 getter 和 setter 即可。但是，你不可以将一个继承来的读写属性重写为一个只读属性。如果你在重写属性中提供了 setter，那么你也一定要提供 getter。如果你不想在重写版本中的 getter 里修改继承来的属性值，你可以直接通过 `super.someProperty` 来返回继承来的值，其中 `someProperty` 是你要重写的属性的名字。

       ```swift
       class Car: Vehicle {
           var gear = 1
           override var description: String {
               return super.description + " in gear \(gear)"
           }
       }
       ```

   - 重写属性观察器：

     - 通过重写属性为一个继承来的属性添加属性观察器。
     - 不可以为继承来的常量存储型属性或继承来的只读计算型属性添加属性观察器。
     - 不可以同时提供重写的 setter 和重写的属性观察器。如果你想观察属性值的变化，并且你已经为那个属性提供了定制的 setter，那么你在 setter 中就可以观察到任何值变化了。

   - 防止重写：把方法，属性或下标标记为 *`final`* 来防止它们被重写，只需要在声明关键字前加上 `final` 修饰符即可

5. 可以通过在关键字 `class` 前添加 `final` 修饰符（`final class`）来将整个类标记为 final 。这样的类是不可被继承的，试图继承这样的类会导致编译报错。



