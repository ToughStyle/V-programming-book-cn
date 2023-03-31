# 第7章 V中的函数

到目前为止，在本书中，我们已经学习了V的基本编程功能，包括数组、映射、条件和迭代语句。现在是时候学习如何在V中编写函数了。编写软件应用程序时，大多数程序员都喜欢将一组语句(例如变量声明和执行算术或逻辑操作)分组，或者遍历数组或映射元素，然后根据需要对它们进行筛选。将逻辑相关的语句封装或分组，为其命名，并可选择提供输入参数和返回类型的技巧通常称为编写函数。

在我们开始之前，以下是本章中将涵盖的主题的简要概述：

- 介绍函数

- 了解函数的类型

- 了解函数的特点

本章的最后，您将对V提供的各种类型的函数有扎实的了解。此外，您将能够编写基本函数、匿名函数和高阶函数。本章还将指导您了解函数具备的广泛功能，并为每个功能提供代码示例。

### 技术要求

完整的本章源代码可在 https://github.com/ToughStyle/V-programming-book-cn/tree/main/codes/Chapter07 上找到。

## 介绍函数

函数允许您在代码块内逻辑封装一组指令，以执行特定操作。通常需要为它们提供一个代表其基本逻辑的名称。函数可能需要输入参数来执行操作。此外，它们可能会返回操作结果。因此，函数提供了代码可重用性和代码可读性。

如果需要，函数可以调用另一个函数。在某些情况下，函数可以调用自身执行递归操作。

V 简化了与函数的处理。在 V 中，函数是使用 `fn` 关键字创建的。在 V 中创建函数的典型语法如下所示：
```v
ACCESS-MODIFIER fn FUNCTION_NAME(ARGUMENT1_NAME ARGUMENT1_DATATYPE, ARGUMENT2_NAME ARGUMENT2_DATA){

    OPERATIONS

}
```
在上述语法中，可以识别函数的两部分：

- 方法签名或函数签名

- 函数体

一般来说，方法或函数签名由方法的第一行标识。它具有访问修饰符、`fn` 关键字，后跟方法名，然后在括号中，您有逗号分隔的参数，包括参数名称和参数数据类型。然后，方法签名的最后一部分表示返回类型。

方法体将具有一组例程，这些例程将使用参数(如果提供)执行函数，并可选择返回值，如在方法签名中定义的。

当我们刚刚学习写函数的基本语法时，我们将进一步了解使用代码示例中提供的 V 中可用函数的不同类型。

## 了解函数的类型

在本节中，我们将研究可以在 V 中编写的不同类型的函数。我们将从基于刚刚学习的语法编写的基本或最常见的函数开始。然后，我们将看一下如何编写没有任何函数名称的匿名函数，并探索使用匿名函数的各种方法。在此之后，我们将学习如何创建接受其他函数作为输入参数的函数，并查看函数返回其他函数作为其返回类型的函数，这通常称为高阶函数。

### 主函数

首先，让我们从主函数开始。该函数是 V 文件或具有多个 V 文件或模块的项目的入口点。每当您使用 `v run filename.v` 运行文件或使用 `v run .` 命令运行项目时，执行控制器都会查找主函数的存在并从那里开始。但是，在编写主函数时，有一些需要注意的事项：

- 主函数不应接受任何输入参数。

- 主函数不应具有返回类型。

- 主函数需要放置在目录结构的根目录中的文件中。

- 编写主函数的模块或文件由 `module main` 语句指定。

以下代码演示了主函数：
```v
module main

fn main() {

    println('Welcome to the World of V!')

}
```
如上所述，从上述代码中我们可以观察到主函数不具有任何输入参数，也没有返回类型。我们还可以观察到，在顶部指定了 `module main` 语句。这使得 V 能够识别并从主模块的主函数开始执行。

您将在本章后面的章节中学习更多关于输入参数和返回类型的知识。本节还提到了模块，但我们将在第9章《V中的模块》中更详细地介绍模块。

### 基本函数

简而言之，基本函数只是具有给定函数名称的包含逻辑相关语句的函数体。它们还可以接受输入参数，这些参数可能被认为是执行操作所必需的，并且如果函数返回任何值作为操作结果，则可以指定返回类型。V 中的函数提供了许多功能，我们将在本章的后续部分进行介绍。但是，首先让我们从一个简单的函数开始。
下面的代码展示了一个简单的函数`greet`:
```v
fn greet(msg string) {

    println(msg)

}
```
上述代码演示了实现一个`greet`函数。`greet`函数的作用是打印提供给它的消息作为输入参数。假设你调用`greet('Hello, Welcome to the world of V programming')`函数; 它将把该消息打印到控制台。我们可以观察到`greet`函数没有返回类型。在本章后续部分中，我们将学习如何创建和使用含有返回值的函数。

