# 表达式（Expressions）
-----------------

> 1.0
> 翻译：[sg552](https://github.com/sg552)
> 校对：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

> 2.0
> 翻译+校对：[EudeMorgen](https://github.com/EudeMorgen)

> 2.1
> 翻译：[mmoaay](https://github.com/mmoaay)

本页包含内容：

- [前缀表达式](#prefix_expressions)
    - [try 运算符](#try_operator)
- [二元表达式](#binary_expressions)
    - [赋值表达式](#assignment_operator)
    - [三元条件运算符](#ternary_conditional_operator)
    - [类型转换运算符](#type-casting_operators)
- [基本表达式](#primary_expressions)
    - [字面量表达式](#literal_expression)
    - [self 表达式](#self_expression)
    - [super 表达式](#superclass_expression)
    - [闭包表达式](#closure_expression)
    - [隐式成员表达式](#implicit_member_expression)
    - [圆括号表达式](#parenthesized_expression)
    - [通配符表达式](#wildcard_expression)
- [后缀表达式](#postfix_expressions)
    - [函数调用表达式](#function_call_expression) 
    - [构造器表达式](#initializer_expression)
    - [显式成员表达式](#explicit_member_expression)
    - [后缀 self 表达式](#postfix_self_expression)
    - [dynamicType 表达式](#dynamic_type_expression)
    - [下标表达式](#subscript_expression)
    - [强制取值表达式](#forced-Value_expression)
    - [可选链表达式](#optional-chaining_expression)

Swift 中存在四种表达式：前缀表达式，二元表达式，基本表达式和后缀表达式。表达式可以返回一个值，还可以执行某些代码。

前缀表达式和二元表达式就是对某些表达式使用各种运算符。基本表达式是最短小的表达式，它提供了获取值的一种途径。后缀表达式则允许你建立复杂的表达式，例如函数调用和成员访问。每种表达式都在下面有详细论述。

> 表达式语法  
<a name="expression"></a>
> *表达式* → [*try运算符*](#try-operator)<sub>可选</sub> [*前缀表达式*](#prefix-expression) [*二元表达式列表*](#binary-expressions)<sub>可选</sub>  
<a name="expression-list"></a>
> *表达式列表* → [*表达式*](#expression) | [*表达式*](#expression) **,** [*表达式列表*](#expression-list)  

<a name="prefix_expressions"></a>
## 前缀表达式
 
前缀表达式由可选的前缀运算符和表达式组成。前缀运算符只接收一个参数。

关于这些运算符的更多信息，请参阅 [基本运算符](../chapter2/02_Basic_Operators.html) 和 [高级运算符](../chapter2/25_Advanced_Operators.html)。

关于 Swift 标准库提供的运算符的更多信息，请参阅 [*Swift Standard Library Operators Reference*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Reference/Swift_StandardLibrary_Operators/index.html#//apple_ref/doc/uid/TP40016054)。

除了标准库运算符，你也可以对某个变量使用 `&` 运算符，从而将其传递给函数的输入输出参数。 更多信息，请参阅 [输入输出参数](../chapter2/06_Functions.html#in_out_parameters)。

> 前缀表达式语法  
<a name="prefix-expression"></a>
> *前缀表达式* → [*前缀运算符*](02_Lexical_Structure.html#prefix-operator)<sub>可选</sub> [*后缀表达式*](#postfix-expression)  
> *前缀表达式* → [*输入输出表达式*](#in-out-expression)  
<a name="in-out-expression"></a>
> *输入输出表达式* → **&** [*标识符*](02_Lexical_Structure.html#identifier)  

<a name="try_operator"></a>
### try 运算符

try 表达式由 `try` 运算符加上紧随其后的可抛出错误的表达式组成，形式如下：

> try `可抛出错误的表达式`

可选的 try 表达式由 `try?` 运算符加上紧随其后的可抛出错误的表达式组成，形式如下：

> try? `可抛出错误的表达式`

如果可抛出错误的表达式没有抛出错误，整个表达式返回的可选值将包含可抛出错误的表达式的返回值，否则，该可选值为 `nil`。

强制的 try 表达式由 `try!` 运算符加上紧随其后的可抛出错误的表达式组成，形式如下：

> try! `可抛出错误的表达式`

如果可抛出错误的表达式抛出了错误，将会引发运行时错误。

在二进制运算符左侧的表达式被标记上 `try`、`try?` 或者 `try!` 时，这个运算符对整个二进制表达式都产生作用。也就是说，你可以使用括号来明确运算符的作用范围。

```swift
sum = try someThrowingFunction() + anotherThrowingFunction()   // try 对两个方法调用都产生作用
sum = try (someThrowingFunction() + anotherThrowingFunction()) // try 对两个方法调用都产生作用
sum = (try someThrowingFunction()) + anotherThrowingFunction() // 错误：try 只对第一个方法调用产生作用
```

`try` 表达式不能出现在二进制运算符的的右侧，除非二进制运算符是赋值运算符或者 `try` 表达式是被圆括号括起来的。

关于 `try`、`try?` 和 `try!` 的更多信息，以及如何使用的例子，请参阅 [错误处理](../chapter2/18_Error_Handling.html)。

> try 表达式语法  
<a name="try-operator"></a> 
> *try 运算符* → **try** | **try?** | **try!**

<a name="binary_expressions"></a>
## 二元表达式

二元表达式形式如下：

> `左侧参数` `二元运算符` `右侧参数`

关于这些运算符的更多信息，请参阅 [基本运算符](../chapter2/02_Basic_Operators.html) 和 [高级运算符](../chapter2/25_Advanced_Operators.html)。

关于 Swift 标准库提供的运算符的更多信息，请参阅 [*Swift Standard Library Operators Reference*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Reference/Swift_StandardLibrary_Operators/index.html#//apple_ref/doc/uid/TP40016054)。

> 注意  
> 在解析时，一个二元表达式将作为一个简单列表表示，然后根据运算符的优先级，再进一步进行组合。例如，`2 + 3 * 5` 首先被看作具有五个元素的列表，即 `2`、`+`、`3`、`*`、`5`，随后根据运算符优先级组合为 `(2 + (3 * 5))`。

> 二元表达式语法  
<a name="binary-expression"></a>
> *二元表达式* → [*二元运算符*](02_Lexical_Structure.html#binary-operator) [*前缀表达式*](#prefix-expression)  
> *二元表达式* → [*赋值运算符*](#assignment-operator) [*try运算符*](#try-operator)<sub>可选</sub> [*前缀表达式*](#prefix-expression)  
> *二元表达式* → [*条件运算符*](#conditional-operator) [*try运算符*](#try-operator)<sub>可选</sub> [*前缀表达式*](#prefix-expression)  
> *二元表达式* → [*类型转换运算符*](#type-casting-operator)  
<a name="binary-expressions"></a>
> *二元表达式列表* → [*二元表达式*](#binary-expression) [*二元表达式列表*](#binary-expressions)<sub>可选</sub>

<a name="assignment_operator"></a>
### 赋值表达式

赋值表达式会为某个给定的表达式赋值，形式如下；

> `表达式` = `值`

右边的值会被赋值给左边的表达式。如果左边表达式是一个元组，那么右边必须是一个具有同样元素个数的元组。嵌套元组也是允许的。

```swift
(a, _, (b, c)) = ("test", 9.45, (12, 3))
// a 为 "test"，b 为 12，c 为 3，9.45 会被忽略
```

赋值运算符不返回任何值。

> 赋值运算符语法  
<a name="assignment-operator"></a>
> *赋值运算符* → **=**  

<a name="ternary_conditional_operator"></a>
### 三元条件运算符

三元条件运算符会根据条件来对两个给定表达式中的一个进行求值，形式如下：

> `条件` ? `表达式（条件为真则使用）` : `表达式（条件为假则使用）`

如果条件为真，那么对第一个表达式进行求值并返回结果。否则，对第二个表达式进行求值并返回结果。未使用的表达式不会进行求值。

关于使用三元条件运算符的例子，请参阅 [三元条件运算符](../chapter2/02_Basic_Operators.html#ternary_conditional_operator)。

> 三元条件运算符语法  
<a name="conditional-operator"></a>
> *三元条件运算符* → **?** [try运算符](#try-operator)<sub>可选</sub> [*表达式*](#expression) **:**  

<a name="type-casting_operators"></a>
### 类型转换运算符

有 4 种类型转换运算符：`is`、`as`、`? `和`!`。它们有如下的形式：

> `表达式` is `类型`  
`表达式` as `类型`  
`表达式` is? `类型`  
`表达式` as! `类型`  

`is` 运算符在运行时检查表达式能否向下转化为指定的类型，如果可以则返回 `ture`，否则返回 `false`。

`as` 运算符在编译时执行向上转换和桥接。向上转换可将表达式转换成超类的实例而无需使用任何中间变量。以下表达式是等价的：

```swift
func f(any: Any) { print("Function for Any") }
func f(int: Int) { print("Function for Int") }
let x = 10
f(x)
// 打印 “Function for Int”
 
let y: Any = x
f(y)
// 打印 “Function for Any”
 
f(x as Any)
// 打印 “Function for Any”
```

桥接可将 Swift 标准库中的类型（例如 `String`）作为一个与之相关的 Foundation 类型（例如 `NSString`）来使用，而不需要新建一个实例。关于桥接的更多信息，请参阅 [*Using Swift with Cocoa and Objective-C*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216) 中的 [Working with Cocoa Data Types](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/WorkingWithCocoaDataTypes.html#//apple_ref/doc/uid/TP40014216-CH6)。

`as?` 运算符有条件地执行类型转换，返回目标类型的可选值。在运行时，如果转换成功，返回的可选值将包含转换后的值，否则返回 `nil`。如果在编译时就能确定转换一定会成功或是失败，则会编译出错。

`as!` 运算符执行强制类型转换，返回目标类型的非可选值。如果转换失败，则会导致运行时错误。表达式 `x as T` 效果等同于 `(x as? T)!`。

关于类型转换的更多内容和例子，请参阅 [类型转换](../chapter2/19_Type_Casting.html)。

> 类型转换运算符语法  
<a name="type-casting-operator"></a>  
> *类型转换运算符* → **is** [*类型*](03_Types.html#type)  
> *类型转换运算符* → **as** [*类型*](03_Types.html#type)  
> *类型转换运算符* → **is** **?** [*类型*](03_Types.html#type)  
> *类型转换运算符* → **as** **!** [*类型*](03_Types.html#type)   

<a name="primary_expressions"></a>
## 基本表达式

基本表达式是最基本的表达式。 它们可以跟前缀表达式、二元表达式、后缀表达式以及其他基本表达式组合使用。

> 基本表达式语法  
> *基本表达式* → [*标识符*](02_Lexical_Structure.md#identifier) [*泛型实参子句*](08_Generic_Parameters_and_Arguments.md#generic-argument-clause)<sub>可选</sub>  
> *基本表达式* → [*字面量表达式*](#literal-expression)  
> *基本表达式* → [*self表达式*](#self-expression)  
> *基本表达式* → [*超类表达式*](#superclass-expression)  
> *基本表达式* → [*闭包表达式*](#closure-expression)  
> *基本表达式* → [*圆括号表达式*](#parenthesized-expression)  
> *基本表达式* → [*隐式成员表达式*](#implicit-member-expression)  
> *基本表达式* → [*通配符表达式*](#wildcard-expression)  

<a name="literal_expression"></a>
### 字面量表达式

字面量表达式可由普通字面量（例如字符串或者数字），字典或者数组字面量，或者下面列表中的特殊字面量组成：

字面量 | 类型 | 值
:------------- | :---------- | :----------
`__FILE__` | `String` | 所在的文件名
`__LINE__` | `Int` | 所在的行数
`__COLUMN__` | `Int` | 所在的列数
`__FUNCTION__` | `String` | 所在的声明的名字

对于 `__FUNCTION__`，在函数中会返回当前函数的名字，在方法中会返回当前方法的名字，在属性的存取器中会返回属性的名字，在特殊的成员如 `init` 或 `subscript` 中会返回这个关键字的名字，在某个文件中会返回当前模块的名字。

`__FUNCTION__` 作为函数或者方法的默认参数值时，该字面量的值取决于函数或方法调用时所处的环境。

```swift
func logFunctionName(string: String = __FUNCTION__) {
    print(string)
}
func myFunction() {
    logFunctionName() // 打印 “myFunction()”
}
myFunction()
```

数组字面量是值的有序集合，形式如下：

> [`值 1`, `值 2`, `...`]

数组中的最后一个表达式可以紧跟一个逗号。数组字面量的类型是 `[T]`，这个 `T` 就是数组中元素的类型。如果数组中包含多种类型，`T` 则是跟这些类型最接近的的公共父类型。空数组字面量由一组方括号定义，可用来创建特定类型的空数组。

```swift
var emptyArray: [Double] = []
```

字典字面量是一个包含无序键值对的集合，形式如下：

> [`键 1` : `值 1`, `键 2` : `值 2`, `...`]

字典中的最后一个表达式可以紧跟一个逗号。字典字面量的类型是 `[Key : Value]`，`Key` 表示键的类型，`Value` 表示值的类型。如果字典中包含多种类型，那么 `Key` 表示的类型则为所有键最接近的公共父类型，`Value` 也是同样如此。一个空的字典字面量由方括号中加一个冒号组成（`[:]`），从而与空数组字面量区分开，可以使用空字典字面量来创建特定类型的字典。

```swift
var emptyDictionary: [String : Double] = [:]
```

> 字面量表达式语法  

<a name="literal-expression"></a>
> *字面量表达式* → [*字面量*](02_Lexical_Structure.md#literal)  
> *字面量表达式* → [*数组字面量*](#array-literal) | [*字典字面量*](#dictionary-literal)  
> *字面量表达式* → **\_\_FILE\_\_** | **\_\_LINE\_\_** | **\_\_COLUMN\_\_** | **\_\_FUNCTION\_\_**  

<a name="array-literal"></a>
> *数组字面量* → **[** [*数组字面量项列表*](#array-literal-items)<sub>可选</sub> **]**  
<a name="array-literal-items"></a>
> *数组字面量项列表* → [*数组字面量项*](#array-literal-item) **,**<sub>可选</sub> | [*数组字面量项*](#array-literal-item) **,** [*数组字面量项列表*](#array-literal-items)  
<a name="array-literal-item"></a>
> *数组字面量项* → [*表达式*](#expression)  

<a name="dictionary-literal"></a>
> *字典字面量* → **[** [*字典字面量项列表*](#dictionary-literal-items) **]** | **[** **:** **]**  
<a name="dictionary-literal-items"></a>
> *字典字面量项列表* → [*字典字面量项*](#dictionary-literal-item) **,**<sub>可选</sub> | [*字典字面量项*](#dictionary-literal-item) **,** [*字典字面量项列表*](#dictionary-literal-items)  
<a name="dictionary-literal-item"></a>
> *字典字面量项* → [*表达式*](#expression) **:** [*表达式*](#expression)  

<a name="self_expression"></a>
### self 表达式

self表达式是对 当前type 或者当前instance的引用。它的形式如下：

> self  
> self.`member name`  
> self[`subscript index`]  
> self(`initializer arguments`)
> self.init(`initializer arguments`)

如果在 initializer, subscript, instance method中，self等同于当前type的instance. 在一个静态方法（static method）, 类方法（class method）中， self等同于当前的type.

当访问 member（成员变量时）， self 用来区分重名变量（例如函数的参数）.  例如，
（下面的 self.greeting 指的是 var greeting: String, 而不是 init（greeting: String） ）

```swift
class SomeClass {
    var greeting: String
    init(greeting: String) {
        self.greeting = greeting
    }
}
```

在mutating 方法中， 你可以使用self 对 该instance进行赋值。

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX(deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

> self 表达式语法  
<a name="self-expression"></a>
> *self 表达式* → **self**  
> *self 表达式* → **self** **.** [*标识符*](02_Lexical_Structure.md#identifier)  
> *self 表达式* → **self** **[** [*表达式*](#expression) **]**  
> *self 表达式* → **self** **.** **init**  

<a name="superclass_expression"></a>
### super 表达式

超类表达式可以使我们在某个class中访问它的超类. 它有如下形式：

> super.`member name`  
> super[`subscript index`]  
> super.init(`initializer arguments`)

形式1 用来访问超类的某个成员（member）. 形式2 用来访问该超类的 subscript 实现。 形式3 用来访问该超类的 initializer.

子类（subclass）可以通过超类（superclass）表达式在它们的 member, subscripting 和 initializers 中来利用它们超类中的某些实现（既有的方法或者逻辑）。

> super 表达式语法  
<a name="superclass-expression"></a>
> *super 表达式* → [*超类方法表达式*](../chapter3/04_Expressions.html#superclass_method_expression) | [*超类下标表达式*](../chapter3/04_Expressions.html#超类下标表达式) | [*超类构造器表达式*](../chapter3/04_Expressions.html#superclass_initializer_expression)  
<a name="superclass-method-expression"></a>
> *超类方法表达式* → **super** **.** [*标识符*](LexicalStructure.html#identifier)  
<a name="superclass-subscript-expression"></a>
> *超类下标表达式* → **super** **[** [*表达式*](../chapter3/04_Expressions.html#expression) **]**  
<a name="superclass-initializer-expression"></a>
> *超类构造器表达式* → **super** **.** **init**  

<a name="closure_expression"></a>
### 闭包表达式

闭包（closure） 表达式可以建立一个闭包（在其他语言中也叫 lambda, 或者 匿名函数（anonymous function））. 跟函数（function）的声明一样， 闭包（closure）包含了可执行的代码（跟方法主体（statement）类似） 以及接收（capture）的参数。 它的形式如下：

```swift
{ (parameters) -> return type in
    statements
}
```

闭包的参数声明形式跟方法中的声明一样, 请参见：[Function Declaration](TODO：添加链接).

闭包还有几种特殊的形式, 让使用更加简洁：

- 闭包可以省略 它的参数的type 和返回值的type. 如果省略了参数和参数类型，就也要省略 'in'关键字。 如果被省略的type 无法被编译器获知（inferred） ，那么就会抛出编译错误。
- 闭包可以省略参数，转而在方法体（statement）中使用 $0, $1, $2 来引用出现的第一个，第二个，第三个参数。
- 如果闭包中只包含了一个表达式，那么该表达式就会自动成为该闭包的返回值。 在执行 'type inference '时，该表达式也会返回。

下面几个 闭包表达式是 等价的：

```swift
myFunction {
    (x: Int, y: Int) -> Int in
    return x + y
}

myFunction {
    (x, y) in
    return x + y
}

myFunction { return $0 + $1 }

myFunction { $0 + $1 }
```

关于 向闭包中传递参数的内容，参见： [Function Call Expression](TODO：添加链接).

#### 捕获列表

闭包表达式可以通过一个参数列表（capture list） 来显式指定它需要的参数。 参数列表由中括号 [] 括起来，里面的参数由逗号','分隔。一旦使用了参数列表，就必须使用'in'关键字（在任何情况下都得这样做，包括忽略参数的名字，type, 返回值时等等）。

在闭包的参数列表（ capture list）中， 参数可以声明为 'weak' 或者 'unowned' .

```swift
myFunction { print(self.title) }                    // strong capture
myFunction { [weak self] in print(self!.title) }    // weak capture
myFunction { [unowned self] in print(self.title) }  // unowned capture
```

在参数列表中，也可以使用任意表达式来赋值. 该表达式会在 闭包被执行时赋值，然后按照不同的力度来获取（这句话请慎重理解）。（captured with the specified strength. ） 例如：

```swift
// Weak capture of "self.parent" as "parent"
myFunction { [weak parent = self.parent] in print(parent!.title) }
```

关于闭包表达式的更多信息和例子，请参见： [Closure Expressions](TODO：添加链接)，关于更多参数列表的信息和例子，请参见： [Resolving Strong Reference Cycles for Closures](TODO：添加链接)。

> 闭包表达式语法  

<a name="closure-expression"></a>
> *闭包表达式* → **{** [*闭包签名(Signational)*](../chapter3/04_Expressions.html#closure_signature)<sub>可选</sub> [*多条语句(Statements)*](../chapter3/10_Statements.html#statements) **}**  

<a name="closure-signature"></a>
> *闭包签名(Signational)* → [*参数子句*](../chapter3/05_Declarations.html#parameter_clause) [*函数结果*](../chapter3/05_Declarations.html#function_result)<sub>可选</sub> **in**  
> *闭包签名(Signational)* → [*标识符列表*](LexicalStructure.html#identifier_list) [*函数结果*](../chapter3/05_Declarations.html#function_result)<sub>可选</sub> **in**  
> *闭包签名(Signational)* → [*捕获(Capature)列表*](../chapter3/04_Expressions.html#capture_list) [*参数子句*](../chapter3/05_Declarations.html#parameter_clause) [*函数结果*](../chapter3/05_Declarations.html#function_result)<sub>可选</sub> **in**  
> *闭包签名(Signational)* → [*捕获(Capature)列表*](../chapter3/04_Expressions.html#capture_list) [*标识符列表*](LexicalStructure.html#identifier_list) [*函数结果*](../chapter3/05_Declarations.html#function_result)<sub>可选</sub> **in**  
> *闭包签名(Signational)* → [*捕获(Capature)列表*](../chapter3/04_Expressions.html#capture_list) **in**  

<a name="capture-list"></a>
> *捕获列表* → **[** [*捕获说明符*](#capture-specifier) [*表达式*](#expression) **]**  
<a name="capture-list-items"></a>
> *捕获列表项列表* → [*捕获列表项*](#capture-list-item) | [*捕获列表项*](#capture-list-item) **,** [*捕获列表项列表*](#capture-list-items)
<a name="capture-list-item"></a>
> *捕获列表项* → [*捕获说明符*](#capture-specifier)<sub>可选</sub> [*表达式*](#expression)
<a name="capture-specifier"></a>
> *捕获说明符* → **weak** | **unowned** | **unowned(safe)** | **unowned(unsafe)**  

<a name="implicit_member_expression"></a>
### 隐式成员表达式

在可以判断出类型（type）的上下文（context）中，隐式成员表达式是访问某个type的member（ 例如 class method, enumeration case） 的简洁方法。 它的形式是：

> .`member name`

例子：

```swift
var x = MyEnumeration.SomeValue
x = .AnotherValue
```

> 隐式成员表达式语法  
<a name="implicit-member-expression"></a>
> *隐式成员表达式* → **.** [*标识符*](../chapter3/02_Lexical_Structure.html#identifier)  

<a name="parenthesized_expression"></a>
### 圆括号表达式

圆括号表达式由多个子表达式和逗号','组成。 每个子表达式前面可以有 identifier x: 这样的可选前缀。形式如下：

>（`identifier 1`: `expression 1`, `identifier 2`: `expression 2`, `...`）

圆括号表达式用来建立tuples ， 然后把它做为参数传递给 function. 如果某个圆括号表达式中只有一个 子表达式，那么它的type就是 子表达式的type。例如： （1）的 type是Int, 而不是（Int）

> 圆括号表达式语法  
<a name="parenthesized-expression"></a>
> *圆括号表达式* → **(** [*表达式元素列表*](#expression-element-list)<sub>可选</sub> **)**  
<a name="expression-element-list"></a>
> *表达式元素列表* → [*表达式元素*](#expression-element) | [*表达式元素*](#expression-element) **,** [*表达式元素列表*](#expression-element-list)  
<a name="expression-element"></a>
> *表达式元素* → [*表达式*](#expression) | [*标识符*](02_Lexical_Structure.md#identifier) **:** [*表达式*](#expression)  

<a name="wildcard_expression"></a>
### 通配符表达式

通配符表达式用来忽略传递进来的某个参数。例如：下面的代码中，10被传递给x, 20被忽略（译注：好奇葩的语法。。。）

```swift
(x, _) = (10, 20)
// x is 10, 20 is ignored
```

> 通配符表达式语法  
<a name="wildcard-expression"></a>
> *通配符表达式* → **_**  

<a name="postfix_expressions"></a>
## 后缀表达式

后缀表达式就是在某个表达式的后面加上 操作符。 严格的讲，每个主要表达式（primary expression）都是一个后缀表达式

Swift 标准库提供了下列后缀表达式：

- ++ Increment
- -- Decrement

对于这些操作符的使用，请参见： Basic Operators and Advanced Operators

> 后缀表达式语法  
<a name="postfix-expression"></a>
> *后缀表达式* → [*主表达式*](../chapter3/04_Expressions.html#primary_expression)  
> *后缀表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) [*后缀运算符*](../chapter3/02_Lexical_Structure.html#postfix_operator)  
> *后缀表达式* → [*函数调用表达式*](../chapter3/04_Expressions.html#function_call_expression)  
> *后缀表达式* → [*构造器表达式*](../chapter3/04_Expressions.html#initializer_expression)  
> *后缀表达式* → [*显示成员表达式*](../chapter3/04_Expressions.html#explicit_member_expression)  
> *后缀表达式* → [*后缀self表达式*](../chapter3/04_Expressions.html#postfix_self_expression)  
> *后缀表达式* → [*动态类型表达式*](../chapter3/04_Expressions.html#dynamic_type_expression)  
> *后缀表达式* → [*下标表达式*](../chapter3/04_Expressions.html#subscript_expression)  
> *后缀表达式* → [*强制取值(Forced Value)表达式*](../chapter3/04_Expressions.html#forced_value_expression)  
> *后缀表达式* → [*可选链(Optional Chaining)表达式*](../chapter3/04_Expressions.html#optional_chaining_expression)  

<a name="function_call_expression"></a>
### 函数调用表达式

函数调用表达式由函数名和参数列表组成。它的形式如下：

> `function name`(`argument value 1`, `argument value 2`)

如果该function 的声明中指定了参数的名字，那么在调用的时候也必须得写出来. 例如：

> `function name`(`argument name 1`: `argument value 1`, `argument name 2`: `argument value 2`)

可以在 函数调用表达式的尾部（最后一个参数之后）加上 一个闭包（closure） ， 该闭包会被目标函数理解并执行。它具有如下两种写法：

```swift
// someFunction takes an integer and a closure as its arguments
someFunction(x, {$0 == 13}+
someFunction(x) {$0 == 13}
```

如果闭包是该函数的唯一参数，那么圆括号可以省略。

```swift
// someFunction takes a closure as its only argument
myData.someMethod() {$0 == 13}
myData.someMethod {$0 == 13}
```

> 函数调用表达式语法  
> *函数调用表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) [*圆括号表达式*](../chapter3/04_Expressions.html#parenthesized_expression)  
> *函数调用表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) [*圆括号表达式*](../chapter3/04_Expressions.html#parenthesized_expression) _可选_ [*后缀闭包(Trailing Closure)*](../chapter3/04_Expressions.html#trailing_closure)  
> *后缀闭包(Trailing Closure)* → [*闭包表达式*](../chapter3/04_Expressions.html#closure_expression)  

<a name="initializer_expression"></a>
### 构造器表达式

Initializer表达式用来给某个Type初始化。 它的形式如下：

> `expression`.init(`initializer arguments`)

初始化函数表达式在调用函数时用来初始某个Type。 也可以使用初始化函数表达式来委托调用（delegate to ）到superclass的initializers.


```swift
class SomeSubClass: SomeSuperClass {
    init() {
        // subclass initialization goes here
        super.init()
    }
}
```

和函数类似， 初始化表达式可以用作数值。 举例来说：

```swift
// Type annotation is required because String has multiple initializers.
let initializer: Int -> String = String.init
let oneTwoThree = [1, 2, 3].map(initializer).reduce("", combine: +)
print(oneTwoThree)
// prints "123"
```

如果要用名字来指定某个type， 可以不用初始化函数表达式直接使用type的initializer。在其他情况下， 你必须使用初始化函数表达式。


```swift
let s1 = SomeType.init(data: 3)  // Valid
let s2 = SomeType(data: 1)       // Also valid

let s4 = someValue.dynamicType(data: 5)       // Error
let s3 = someValue.dynamicType.init(data: 7)  // Valid
```

> 构造器表达式语法  
> *构造器表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) **.** **init**  

<a name="explicit_member_expression"></a>
### 显式成员表达式

显示成员表达式允许我们访问type, tuple, module的成员变量。它的形式如下：

> `expression`.`member name`

该member 就是某个type在声明时候所定义（declaration or extension） 的变量, 例如：

```swift
class SomeClass {
    var someProperty = 42
}
let c = SomeClass()
let y = c.someProperty  // Member access
```

对于tuple, 要根据它们出现的顺序（0, 1, 2...）来使用:

```swift
var t = (10, 20, 30)
t.0 = t.1
// Now t is (20, 20, 30)
```

对于某个module的member的调用，只能调用在top-level声明中的member.

> 显式成员表达式语法  
> *显示成员表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) **.** [*十进制数字*](../chapter3/02_Lexical_Structure.html#decimal_digit)  
> *显示成员表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) **.** [*标识符*](../chapter3/02_Lexical_Structure.html#identifier) [*泛型参数子句*](GenericParametersAndArguments.html#generic_argument_clause) _可选_  

<a name="postfix_self_expression"></a>
### 后缀 self 表达式

后缀表达式由 某个表达式 + '.self' 组成. 形式如下：

> `expression`.self  
> `type`.self  

形式1 表示会返回 expression 的值。例如： x.self 返回 x

形式2：返回对应的type。我们可以用它来动态的获取某个instance的type。

> 后缀Self 表达式语法  
> *后缀self表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) **.** **self**  

<a name="dynamic_type_expression"></a>
### dynamicType 表达式

（因为 dynamicType 是一个独有的方法，所以这里保留了英文单词，未作翻译, --- 类似与self expression）

dynamicType 表达式由 某个表达式 + '.dynamicType' 组成。

> `expression`.dynamicType

上面的形式中， expression 不能是某type的名字（当然了，如果我都知道它的名字了还需要动态来获取它吗）。动态类型表达式会返回"运行时"某个instance的type, 具体请看下面的列子：

```swift
class SomeBaseClass {
    class func printClassName() {
        println("SomeBaseClass")
    }
}
class SomeSubClass: SomeBaseClass {
    override class func printClassName() {
        println("SomeSubClass")
    }
}
let someInstance: SomeBaseClass = SomeSubClass()

// someInstance is of type SomeBaseClass at compile time, but
// someInstance is of type SomeSubClass at runtime
someInstance.dynamicType.printClassName()
// prints "SomeSubClass"
```

> 动态类型表达式语法  
> *动态类型表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) **.** **dynamicType**  

<a name="subscript_expression"></a>
### 下标脚本表达式

下标脚本表达式提供了通过下标脚本访问getter/setter 的方法。它的形式是：

> `expression`[`index expressions`]

可以通过下标脚本表达式通过getter获取某个值，或者通过setter赋予某个值.

关于subscript的声明，请参见： Protocol Subscript Declaration.

> 附属脚本表达式语法  
> *附属脚本表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) **[** [*表达式列表*](../chapter3/04_Expressions.html#expression_list) **]**  

<a name="forced-Value_expression"></a>
### 强制取值表达式

强制取值表达式用来获取某个目标表达式的值（该目标表达式的值必须不是nil ）。它的形式如下：

> `expression`!

如果该表达式的值不是nil, 则返回对应的值。 否则，抛出运行时错误（runtime error）。
返回的值可能会被需改，可以是被赋值了，也可以是出现异常造成的。比如：
```swift
var x: Int? = 0
x!++
// x is now 1
 
var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]
someDictionary["a"]![0] = 100
// someDictionary is now [b: [10, 20], a: [100, 2, 3]] 
```

> 强制取值(Forced Value)语法  
> *强制取值(Forced Value)表达式* → [*后缀表达式*](../chapter3/04_Expressions.html#postfix_expression) **!**  

<a name="optional-chaining_expression"></a>
### 可选链表达式

可选链表达式由目标表达式 + '?' 组成，形式如下：

> `expression`?

后缀'?' 返回目标表达式的值，把它做为可选的参数传递给后续的表达式

如果某个后缀表达式包含了可选链表达式，那么它的执行过程就比较特殊： 首先先判断该可选链表达式的值，如果是 nil, 整个后缀表达式都返回 nil, 如果该可选链的值不是nil, 则正常返回该后缀表达式的值（依次执行它的各个子表达式）。在这两种情况下，该后缀表达式仍然是一个optional type（In either case, the value of the postfix expression is still of an optional type）

如果某个"后缀表达式"的"子表达式"中包含了"可选链表达式"，那么只有最外层的表达式返回的才是一个optional type. 例如，在下面的例子中， 如果c 不是nil, 那么 c?.property.performAction（） 这句代码在执行时，就会先获得c 的property方法，然后调用 performAction（）方法。 然后对于 "c?.property.performAction（）" 这个整体，它的返回值是一个optional type.

```swift
var c: SomeClass?
var result: Bool? = c?.property.performAction()
```

如果不使用可选链表达式，那么 上面例子的代码跟下面例子等价：

```swift
if let unwrappedC = c {
    result = unwrappedC.property.performAction()
}
```
后缀'?' 返回目标表达式的值可能会被修改，可能是由于出现了赋值，也有可能是出现异常而产生的修改。如果可选链表达式为`nil`，则表达式右边的复制操作不会被执行。比如：
```swift
func someFunctionWithSideEffects() -> Int {
    return 42  // No actual side effects.
}
var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]
 
someDictionary["not here"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects is not evaluated
// someDictionary is still [b: [10, 20], a: [1, 2, 3]]
 
someDictionary["a"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects is evaluated and returns 42
// someDictionary is now [b: [10, 20], a: [42, 2, 3]]
```

> 可选链表达式语法  
<a name="optional-chaining-expression"></a>
> *可选链表达式* → [*后缀表达式*](#postfix-expression) **?**  
