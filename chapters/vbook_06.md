# 第6章 条件语句和迭代语句

条件编程可帮助您以所需的方式控制程序的执行。这意味着您可以编写满足各种用例集的软件，借助条件分支实现。因此，在每个条件分支中，您可以编写特定用例的逻辑。在本章中，我们将详细介绍V编程语言中的条件语句。我们将研究如何使用`if`、`if-else`和链接`else-if`的条件块以及标签支持的`goto`语句。我们还将深入探讨`match`块，该块用于条件代码分支。

在本章后面的部分，我们将查看各种类型的迭代。使用迭代代码块编写程序可以让您访问集合中的每个实体。使用此方法，您可以专注于集合中每个元素，并因此可以为每个迭代上的实体应用特殊处理。针对每个迭代中的元素，您可以结合条件分支执行特殊处理；这将编程地反映任何用例的功能。在本章中，我们将使用`for`循环在映射和数组上执行迭代，然后我们将研究使用`for`循环编写迭代语句的不同方法。

在本章中，我们将涵盖以下主题：

- 条件块
  - `if`/`else`块
  - `match`块 
- 迭代语句

通过本章的学习，您将能够在V中编写条件和迭代语句。

### 技术要求

对于那些希望在本章节中跟随代码示例的读者，建议按照第二章"安装 V 编程"中提到的方式安装 V。要编写 V 代码，可以使用命令行终端和编辑器，例如 Nano 或 Vim。否则，也可以选择使用 Notepad ++、Visual Studio Code 或任何其他编程编辑器。