我们刚刚学习了如何创建非常基本的函数，V 允许您创建其他各种类型的函数，例如匿名函数和高阶函数。接下来，我们将使用代码示例详细解释如何编写匿名函数和高阶函数。

### 匿名函数

匿名函数是没有名称的函数块，可以在另一个函数内创建。匿名函数可以随时声明并分配给变量，以便通过调用该变量来调用它们。匿名函数的作用域在声明它们的函数范围内。让我们从以下代码开始更好地理解匿名函数：
```v
module main

fn main() {

    greet := fn (name string) {

        println('Hello, $name')

    }

    greet('Pavan')

    greet('Sahithi')

}
```
上述代码描述了一个匿名函数，该匿名函数向我们打招呼并输出`Hello`。匿名函数赋值给一个变量`greet`，并接受字符串类型的输入参数`name`。匿名函数也可以返回值；但是，在这种情况下，它仅使用`println`语句打印问候语到标准输出。

以下是输出：
```
Hello, Pavan

Hello, Sahithi
```
匿名函数还可以在处理数组时使用。有关如何使用匿名函数的详细演示，请参阅第5章《V中的数组和映射》的"在数组元素上应用映射"部分。

### 高阶函数

V 允许您定义接受或返回其他函数的函数。这些函数类型通常称为高阶函数。在接下来的章节中，我们将详细研究接受函数作为输入参数和返回另一个函数作为其返回类型的高阶函数。

### 接受其他函数作为输入参数的高阶函数

在本节中，我们将通过详细的演示理解接受另一个函数作为输入参数的高阶函数的概念。让我们定义三个返回string类型消息的函数，如下所示：
```v
fn greet_morning() string {

    return 'Good Morning'

}

fn greet_noon() string {

    return 'Good Afternoon'

}

fn greet_evening() string {

    return 'Good Evening'

}
```
请注意，这三个函数是基本函数，每个函数都返回表示不同问候语的字符串：Good Morning，Good Afternoon和Good Evening。现在，我们将创建一个高阶函数，它接受两个输入参数。其中一个输入参数是一个函数类型，而另一个是字符串类型：
```v
fn greet(f fn () string, name string) string {

        return '$f(), $name!'

}
```
这里，`greet`函数是一个高阶函数，接受返回`string`类型的任何函数。为了使`greet`函数接受一个函数，它必须遵循`greet`函数方法签名指定的标准。因此，我们来看一下`f fn() string`输入参数。请注意，名称为f的输入参数必须是一个函数，因为它由`fn() string`类型的函数识别出来。在`fn() string`表达式中，空括号()指定传递的函数必须是不带任何输入参数的函数，并且`fn()`中的类型指示该函数必须返回`string`类型的值。
由于 `greet_morning`、`greet_noon` 和 `greet_evening` 符合作为 `greet` 函数的第一个输入参数提供的要求，让我们尝试通过以下方式传递它们：
```v
mut res := greet(greet_morning, 'Pavan')

println(res)

res = greet(greet_evening, 'Sahithi')

println(res)
```
在这里，我们每次都将 `greet_morning` 和 `greet_evening` 函数作为输入参数传递，带上将被问候的人的姓名。

作为高阶函数的参数传递的函数的签名必须匹配高阶函数的输入参数中指定的定义。此外，请注意，当我们将函数作为参数传递时，我们只是指定函数的名称，没有任何括号。作为输入参数传递的函数所需的任何参数都必须由高阶函数提供。因此，在这种情况下，高阶 `greet` 函数必须负责向其接受的函数提供参数。

除了将预定义的函数作为输入参数传递给高阶函数之外，您还可以传递匿名函数。让我们来看一下如何将匿名函数作为输入参数传递给高阶函数的以下代码：
```v
res1 := greet(fn () string {

    return 'New year greetings to you'

}, 'Sahithi')

println(res)
```
在上面的代码中，我们将匿名函数作为输入参数传递给高阶函数。在这里，匿名函数与高阶 `greet` 函数中定义的 `f fn() string` 签名匹配。

以下是该示例的完整源代码和输出：
```v
module main

fn greet_morning() string {

    return 'Good Morning'

}

fn greet_noon() string {

    return 'Good Afternoon'

}

fn greet_evening() string {

    return 'Good Evening'

}

fn greet(f fn () string, name string) string {

    return '$f(), $name!'

}

fn main() {

    mut res := greet(greet_morning, 'Pavan')

    println(res)

    res = greet(greet_evening, 'Sahithi')

    println(res)

    res = greet(fn () string {

        return 'New year greetings to you'

    }, 'Sahithi')

    println(res)

}
```
以下是输出结果：
```
Good Morning, Pavan!

Good Evening, Sahithi!

New year greetings to you, Sahithi!
```
高阶 `greet` 函数采用 `greet_morning` 作为输入参数，同时携带 `Pavan` 的姓名，以 `greet_evening` 作为输入参数，同时携带 `Sahithi` 的姓名。它打印与提供给它的输入参数相匹配的问候语。当传递与输入参数作为函数规范匹配的匿名函数时，高阶 `greet` 函数处理结果，并以匿名函数返回的消息向我们致以问候。

### 返回其他函数的高阶函数

在本节中，我们将详细演示返回其他函数的高阶函数。

需要注意的是，高阶函数返回的函数必须与高阶函数签名中指定的返回类型匹配。具体来说，它必须匹配以下内容：

- 输入参数的数量：返回的函数必须接受与高阶函数返回类型中指定的相同数量的输入参数。

- 输入参数的类型：返回的函数的输入参数必须与高阶函数返回的函数签名中指定的参数数据类型匹配。

- 函数的返回类型：返回的函数必须返回与高阶函数返回的函数签名指定的类型相同的类型。
为了演示，我们试图创建一个函数，通过接受表示操作的输入参数返回所需类型的操作。

让我们定义一个名为 `Operation` 的枚举。下面定义的 `Operation` 枚举将其字段表示为我们要执行的操作列表：
```v
enum Operation {

    add

    sub

    mul

}
```
接下来，我们将定义三个函数：`adder`、`subtractor`和`multiplier`，它们分别执行其对应函数名称的逻辑，如下所示：
```v
fn adder(i int, j int) int {

    return i + j

}

fn subtractor(i int, j int) int {

    return i - j

}

fn multiplier(i int, j int) int {

    return i * j

}
```
现在，我们将定义一个接受 `Operation` 枚举的高阶函数。此函数根据 `match {}` 块处理的匹配操作之一返回 `adder`、`subtractor` 和 `multiplier` 中的一个函数，如下所示：
```v
fn fetch(op Operation) fn (int, int) int {

    return match op {

        .add {

            adder

        }

        .sub {

            subtractor

        }

        .mul {

            multiplier

        }

    }

}
```
`fetch` 函数是一个返回具有 `fn (int,int) int` 签名的另一个函数的高阶函数。

在我们的情况下，指定在高阶 `fetch` 函数上的返回类型是 `fn (int, int) int`。这意味着高阶 `fetch` 函数只能返回符合以下标准的函数：

- 返回的函数必须接受两个输入参数。

- 两个输入参数都必须是 int 类型。

- 返回的函数必须返回 int 类型。

因此，在上面定义的 `adder`、`subtractor` 和 `multiplier` 函数中，指定了与上述高阶 `fn(int, int) int` 函数类型匹配的返回类型。现在，我们将声明两个整数变量并通过查询高阶 `fetch` 函数来执行各种操作，以返回通过传递 `Operation` 枚举的字段的函数：
```v
i := 2

j := 5

// 获取 adder 函数并执行它

mut f := fetch(.add) // 返回 adder 函数

mut res := f(i, j) // 调用 adder(2, 5)

println('sum of $i and $j: $res')
```
上面的代码使用传递给 `Operation` 枚举的 `add` 字段作为输入参数调用高阶 `fetch` 函数。在这种情况下 `fetch(.add)` 将返回 `adder` 函数，然后将其分配给变量 `f`。由于我们以变量形式访问 `adder` 函数，因此我们将通过将 `i` 和 `j` 整数作为输入参数传递到存储在 `f` 变量中的 `adder` 函数来执行 `adder` 函数，它们匹配 `adder` 函数签名中定义的输入类型。

类似地，我们可以通过将 `Operation` 枚举的 `sub` 和 `mul` 字段作为输入参数传递给 `fetch` 函数来获取相应的减法和乘法函数来执行减法和乘法。以下是调用所有高阶函数的主函数：
```v
fn main() {

    i, j := 2, 5

    mut f := fetch(.add) // 返回 adder 函数

    mut res := f(i, j) // 调用 adder(2, 5)

    println('sum of $i and $j: $res')

    f = fetch(.sub) // 返回 subtractor 函数

    res = f(i, j) // 调用 subtractor(2, 5)

    println('difference of $i and $j: $res')

    f = fetch(.mul) // 返回 multipler 函数

    res = f(i, j) // 调用 multiplier(2, 5)

    println('product of $i and $j: $res')

}
```
以下是输出结果：
```
sum of 2 and 5: 7

difference of 2 and 5: -3

product of 2 and 5: 10
```
从前面的代码中，我们可以看到`fetch (.sub)` 语句返回`subtract`函数，这个函数赋值给了`f`，是可变变量。此时，我们已经可以访问减法函数。为了执行减法函数，我们传入`i`和`j`整数值分别为2和5。由于我们已定义`subtract`函数，它返回执行的整数值，我们将该值存储在可变变量`res`中。前面的代码还指示了如何以类似的方式获取`multiplier`函数，并输出显示`adder`、`subtract`和`multiplier`函数执行操作的相应结果。既然我们已经学习了V中的各种类型的函数，让我们来了解一下V中函数的特点。