本章节的完整源代码可在 [https://github.com/ToughStyle/V-programming-book-cn/tree/main/codes/Chapter06](https://github.com/ToughStyle/V-programming-book-cn/tree/main/codes/Chapter06) 找到。

## 条件`if`/`else`块

通常，根据操作结果执行一些特定例程是必要的。在任何编程语言中，我们都可以通过条件语句(例如 `if` 语句或 `match` 块)来实现此目的。在本节中，我们将深入了解 V 提供的每个条件语块。我们将从 `if` 语句开始讨论。

### `if` 语句

`if` 和 `if-else` 语块等条件语句允许您基于语句中评估的条件的结果来做出决策。评估可能涉及逻辑或关系运算符的结果。V 中的 `if` 语句允许您创建一个特殊的代码块，该代码块仅在满足 `if` 语句中提到的条件时执行。以下是编写 `if` 块的语法：
```v
if CONDITION {

        // CONDITION 评估为 true

}
```
上述语法演示了如何编写 `if` 语句。它以 `if` 关键字开始，后跟计算为 `true` 或 `false` 的布尔结果的条件。然后，语法遵循括在大括号 `{` 和 `}` 中的特殊代码块，只有在条件的结果评估为 `true` 时才会执行。

这是告诉我们如何编写 `if` 语句的基本语法。但是，`if` 语句有各种不同的版本：

- `if-else` 语句
- `else-if` 链式语句
- 带有 `goto` 语句的 `if` 语句

让我们更深入地了解每个内容。

### `if-else` 语句

`if-else` 语句允许您决定是否执行某个特定代码块，前提是 `if` 语句中的条件评估为 `true` ，并且处理该代码块的情况是条件评估为 `false` 时的情况。在 `if-else` 语块中，根据条件的评估结果，`if` 语块或 `else` 语块中的代码将执行其中之一。

以下代码显示了使用 `if` 和 `else` 关键字编写 `if-else` 语块的语法：
```v
if CONDITION {

        // CONDITION 评估为 true

} else {

        // CONDITION 评估为 false

}
```
### `else-if` 链式语句

如前所述，在 `if-else` 的情况下，将执行至少一个代码块。在某些场景中，您可能希望对 `if` 语句的第一个条件进行自定义检查，并在其评估为 `false` 且不直接允许控制流执行 `else` 块中的代码时执行正在执行的代码。您可以为此类用例编写 `else if` 语句。

这里显示了编写 `else if` 的语法，以及 `if` 和 `else：`
```v
if CONDITION_1 {

        // CONDITION_1 评估为 true

} else if CONDITION_2 {

        // CONDITION_2 评估为 true

} else {

        // 没有条件评估为 true

}
```
与 `else` 块不同，`else if` 块需要指定条件。但是，拥有 `else` 块是可选的。您可以链接许多这样的 `else if` 块，这些块在 `if` 中与每个 `else if` 块中指定不同的条件。但是，一旦控制进入满足条件的任何块，将执行特定于该块的代码，并完全退出 `if`、`else-if` 和 `else` 链。
让我们看一下以下代码：

```v
module main

fn breakfast_menu(day string) {
    if day == 'Monday' {
        println('Bread，Jam，Half boiled Egg')
    } else if day == 'Tuesday' {
        println('Bread，Jam，Juice')
    } else if day == 'Wednesday' {
        println('Milk，Bread，Fruit Bowl')
    } else if day == 'Thursday' {
        println('Bread，Jam，Juice')
    } else if day == 'Friday' {
        println('Cereals，Bread，Jam，Half boiled Egg')
    } else if day == 'Saturday' {
        println('Milk，Bread，Fruit Bowl')
    } else if day == 'Sunday' {
        println('Cereals，Bread，Jam，Half boiled Egg')
    } else {
        println('无效输入')
    }
}

fn main() {
    breakfast_menu('星期六')
}
```
前面的代码的输出如下所示：
```
Milk，Bread，Fruit Bowl
```
前面的代码演示了 `else-if` 语句的链接，它进行各种天数的检查，然后打印与早餐菜单函数的输入参数匹配的菜单。 `else-if` 条件没有限制，但是代码看起来杂乱无章。在本章后续部分中，我们将学习如何针对这种情况使用 `match`，编写可重复使用的代码，并使用 `match` 块使代码看起来整洁。

### 带有 `goto` 语句的 `if` 语句

V 允许您标记代码并使用 `goto` 关键字引用执行控制。 `goto` 语句需要指定标签，该标签指示当执行流遇到 `goto` 语句时导航到标签的控件。标签使用普通文本定义，后跟 : 冒号符号。

### 注意

`goto` 语句必须包装在 `unsafe` 块中。这是因为 `goto` 允许程序执行流绕过变量初始化或返回访问已被释放的内存的代码。由于 `goto` 语句需要一个不安全块，因此应该避免使用它以防止违反内存安全的风险。

以下代码显示了包装在 `unsafe` 块中的 `goto` 语句的语法：
```v
sample_label:
    println('this will be called when goto is invoked')

unsafe {
    goto sample_label
}
```
现在，让我们看一下以下演示使用 `goto` 语句的代码：

```v
module main

import os

fn main() {
    improper_input_age:
    println('无效输入。请提供大于0的值。')

    next_person:
    inp := os.input('请输入您的年龄：')

    if inp != 'stop' {
        age := inp.int()
        if age >= 13 {
            println('您可以观看这部电影')
        } else if age > 0 && age < 13 {

            println('需要家长指导才能观看此电影')
        } else if age <= 0 {
            unsafe {
                goto improper_input_age
            }
        }

        unsafe {
            goto next_person
        }
    }
}

```

上面的代码检查从标准输入控制台提供的年龄变量。然后，它检查年龄输入是否符合某些标准。如果年龄是负数，则移动到逻辑上被命名为 `improper_input_age` 的代码，并执行打印 `Invalid input. Please provide value greater than 0` 的消息的代码。对于除 stop 外的所有其他输入，它都会检查是否有资格观看具有家长指导或没有的电影。然后，使用另一个带标签的 `goto` 语句 `next_person` 接受下一个队列中的人的年龄输入。此外，我们可以观察到，`goto` 代码块包含在 `unsafe` 块中。当输入提供为 `stop` 时，程序从执行中退出。

现在，我们已经详细了解了 `if` 语句及其如何与代码示例一起使用的细节，接下来我们将继续学习如何在条件编程中使用 `match` 块。
## 条件`match`块

`match`语句块可以执行对代码块内指定的条件进行模式匹配的操作。在大多数情况下，它也被用作开关 `case` 语句。因为V语言没有 `switch case` 语句块，所以可以借助 `match` 语句块来实现类似的功能。

首先，我们会学习到 `match` 语句块的基本语法，然后，在随后的部分中，我们将探讨 `match` 语句块的各种用例，包括以下内容：

- `match` 语句块作为开关 `case`

- `match` 语句块的级联条件

- 使用带枚举类型 `else` 条件的 `match`

- `match` 作为模式匹配

以下是使用 `match` 关键字的 `match` 语句的语法：
```v
match VALUE {
    CONDITION_1 { /*CONDITION_1 匹配了。*/ }
    CONDITION_2 { /*CONDITION_2 匹配了。*/ }
    ..
    CONDITION_N { /*CONDITION_N 匹配了。*/ }
    else { /*没有任何一种模式匹配。执行其他程序。*/ }
}
```
在上述语法中，`match` 关键字期望 `VALUE` 与其内部定义的所有条件具有相似的数据类型。此外，必须声明我们在前述语法中添加的所有模式(代替条件的位置)都属于相似的数据类型。否则，程序将抛出一个错误，提示无法与条件匹配。除非针对枚举类型声明了 `match`，否则必须声明一个 `else` 条件，作为在没有条件得到满意的匹配时的终点。

总之，`match` 语句块期望以下内容：

- 条件应该与传递给 `match` 语句块的值具有相似的类型。

- `match` 语句块的所有条件分支的返回类型必须是相似的类型。

- 在未指定所有可能条件的情况下，`match` 语句块必须是全面的。可以通过 `else` 块实现。

- `match` 语句块的匹配 `case` 只能被处理一次。这也适用于范围，V语言会在编译时自动检查模式匹配的重复范围或重叠范围，并抛出一个错误，提示 `match case` 被处理了不止一次。

现在我们已经了解了 `match` 语句块的基本语法，我们将进行更进一步的学习，并检查您可以使用 `match` 语句块的不同方式。

### `match` 语句块作为开关 `case`

在 V 中，`match` 语句块可以用作传统的 `switch case`。我们可以通过使用 `match` 语句块来改写在 `Chaining else-if` 部分中看到的代码示例来了解这一点。让我们来看一下下面的代码：
```v
module main

fn breakfast_menu(day string) {
    match day {
        'Monday' {
            println('面包，果酱，半煮蛋')
        }
        'Tuesday' {
            println('面包，果酱，果汁')
        }
        'Wednesday' {
            println('牛奶，面包，水果碗')
        }
        'Thursday' {
            println('面包，果酱，果汁')
        }
        'Friday' {
            println('麦片，面包，果酱，半煮蛋')
        }
        'Saturday' {
            println('牛奶，面包，水果碗')
        }
        'Sunday' {
            println('麦片，面包，果酱，半煮蛋')
        }
        else {
            println('无效输入')
        }
    }
}

fn main() {
    breakfast_menu('Sunday')
}
```
下面是上述代码的输出：
```
麦片，面包，果酱，半煮蛋
```
上述代码将提供给 `breakfast_menu` 函数的 `day` 输入与 `match` 语句中定义为该日的条件匹配，并打印与在 `match` 语句中定义的条件相匹配的早餐菜单。当星期天被赋值为`day`的值时，将执行具有星期天条件定义的 `match` 语句。

### `match` 语句块的级联条件

在 `match` 语句块的多个条件执行相同操作的情况下，我们可以使用逗号来级联 `match` 语句块的条件。在前面的例子中，星期五和星期日的早餐菜单相同；同样，周二和周四的早餐菜单也相同。
所以，我们可以将星期五和星期日的逻辑与星期二和星期四的逻辑合并在一个匹配块的条件中，如下所示:

```v
module main

fn breakfast_menu(day string) string {
    return match day {
        'Monday' {
            'Bread, Jam, Half boiled Egg'
        }
        'Tuesday', 'Thursday', 'Friday', 'Sunday' {
            'Cereals, Bread, Jam, Half boiled Egg'
        }
        'Wednesday', 'Saturday' {
            'Milk, Bread, Fruit Bowl'
        }
        else {
            'invalid input'
        }
    }
}

fn main() {
    friday_menu := breakfast_menu('Friday')
    println(friday_menu)

    sunday_menu := breakfast_menu('Sunday')
    println(sunday_menu)

    tuesday_menu := breakfast_menu('Tuesday')
    println(tuesday_menu)

    thursday_menu := breakfast_menu('Thursday')
    println(thursday_menu)

}
```

以上是我们之前示例代码的输出结果：

```
Cereals, Bread, Jam, Half boiled Egg

Cereals, Bread, Jam, Half boiled Egg

Bread, Jam, Juice

Bread, Jam, Juice
```

在使用逗号级联匹配块条件后，`breakfast_menu` 函数仍然是相同的，但是冗余的代码更少，更易于阅读。此外，它已重构为返回 `string` 类型的值。现在，它返回与匹配块条件之一匹配的菜单与输入参数 `day`。

### 使用枚举类型来匹配

V 中的匹配块也接受枚举类型来匹配其封闭条件。枚举类型的字段用以表示带前缀 `. ` 点符号的字段名。由于枚举类型有一组定义的字段，当所有枚举字段都在匹配条件中提到时，使用 `else` 是被禁止的：

```v
module main

enum Day {
    sunday
    monday
    tuesday
    wednesday
    thursday
    friday
    saturday
}

fn breakfast_menu(day Day) string {
    return match day {
        .monday {
            'Bread, Jam, Half boiled Egg'
        }
        .tuesday, .thursday {
            'Bread, Jam, Juice'
        }
        .wednesday {
            'Milk, Bread, Fruit Bowl'
        }
        .friday, .sunday {
            'Cereals, Bread, Jam, Half boiled Egg'
        }
        .saturday {
            'Milk, Bread, Fruit Bowl'
        }
    }
}

fn main() {
    friday_menu := breakfast_menu(Day.friday)
    println(friday_menu)

    sunday_menu := breakfast_menu(Day.sunday)
    println(sunday_menu)

    tuesday_menu := breakfast_menu(Day.tuesday)
    println(tuesday_menu)

    thursday_menu := breakfast_menu(Day.thursday)
    println(thursday_menu)
}
```

以上是我们代码示例的输出结果：

```
Cereals, Bread, Jam, Half boiled Egg

Cereals, Bread, Jam, Half boiled Egg

Bread, Jam, Juice

Bread, Jam, Juice
```

为了演示将枚举字段用作匹配块条件的用法，我们声明了一个名为 `Day` 的枚举类型，其字段表示一周的日子。`breakfast_menu` 函数有一个匹配块，它以 `Day` 枚举型的输入参数返回菜单作为字符串类型。此外，匹配块的条件是前缀为点符号 `.` 的 `Day` 枚举类型字段。由于我们在条件中指定了 `Day` 枚举型的所有字段，因此在所有情况下对应的匹配块已覆盖了所有可能的匹配。因此，在这种情况下，`else` 块的使用是被禁止的。
### 使用带有 `else` 条件的 `match` 与枚举类型

如果我们在 `match` 块的条件列表中未提及所有枚举字段，则必须为该 `match` 块指定 `else` 条件。

以下代码演示了在 `match` 块中与枚举类型一起使用 `else` 的用法：

```v
module main

enum Day {
    sunday
    monday
    tuesday
    wednesday
    thursday
    friday
    saturday
}

fn weekend_breakfast_menu(day Day) string {
    return match day {
        .sunday {
            'Cereals, Bread, Jam, Half boiled Egg'
        }
        .saturday {
            'Milk, Bread, Fruit Bowl'
        }
        else {
            'Sorry, we are closed on weekdays!'
        }
    }
}

fn main() {
    sunday_menu := weekend_breakfast_menu(Day.sunday)
    println(sunday_menu)

    tuesday_menu := weekend_breakfast_menu(Day.tuesday)
    println(tuesday_menu)

}
```

以下是输出结果：

```
Cereals, Bread, Jam, Half boiled Egg

Sorry, we are closed on weekdays!
```

在上述代码中，我们替换了方法，并只想在周末(星期六和星期日)展示菜单。由于匹配块没有使用所有字段，而只将字段作为 `.saturday` 和 `.sunday` 条件进行了指定，因此我们必须在匹配块中使用 `else`。如果我们不在这里写 `else`，则会导致错误，并显示一个消息，指出：

```
error: match must be exhaustive (add match branches for: .monday, .tuesday, .wednesday, .thursday).
```

### 使用 `match` 进行模式匹配

到目前为止，我们已经了解了将 `match` 块作为传统的 `switch case` 使用的情况。但是，`match` 的实际强大之处在于可以用于模式匹配。

以下代码演示了使用 `match` 进行模式匹配的用法：

```v
module main

fn main() {
    age := 18

    res := match age {
        0...18 { 'Person with $age classified as a Child' }
        
        19...120 { 'Person with $age classified as an Adult' }
        
        else { '$age is must be in the range 0 to 120' }
    }

    println(res)
}
```

以下是输出结果：

```
Person with 18 classified as a Child
```

上述代码演示了使用范围进行 `match` 块的用法。请注意，范围使用 `...`(即三个点)以将范围定义为 `match` 块内的一个条件。将范围定义为 `match` 块的条件分支包括范围内的最后一个元素。代码检查年龄的值，如果它在 0 到 18(包含 18)的范围内，则被归类为儿童。另一种情况则检查年龄在 19 到 120(包括 120)之间的成年人。如果该数字未满足从 0 到 18 和从 19 到 120 的两个范围的任何一个条件分支，则会显示一条消息，提供 0 到 120 的年龄以正确分类一个人。

在本节中，我们详细了解了 `if` 语句和 `match` 块的用法，并理解了它们的语法以及如何使用这些条件块来编写代码示例。现在，是时候进入本章的下一个部分，了解在 V 中使用迭代语句的内容。

## 迭代语句

在软件开发中，您可能需要处理或处理 `collection`(例如数组或映射)中的每个元素。有时，您会想要访问集合的每个元素并更改其值或仅读取它以进行进一步处理。在 V 中，您可以编写使用 `for` 循环的迭代语句来实现此目的。

`for` 循环旨在遍历集合的元素。该集合通常是具有某种数据类型的元素的数组，或者可能是以键值对形式保存数据的映射。
在这一部分，我们将先介绍如何在V语言中编写最基本的`for`循环语法，然后我们将探讨各种操作`for`循环的方法，包括以下内容：

- 对map进行遍历的`for`循环
- 对数组进行遍历的`for`循环
- 在数组中没有索引的`for`循环
- 传统的C语言风格的`for`循环
- 反向`for`循环
- 对范围进行遍历的`for`循环
- 裸`for`循环或无限`for`循环
- 在`for`循环中使用`break`语句
- 在`for`循环中使用`continue`语句
- 使用标签与`continue`和`break`语句

首先，让我们看一下以下语法，展示如何使用`for`关键字和`in`运算符编写`循环：
```v
for INDEX_VAR, VALUE_VAR in COLLECTION {
    // 访问每个元素的索引和值
}
```
从上述语法可以看出，`for`循环以`for`关键字开始，然后声明了两个变量：`INDEX_VAR和VALUE_VAR`。然后是`in`运算符，它希望在其之后指定包含集合的变量。

`INDEX_VAR`变量是整数类型，而`VALUE_VAR`变量是集合中所持有值的类型。

请注意，`COLLECTION`可以是数组或`Map`。当编写`Map`、数组和数字范围上的`for`循环时，需要牢记以下几点：

- `for`循环对`Map`进行遍历会生成每个被迭代的项的`key`和`value`。
- 当使用`for`循环处理数组时，可以声明一个可选索引变量。
- 在范围上使用`for`循环仅允许声明保存被迭代范围内变量值的变量。

现在，我们已经审查了编写`for`循环的基本语法，接下来我们将查看如何使用`for`循环在集合(如数组和`Map`)上进行操作。之后，我们将看一下使用`for`循环实现逻辑的不同方法。因此，让我们先学习如何在`Map`上使用`for`循环。

### 对`Map`进行遍历的`for`循环

以下语法向您展示如何使用`for`循环在`Map`的键值对上进行迭代：
```v
for KEY_VAR, VALUE_VAR in MAP_VAR {
    // 在此处访问键和值
}
```
在上述语法中，`KEY_VAR`和`VALUE_VAR`指示每个迭代器的`Map`的键和值。

让我们来看看下面的代码：
```v
module main

fn main() {
    lottery := map{
        'First':       1000
        'Second':      700
        'Consolation': 200
    }

    for k, v in lottery {
        println('$k prize lottery amount: $v')
    }
}
```
以下是我们代码示例的输出：
```
First prize lottery amount: 1000 USD

Second prize lottery amount: 700 USD

Consolation prize lottery amount: 200 USD
```
上述代码迭代了`Map`的键值对。在`for`语句中，两个`k`和`v`变量分别代表该`Map`变量`lottery`的元素的键和值。

有时候，我们只对键或值感兴趣。在这种情况下，我们可以在使用`for`循环迭代`Map`时忽略返回的键或值。为了忽略键或值，可以在`for`语句的相应位置使用"_"进行替换。

使用"_"防止将键或值分配给一个变量并常常被认为是一种内存高效的方法。

以下代码使用`for`循环忽略了`basket Map`的键：
```v
module main

fn main() {
    basket := map{
        'apples':  10
        'bananas': 12
    }

    mut total := 0

    for _, v in basket {
        total += v
    }

    println('Total number of fruits: $total')
}
```
下面是输出结果：
```
Total number of fruits: 22
```
与忽略`basket Map`的键相同，我们也可以使用`_`忽略值，将其置于表示`Maps`数对的`v`变量的位置。
### 数组上的for循环

在数组类型上执行迭代将为您提供可选索引以及正在迭代的每个项目的值。以下是一个代码示例：

```v
module main

fn main() {
    fruits := ['apple', 'banana', 'coconut']

    for idx, ele in fruits {
        println('idx: $idx \t fruit: $ele')
    }
}
```

以下是输出结果：

```
idx: 0   fruit: apple

idx: 1   fruit: banana

idx: 2   fruit: coconut
```

在上述代码中，`for`循环迭代名为`fruits`的带有字符串数据类型值的数组。代码块使用`for`循环迭代命名为`fruits`的数组，定义了两个变量：`idx`和`ele`。`idx`变量始终是整数数据类型，并表示每个元素的索引，从0开始，并且对于每次迭代递增1。`ele`变量保存该迭代的元素的值。在本例中，该值是一个字符串类型，其中包含来自水果数组的水果名称。在索引和元素之后，我们指定`in`运算符，然后指定我们正在迭代的集合。在本例中，集合是水果数组。

### 没有索引的数组循环

数组上的`for`循环只能在其语法中定义一个变量，该变量保存元素的值。因此，我们有选项来定义指示元素的索引的变量。

以下代码演示了未声明索引变量的数组上的`for`循环每次迭代：

```v
module main

fn main() {
    col := [1, 2, 3, 4, 5, 6, 7]

    for val in col {
        if val % 2 == 0 {
            println('$val is Even')
        } else {
            println('$val is Odd')
        }
    }
}
```

以下是输出结果：

```
1 is Odd
2 is Even
3 is Odd
4 is Even
5 is Odd
6 is Even
```

在上面的示例中，注意`for val in col`语句。这仅定义`val`变量，其中包含该迭代的变量的值。

### 传统的C样式`for`循环

有时，我们需要使用`for`循环跳过n个元素。您可以通过编写与C＃或C等大多数编程语言中通常使用的`for`循环相同的方式来实现此目的。V允许您以与C编程相同的方式编写`for`循环。以下代码显示了V中传统的C-style `for`循环：

```v
module main

fn main() {
    sample := [3, 4, 23, 12, 4, 1, 45, 12, 42, 17, 92, 38]

    for i := 0; i < sample.len; i += 3 {
        println(sample[i])
    }
}
```

以下是输出结果：

```
3
12
45
17
```

### 反向`for`循环

要从数组的最后一个元素开始迭代数组的元素，您可以编写与C或C＃中相同的传统`for`循环语法。

以下代码从最后一个元素开始打印数组的元素：

```v
module main

fn main() {
    subjects := ['zoology', 'chemistry', 'physics', 'algebra']

    for i := subjects.len - 1; i >= 0; i-- {
        println(subjects[i])
    }
}
```

### 在范围上的`for`循环

与在数组上进行迭代不同，您无法在使用`for`循环迭代范围时声明索引变量。您可以使用`for`循环迭代范围的元素，如下所示：

```v
module main

fn main() {
    for val in 0 .. 4 {
        println(val)
    }
}
```

以下是输出结果：

```
0
1
2
3
```

在`for`循环中，我们声明了一个从0开始的4个数字序列的范围。在`for`循环中，我们打印了每个元素的值，这些元素在迭代过程中出现在一系列范围中。

### 裸`for`循环或无限`for`循环

此外，我们可以编写没有条件的`for`循环，就像传统的`for`循环一样。当我们只想查看数组中每个元素的值时，我们也可以在不使用`in`运算符的情况下编写它。这些类型的`for`循环通常称为无限`for`循环或无休止的`for`循环。

当在控制台程序中使用无限`for`循环时，通常用于接受用户输入并根据其确定操作。通常，这些类型的`for`循环需要外部干预才能停止其无限循环的执行。这些外部因素，例如强制停止程序的执行或与条件匹配的用户输入，在代码内部处理。
这段代码展示了使用简单 `for` 语句的语法，不带任何条件进行循环，如下所示：
```v
module main

fn main() {
    mut count := 1

    for {
        println('Hi $count times')
        count += 1
    }
}
```
你可能已经注意到了，在上一个示例中，由于没有任何限定条件，`for` 循环会不断打印，直到我们强行停止程序的执行。前面的代码没有任何限制条件。我们可以使用 break 关键字为这种情况引入条件。

### 在 `for` 循环中使用 `break`

V 允许您在遇到 `break` 关键字时突然退出 `for` 循环。

`break` 关键字停止 `for` 循环执行的迭代并退出它。通常， `break` 语句用于在满足我们定义的某些条件之后终止 `for` 循环。

考虑以下代码，它描述了在 `for` 循环内使用 `break` 的用法：
```v
module main

import os

fn main() {
    mut count := 0

    input := os.input('Enter number of times to Greet:')

    limit := input.int()

    for {
        if count >= limit {
                break
        }
        println('Hi')
        count += 1
    }

    println('Greeted Hi $count times')

}
```
前面代码的输出如下所示：
```
Enter number of times to Greet:3
Hi
Hi
Hi
Greeted Hi 3 times
```
现在，我们正在限制次数，这由用户输入进行控制，然后在满足 `count>=limit` 条件时，我们使用 `break` 语句退出 `for` 循环。

### 在 `for` 循环中使用 `continue`

有时，程序需要决定是继续执行代码块还是跳过并开始处理下一个元素的迭代。在这种情况下，可以使用 `continue` 关键字，遇到它时将停止当前迭代的执行并继续进行 `for` 循环中的下一项元素。

以下代码描述了在 `for` 循环中使用 `continue` 的用法：
```v
module main

fn main() {
    for i in 0 .. 10 {

        if i % 2 == 0 { // skips printing number
        // that is a multiple of 2
            continue
        }

        println(i)
    }
}
```
下面是输出：
```
1
3
5
7
9
```
在前面的代码中，`for` 循环正在打印奇数。因此，如果 i 迭代变量所持有的值是偶数，则使用 `continue` 语句尽早退出迭代。请注意，当 `i % 2 == 0` 语句的结果等于 `true` 时，将执行 `continue` 语句。

### 在 `label` 中使用 `continue` 和 `break` 语句

就像 `goto` 语句引用标签一样，您还可以使用标签写 `continue` 和 `break` 语句。

让我们考虑以下示例：
```v
module main

import os

fn main() {
    input := os.input('Enter the number of
        multiplication tables to print:')

    limit := input.int()

    if limit <= 0 {
        return
    }

    first_loop: for i := 1; i <= 10; i++ {
        println('Printing multiplication table for $i')
        for j := 1; j <= 10; j++ {
                mul := i * j
                println('$i * $j = $mul')
                if mul >= limit * 10 {
                    break first_loop
                }
        }
        println('*********')

    }

}
```
下面是输出：
```
Enter the number (1 to 10):2

Printing multiplication table for 1

1 * 1 = 1
1 * 2 = 2
1 * 3 = 3
1 * 4 = 4
1 * 5 = 5
1 * 6 = 6
1 * 7 = 7
1 * 8 = 8
1 * 9 = 9
1 * 10 = 10

*********

Printing multiplication table for 2

2 * 1 = 2
2 * 2 = 4
2 * 3 = 6
2 * 4 = 8
2 * 5 = 10
2 * 6 = 12
2 * 7 = 14
2 * 8 = 16
2 * 9 = 18
2 * 10 = 20
```
上述代码打印了1到10之间的乘法表，但是我们可以使用程序的输入来限制要打印的表的数量。当 `mul >= limit * 10` 语句结果为 `true` 时，根据所评估的条件，我们在满足输入标准后结束执行。该语句会在遇到 `break first_loop` 语句时中断 `for` 循环的执行，即带有标签 `first_loop` 的 `for` 循环。因此，带标签的 `break` 或 `continue` 语句可用于控制嵌套的 `for` 循环的执行。

## 总结

在本章中，我们学习了如何编写条件块和迭代语句。我们了解了如何使用 `if` 条件及其其他变体，例如 `if`、`if-else` 和多个 `else-if` 语句的连锁用法。然后，我们学习了 `goto` 语句如何在处理 `if` 代码块时帮助您导航到任何标记的代码块。在条件块部分中，除了 `if` 语句，我们还了解了 `match` 块的用法。我们使用 `match` 块重构了实现连锁 `else-if` 的示例代码；此后，代码看起来更整洁易读。此外，我们还了解了将 `match` 块用作传统 `switch case` 的用法，并学习了如何使用模式匹配实现它。

本章还深入探讨了迭代语句。我们学习了如何编写基本 `for` 循环的语法，然后发现了如何使用 `for` 循环处理数组和映射。除此之外，我们还探讨了如何使用 `for` 循环从集合中进行反向迭代，并学习了如何在一系列值上进行迭代。我们还研究了如何使用 `continue` 关键字跳过一个迭代并使用 `break` 关键字突然退出 `for` 循环。我们还了解了使用标签使用 `continue` 和 `break` 语句的技巧，并编写了打印乘法表的代码。

通过理解条件块和迭代语句的概念，我们将继续学习软件编程的基本但最重要的构建块即函数。