## 了解函数的特点

在之前的章节中，我们学习了各种类型的函数并发现如何编写基本函数、匿名函数和高阶函数。作为V程序员，了解函数的各种特点是很重要的，它将使您能够在编程时顺利地使用它们。以下是V中函数特点的列表：

- 函数可以返回值或仅执行操作。

- 函数可以接受零个或多个输入参数。

- 函数可以返回多个值。

- 函数可以调用其他可访问的函数。

- 函数只允许使用数组、接口、映射、指针和结构体作为可变参数。

- 脚本模式下的函数声明应位于所有脚本语句之前。

- 函数不允许访问模块变量或全局变量。

- 函数不允许默认或可选参数。

- 函数可以具有可选的返回类型。

- 函数默认为私有，可以使用pub访问修饰符关键字将它们公开给其他模块。

- 函数允许您使用延迟块来推迟执行流程。

- 函数可以表示为数组或映射的元素。

- 让我们详细了解每个这些V中提供的函数特点。

- 函数可以返回值或仅执行操作

在V中，默认情况下，函数是纯函数。这意味着返回值只是其参数的函数，它们的评估没有副作用(除了I/O)。这意味着函数只负责它们所要做的事情，不承担其他责任。为了证明这一点，请考虑以下的`sum`函数：
```v
fn sum(a int, b int) int {

    return a + b

}
```
在上面的代码中，`sum`函数会将作为输入参数提供给它的2个数字相加，然后可以将数字5轻松地替换为函数调用`sum(2,3)`. 因此，`sum`函数只执行两个数字的相加操作并返回该值，而且不承担其他责任。这表明，对于相同的输入，纯函数总是提供一致的结果，无论我们调用它多少次。

如果要让函数返回操作的结果，则需要在方法签名中指定与方法返回的值匹配的返回类型。函数使用`return`关键字将值传递给它们的调用者：
```v
fn say_hello() string {

    return 'Hello!'

}

// call the method

res := say_hello()

println(res) // prints: Hello!
```
在上面的代码中，命名为`say_hello`的函数返回一个在`return`关键字之后指定为字符串数据类型的`Hello!`字符串，并与方法签名中指定的返回类型匹配。与上面的例子相反，有些方法不必要求调用者确认它们并可以执行操作，如I/O操作(如打印到控制台，文件操作，从数据库表中插入、更新或删除记录或设置环境变量)等。例如，考虑以下方法：
```v
fn console_greeter() {

    println('Hello!')

}

console_greeter() // prints: Hello!
```
此函数的调用者不期望从console_greeter方法获得返回值。这是因为它的方法签名中没有指定任何返回类型。此方法只是将Hello！文本打印到控制台，且不向调用程序返回任何内容。

### 函数可以接受零个或多个输入参数

函数可以传递输入参数，这些参数可能帮助您完成底层操作。
在 V 中，方法名后的圆括号内需要传递输入参数。一个输入参数由其名称后跟输入参数的数据类型表示。如果有多个输入参数，则必须使用逗号分隔它们，如下所示：
```v
fn add(a int, b int) int {

    return a + b

}

res := add(2, 4)

println(res) // 输出：6
```
在上面的代码中，我们声明了一个名为`add`的函数，它返回两个整数的和。这两个整数作为输入参数提供给`add`方法。

### 函数可以返回多个值

V 允许您以多个返回值的形式提供在函数内执行的操作结果。语法使用`return`关键字，其后的值用逗号分隔。此外，在输入参数之后的方法签名中指定返回类型是必要的。多个返回类型需要用圆括号括起来，其中只有数据类型按其返回的顺序用逗号分隔：
```v
fn greet_and_message_length(name string) (string, int) {

    mut greeting := 'Hello, ' + name + '!'

    return greeting, greeting.len

}

i, j := greet_and_message_length('Navule')

println(i)

println(j)
```
在上面的代码中，`greet_and_message_length`函数返回不同数据类型的多个值。它接受字符串数据类型的名称输入参数。然后，它执行字符串连接，最后返回字符串数据类型的问候语及其长度，该长度是int数据类型。这遵循`(string，int)`方法签名中提到的序列顺序。

调用者通过逗号分隔的变量接收值，如下所示：
```v
i, j := greet_and_message_length('Navule')
```
这是输出结果：
```
Hello, Navule!

14
```
### 忽略函数返回值

如果您只对一个或几个函数返回值感兴趣，则可以使用下划线来忽略相应值的初始化。

例如，以下代码显示调用者仅有兴趣捕获问候信息，而不关心消息长度，因此在返回消息长度的位置上使用`_`跳过将其初始化：
```v
i，_:= greet_and_message_length('Navule')
```
此忽略返回值的技术也可以应用于返回单个值的方法。

### 函数可以调用其他可访问的函数

函数可以调用其可以访问的其他函数。访问包括同一模块的函数或导入模块的公共函数。V 中的模块允许您将相关功能组合在一起，因此帮助您模块化代码。关于模块的更多内容将在第9章模块中学习。

在以下示例中，`greet`函数被`welcome`函数调用，并添加了欢迎消息的结果：
```v
fn greet(p string) string {

    return 'Hello, $p!'

}

fn welcome(p string) string {

    msg := 'Welcome to the Mall!'

    mut g := greet(p)

    g = g + ' $msg'

    return g

}

res := welcome('Visitor')

println(res)
```
这是输出结果：
```
Hello, Visitor! Welcome to the Mall!
```
### 函数只允许使用数组、接口、映射、指针和结构体作为可变参数

有时，您需要修改变量，例如基本类型、数组或结构体。您已经了解到这样一个变量的修改程序可以将其作为可重用代码块移动到函数中。通常，您会创建一个接受要修改的变量作为参数的函数。在函数内部，您将它分配给新的可变变量，然后执行更新。最后，您返回已更新的变量，并标记函数的方法签名为正在更新的变量的返回类型。
为了演示变量修改的情况，让我们了解以下用例。假设您想通过指定的数字递增整数数组的所有元素；通常，您编写一个函数来为新函数接受数组和递增因子作为两个参数。让我们称之为`increment_array_items`。由于该函数还必须返回数组，因此我们需要在方法签名中指定`[]int`，如下所示：

```v
fn increment_array_items(arr []int, inc int) []int {

    mut tmp := arr.clone()

    for mut i in tmp {

        i += inc

    }

    return tmp

}

a := [5, 6]

res := increment_array_items(a, 100)

println('a: $a')

println('res: $res')
```
从上面的代码中，我们可以观察到，我们正在将`arr`输入参数克隆到`tmp`可变变量中，然后递增`tmp`数组的元素值。最后，我们返回`tmp`数组。

在这种方法中，我们最终拥有不同的数组，这可以从输出中看出：

```
a: [5, 6]

res: [105, 106]
```
`increment_array_items`函数可以修改为接受可变参数，现在我们将研究如何实现它。V 允许您通过将它们作为可变参数传递给函数而无需指定任何返回类型来通过传递可变参数更新变量。在修改`increment_array_items`以接受可变参数之前，有一定的规则需要记住，以便使用接受可变参数的函数。它们如下所示：

### 只允许对数组、接口、映射、指针和结构体类型使用可变参数。

传递给接受可变参数的函数的参数也必须声明为可变的。

调用带有可变参数的函数需要您在函数调用期间指定`mut`关键字。也就是说，当将值发送到函数的可变参数的位置时，必须指定`mut`关键字。

让我们修改`increment_array_items`以反映更新作为可变函数参数传递给它的变量的功能：

```v
fn increment_array_items(mut arr []int, inc int) {

    for mut i in arr {

        i += inc

    }

}

mut a := [5, 6]

increment_array_items(mut a, 100) // 必须指定关键字mut

// 在将值发送到函数的可变参数的位置时指定mut

println('a: ${a}')
```
如你所见，更新后的increment_array_items函数不必在其方法签名中指定任何返回类型。此外，请注意，arr参数标记为mut关键字。

以下是输出结果：

```
a: [105, 106]
```
脚本模式中的函数声明应该在所有脚本语句之前

如果您正在V中编写脚本或尝试在V文件中尝试各种快速而肮脏的程序行为结果，则需要在声明变量之前定义所有函数。

将以下V脚本复制并放置到名为`script_functions.vsh`的文件中：

```v
#!/usr/local/bin/v run

cnt := 2

for i in 0 .. cnt {

    log('iteration $i')

}

fn log(msg string) {

    println(msg)

}
```
如果您在Windows上，请运行以下命令以执行V脚本：

```
v run script_functions.vsh
```

如果您在任何Unix平台上，则需要在命令行终端中运行`./script_funtioncs.vsh`。但是，在运行.vsh脚本之前，您需要将其标记为可执行文件。要将其标记为可执行文件，请运行以下命令以执行V脚本：

```
chmod u+x: ./script_functions.vsh
```
这是输出结果：
```
error: function declarations in script mode should be before all script statements
```
请注意，终端将打印错误。为了使vsh脚本起作用，请更新它，以便函数出现在脚本文件中的其他表达式或变量声明之前，如下所示：

```v
#!/usr/local/bin/v run

fn log(msg string) {

    println(msg)

}

cnt := 2

for i in 0 .. cnt {

    log('iteration $i')

}
```
这是输出结果：

```
iteration 0

iteration 1
```
我们可以观察到，当日志函数移到vsh脚本的顶部以前声明任何其他表达式或变量时，脚本成功执行。
### 函数不允许访问模块变量或全局变量

正如前面所提到的，V中的函数默认为纯函数。这意味着函数只能处理传递给它们的参数并返回处理后的输出，函数不能访问在函数体外定义的变量。

然而，V使我们能够声明全局变量。在实现低级应用程序(如编程操作系统内核或系统驱动程序)时，您可能需要全局访问变量。在这种情况下，您可以将变量声明为全局变量，并使用`-enable-globals`参数运行V程序。

现在，我们将介绍如何声明全局变量并使用`-enable-globals`标志运行V编程。我们要处理的代码将以以下目录层次结构组织：

        E:
        │   main.v
        │
        └───mymod
                mymod.v

从一个空目录开始，让我们创建一个名为main.v的文件，其中包含以下代码：
```v
// file: main.v

module main

import mymod

fn main() {

    mymod.msg := 'global variable demo'

    println(mymod.msg)

}
```
在上面的代码中，我们导入了一个名为`mymod`的模块，我们将进一步开发该模块。在`main`方法中，我们正在将值设置为我们将在`mymod`中定义的字符串数据类型的名为`msg`的变量，作为全局变量。

在V中创建全局变量的语法如下：
```v
__global(

<variable_name> <data_type>

)
```
现在，让我们创建一个名为`mymod`的模块，并添加一些代码。为此，我们将创建一个名为`mymod`的目录，并在该目录中使用以下代码创建一个名为`mymod.v`的文件：
```v
// file: mymod/mymod.v

module mymod

__global (

    msg string

)
```
在`mymod.v`文件中，我们定义了一个字符串数据类型的`msg`全局变量。从命令行终端，导航到主目录，在那里我们有`main.v`。要运行`main.v`，您需要将`-enable-globals`作为参数提供给V，如下所示：
```
v -enable-globals run main.v
```
你将看到以下输出：
```
global variable demo
```
### 函数不允许默认或可选的参数

V不允许您声明函数并将其参数设置为默认值。这表明V不支持可选参数。但是，有趣的是，在V中，可以使用默认值赋值给其字段的结构体来定义结构体。为了实现这个功能，您可以通过创建一个接受结构体作为参数的函数来解决这个限制。我们将在第8章"结构体"中学习更多关于结构体的知识。另外，在第8章"结构体"中，我们将观察到允许您将带有默认值赋值给其字段的结构体作为参数传递给函数的代码。在那里，我们将学习关于结构体作为函数尾部字面量参数的知识。

### 函数可以有可选的返回类型

函数也可以声明可选的返回类型。这是通过在函数签名中指定的返回类型的前缀添加?符号来实现的，如下例所示。对于具有可选返回类型的函数，除实际返回类型之外的可选类型将为`none`。具有可选返回类型的函数的调用者必须指定或`{}`块，如下所示：
```v
module main

fn is_teen(age int) ?string {

    if age < 0 {

        return none

    } else if age >= 13 && age <= 19 {

        return 'teenager'

    } else {

        return 'not teenager'

    }

}

fn main() {

    x := is_teen(-3) or { 'invalid age provided' }

    println(x)

}
```
在上面的例子中，`is_teen`是一个可选返回字符串的函数，表示输入年龄是否为青少年。由于年龄不能为负值，所以在这种情况下该方法不返回任何内容，这由`return none`语句表示。此外，请注意，具有可选返回类型的函数的调用者是main方法，我们正在指定`or {}`块，这由`or { 'invalid age provided' }`语句表示。

这是输出结果：
```
invalid age provided
```
你可以使用可选返回类型的函数来指定错误而不是`none`，例如以下示例：

```v
module main

fn is_teen(age int) ?string {

    if age < 0 {

        return error('invalid age provided')

    } else if age >= 13 && age <= 19 {

        return 'teenager'

    } else {

        return 'not teenager'

    }

}

fn main() {

    x := is_teen(-3) or { err.msg }

    println(x)

}
```
运行结果如下：
```
invalid age provided
```

这段代码演示了对可选返回类型的函数使用`error`。这表明你可以用接受字符串类型消息的`error()`函数返回给调用者一个字符串，代替原本的`return none` ，如此一来调用者便可以使用内置的变量`err`来访问错误信息。

`or`块必须返回非可选返回类型匹配的类型。在我们的示例中，`or`块与返回类型为`?string`的`is_teen`函数一起使用，因此`enclosed`在`or`块中的功能应返回一个字符串。或者，你也可以写成`or { panic(err) }` 或者 `or { exit(1) }` `。exit` 函数接受一个整数类型的输入参数。通常情况下，`exit(0)`表示程序运行顺利，而exit函数的任何其他输入参数(如`exit(1)`)都表示程序执行失败。

默认情况下，V的所有函数都是私有的，只能在它们定义的默认范围内访问。但是，通过`pub`关键字将其标记为公共访问，可以公开将函数暴露给其他模块。

例如，考虑以下文件结构的演示V应用程序：

        E:\v_demo
        │   public_function_demo1.v
        │   public_function_demo2.v
        │   public_function_demo3.v
        │
        └───mod1
                mod1.v

请注意，演示V项目有两个v文件，调用`mod1`模块中定义的各种函数。

下面看一下`mod1.v`文件：
```v
// file: mod1/mod1.v

module mod1

fn greet1() string {

    return 'Hello from greet1'

}

pub fn greet2() string {

    return 'Hello from greet2'

}

pub fn greet_and_wish() string {

    wish := 'Have a nice day!'

    return greet1() + ', ' + wish

}
```
`mod1.v`文件定义了三个函数，分别为`greet1`、`greet2`和`greet_and_wish`。在这三个函数中，只有`greet2`和`greet_and_wish`被标记为`pub`访问修饰符。 `pub`访问修饰符使函数可在父模块中使用，而根据`greet1`函数的定义，默认情况下它是私有的。

接下来看一下public_function_demo1.v文件：
```v
// file: public_function_demo1.v

import mod1

g := mod1.greet1()

println(g)
```
在`public_function_demo1.v`文件中，我们导入了名为mod1的模块。接下来，我们尝试将私有函数`greet1`返回的值存储在一个变量中。

现在我们执行以下命令运行`public_function_demo1.v`代码：
```
v run public_function_demo1.v
```
执行完以上命令后，你将看到以下错误信息：
```
error: function `mod1.greet1` is private
```
然后，看一下`public_function_demo2.v`文件的内容：
```v
// file: public_function_demo2.v

import mod1

g := mod1.greet2()

println(g)
```
在`public_function_demo2.v`文件中，我们导入了名为`mod1`的模块，并调用了作为`mod1`公共函数暴露出来的`greet2`函数。接下来，我们尝试将`greet2`函数返回的值存储在一个变量中，然后将该值打印到控制台。
现在，让我们运行以下命令来执行`public_function_demo2.v`文件中的代码：
```
v run public_function_demo2.v
```
运行上述命令后，你将看到以下输出：
```
Hello from greet2!
```
此外，让我们再看一下`public_function_demo3.v`文件的内容：
```v
// file: public_function_demo3.v

import mod1

g := mod1.greet_and_wish()

println(g)
```
在`public_function_demo3.v`文件中，我们导入了名为`mod1`的模块，并调用了作为`mod1`公共函数暴露出来的`greet_and_wish`函数。接下来，我们尝试将`greet_and_wish`函数返回的值存储在一个变量中，然后将该值打印到控制台。

在执行该文件之前，让我们再看一下`greet_and_wish`函数的定义：
```v
// file: mod1/mod1.v

/*full code of this file omitted for brevity*/

pub fn greet_and_wish() string {

    wish := 'Have a nice day!'

    return greet1() + ', ' + wish

}
```
由于`greet_and_wish`函数被标记为`pub`访问修饰符，因此它可以被其父模块访问。此外，请注意它在其操作列表中调用了另一个私有函数`greet1`。由于`greet1`和`greet_and_wish`属于同一模块，因此`greet_and_wish`可以作为它执行的一部分调用`greet1`。`greet_and_wish`方法的调用者将能够成功访问该方法并使用结果，而不会出现任何错误。

现在，让我们运行以下命令来执行`public_function_demo3.v`文件中的代码：
```
v run public_function_demo3.v
```
运行上述命令后，你将看到以下输出：
```
Hello from greet1, Have a nice day!
```
默认情况下，V中声明的所有函数都是私有的，只能在它们定义的默认范围内访问。

### 函数允许你使用`defer`块延迟执行流程

V允许你在`defer {}`块中封装延迟执行流程。使用`defer`关键字创建`defer`块，并在其后紧跟花括号包裹功能。不同类型的函数执行方式不同。例如，如果函数返回特定类型的值，则在评估返回语句之后执行`defer`块。另外，如果函数没有返回值，则在流程从定义它的函数离开之前执行`defer`块。

打个比方，以下是`void_func_defer`函数的定义：
```v
module main

fn void_func_defer() {

    println('Hello')

    defer {

        println('Hi from defer block')

        }

    println('How are you?')

    // the defer block will be executed when the

// execution control reaches here

}

fn main() {

    void_func_defer()

}
```
以下是输出结果：
```
Hello

How are you?

Hi from defer block
```
`void_func_defer`函数没有返回类型，根据打印语句，我们可以观察到在打印出`Hello`和`How are you?`之后，延迟块执行了其内部的语句。

### 函数可以用作数组或映射的元素

V允许你定义一个包含多个具有相同签名的函数的数组或映射。让我们重用我们在`Higher-order functions that return other functions`中定义的`adder`、`subtractor`和`multiplier`函数。由于这三个函数具有相同的签名，所以我们可以将这三个函数的数组定义如下：
```v
funcs := [adder, subtractor, multiplier]
```
当将函数添加为数组元素时，无需指定参数或返回类型。参数将在我们访问`funcs`数组元素时传递，如以下代码中所示。现在，我们可以遍历这个数组，该数组执行与这些函数底层的数学运算相关的操作，如以下示例：
```v
i, j := 2, 5

for f in funcs {

    res := f(i, j)

    println(res)

}
```
以下是输出结果：
```
7

-3

10
```
在上面的代码中，对于名为`funcs`的数组变量的每个元素进行迭代时，`f`迭代变量代表三个函数中的一个。在每次迭代中，我们将`i`和`j`整数变量传递给`f`迭代变量，该变量代表我们之前定义的函数。

上述输出的问题在于它不直观。我们无法确定对i和j整数变量执行了什么操作。因此，让我们通过定义一个包含这些函数的映射来使其更加直观。我们将定义一个映射，使键表示函数所做操作的一个单词描述，而值则是实际的函数本身：
```v
d := map{

    'sum':        adder

    'difference': subtractor

    'product':    multiplier

}
```
在将函数作为元素添加到映射中时，不必指定参数或返回类型。

我们已声明一个`d`映射变量，其中键-值对表示一个单词描述，性别名作为键，函数名称作为值：
```v
for key, val in d {

    res := val(i, j)

    println('$key of $i and $j: $res')

}
```
以下是演示如何将函数用作数组和映射元素的完整源代码：

```v
module main

fn adder(i int, j int) int {

    return i + j

}

fn subtractor(i int, j int) int {

    return i - j

}

fn multiplier(i int, j int) int {

    return i * j

}

fn main() {

    i, j := 2, 5

    println('Functions as elements of an Array')

    funcs := [adder, subtractor, multiplier]

    for f in funcs {

        res := f(i, j)

        println(res)

    }

    println('Functions as elements of Map')

    d := map{

        'sum':        adder

        'difference': subtractor

        'product':    multiplier

    }

    for key, val in d {

        res := val(i, j)

        println('$key of $i and $j: $res')

    }

}
```

下面是输出内容：
```
Functions as elements of an Array

7

-3

10

Functions as elements of Map

sum of 2 and 5: 7

difference of 2 and 5: -3

product of 2 and 5: 10
```

这里，函数可以很容易地作为数组或映射的元素构造。但是将它们定义为映射元素会更具可读性和可理解性。此外，从上述输出可以清楚地看出，在迭代映射时每次执行的操作与数组不同，更加直观。

## 总结
我们已经成功理解了V中的函数概念。简要总结一下，我们首先详细介绍了编写函数的需要。然后，我们查看了基本函数的语法，并理解了与定义函数相关的各种术语。接下来，我们学习了如何编写不同类型的函数，如基本函数、匿名函数和高阶函数，并提供了详细的代码示例。

在本章的后面部分，我们发现了各种特性，例如如何定义并使用接受参数的函数、返回值的函数和函数范围等。我们还看到了如何在具有`.vsh`扩展名的V脚本文件中编写函数。

此外，我们还介绍了高级功能，例如如何创建函数和可变参数以及在定义这些函数时需要注意的各种事项。我们学习了如何使用`defer`块推迟函数执行流程。

通过深入了解V中的函数，现在是时候进入下一章了，我们将在其中了解V中的`structs`。
