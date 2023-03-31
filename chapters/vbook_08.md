# 第8章 结构体

在编写软件应用程序时，您可能需要以保持其所有属性的结构形式来表示对象。例如，如果您正在编写一个处理笔记或待办事项列表的软件应用程序，则需要以结构体的形式表示每个笔记。假设笔记的最基本属性是笔记的文本内容、创建时间和唯一标识笔记的笔记ID，则这些属性可以被集合地放在表示笔记的结构体内。在 V 中，您可以使用 `struct` 关键字创建表示对象的蓝图。在本章中，我们将学习通常称为结构体的这些蓝图。以下是我们将在本章中学习的主题列表：

- 介绍结构体
- 更新结构体的字段
- 定义结构体字段的方法
- 在另一个结构体内添加结构体作为字段
- 将结构体作为函数的尾随字面量参数

在本章结束时，您将能够编写包含具有不同访问修饰符的结构体字段、定义具有默认值的结构体字段，并理解如何添加属于结构体的方法以及如何创建具有结构体作为输入参数的函数。

### 技术要求

本章的完整源代码可在 GitHub 上获得：

https://github.com/ToughStyle/V-programming-book-cn/codes/Chapter08

## 介绍结构体

在 V 中，结构体允许您定义复合对象。它们使您能够创建复杂的数据结构，允许其字段具有不同的数据类型。结构体的字段可以是原始数据类型、枚举类型，也可以是另一个结构体。

我们将通过观察基本语法来开始理解结构体。随后，我们将学习如何基于此语法定义结构体，使用分配给其字段的值初始化结构体，并访问这些字段。然后，我们将探讨什么是堆结构体，并用详细的代码示例进行说明。

### 定义结构体

您可以使用 `struct` 关键字后跟结构体名称来在 V 中定义结构体。V 中结构体的基本语法如下所示：
```V
struct STRUCTNAME {

    FIELDNAME1 DATATYPE

    FIELDNAME2 DATATYPE

}
```
例如，我们定义一个 `Note` 结构体以了解一个实际应用的结构体在 V 中的外观：
```V
struct Note {

    id      int

    message string

}
```
从上述代码中，我们可以看出 Note 结构体有两个字段 id 和 message，它们的数据类型分别为整数和字符串。

### 初始化结构体

我们现在将初始化结构体，如下所示：
```V
n := Note{1, 'a simple struct demo'}
```
传递给结构体的参数需要按照它们在结构体定义中出现的顺序排序。我们注意到我们使用值分配给其字段初始化 `Note` 类型的结构体。请注意，结构体字段的值是隐式指定的，按照它们在结构体定义中的顺序被赋给结构体字段。在这种情况下，`id` 将隐式赋值为 `1，message` 的值为` 'a simple struct demo'`：
```V
n := Note{

    message: 'a simple struct demo'

    id: 1

}
```
您还可以在 V 中使用字段名称后跟冒号(:)和要分配的值显式初始化结构体的字段，并适合结构体字段的数据类型。

结构体变量 `n` 的类型是 `Note`，可以通过运行以下代码进行确认：
```V
println(typeof(n).name) // Note
```
### 访问结构体的字段

您可以直接访问结构体的字段，这些字段可在结构体值对象上使用。

例如，要打印 `Note` 结构体的 `message` 的值，您可以在值对象上使用 `.field`，如下所示：
```V
n := Note{1, 'a simple struct demo'}println(n.message)
```
输出将打印 `Note` 结构体的 `message` 字段保存的值，如下所示：
```
a simple struct demo
```
### 了解堆结构

当一个结构体被初始化时，它的内存默认在堆栈上分配。通过在初始化时在结构体名称前面添加 `&` ，可以在堆上分配结构体的内存，示例如下：
```V
n1 := &Note{1, 'this note will be allocated on heap'}
```
上述代码示例演示了堆结构的初始化，通过在结构体名称前添加 `&` 来识别。访问堆结构的字段与普通结构体类似。

堆结构体变量 `n1` 的类型是 `&Note` ，可以通过运行以下代码进行确认：
```V
println(typeof(n1).name) // &Note
```
处理携带大量数据的结构体时，堆结构体特别有用。因此，选择堆结构体可以减少显式内存分配。

现在我们已经了解了结构体的基本语法、尝试了初始化并访问结构体字段，以及创建堆结构体，接下来我们将讨论不同的更新结构体字段值的方法。

## 更新结构体字段

在处理结构体时，有时可能需要更新结构体中的特定字段，以更改字段持有的现有或默认值。 V 在更新结构体字段方面具有特定的规格。在本节中，我们将看到更新结构体字段的不同方法，并了解所有需要的先决条件，以使对结构体字段的值进行更改。

所有结构体的字段默认都是不可变的，只能初始化一次。要更改结构体的字段值，需要将其指定在标有 `mut:` 的部分下方。在 `mut:` 下定义的所有字段都将成为可变字段。

我们将更改结构体 `Note` ，以使 `message` 字段可变：
```V
struct Note {

    id      int

mut:

    message string

}
```
现在，我们将声明并初始化 `Note` 结构体，如下所示：
```V
n := Note {1, 'a simple struct demo'}

println(n)
```
输出将打印 `Note` 对象到控制台，在新行中显示如下：
```V
Note{

    id: 1

    message: 'a simple struct demo'

}
```
让我们更新 `Note` 结构体的 `message` 字段，如下所示：
```V
n := Note {1, 'a simple struct demo'}

n.message = 'a simple struct updated'
```
您会惊讶地看到有一个错误，内容如下：
```
error: `n` is immutable, declare it with `mut` to make it mutable
```
让我们将变量声明更新为可变的性质。同时，我们将尝试更新 `message` 字段的值，如下所示：
```V
mut n := Note { 1, 'a simple struct demo' }

n.message = 'a simple struct updated'
```
要更新结构体的字段，必须同时使字段和初始化结构体的变量都是可变的。

此时，完整的代码如下所示：
```V
module main

struct Note {

    id int

mut:

    message string

}

fn main() {

    mut n := Note{1, 'a simple struct demo'}

    println('before update')

    println(n)

    n.message = 'a simple struct updated'

    println('after update')

    println(n)

}
```
上述代码的输出如下所示：
```V
before update

Note{

    id: 1

    message: 'a simple struct demo'

}

after update

Note{

    id: 1

    message: 'a simple struct updated'

}
```
现在，让我们看看如果尝试更新 `Note` 结构体的不可变 `id` 字段会发生什么情况：
```V
mut j := Note{1, 'a simple struct demo'}

j.id = 2
```
您将看到以下错误消息，提醒 `Note` 结构体中的 `id` 字段是不可变的：
```
error: field `id` of struct `Note` is immutable
```
在更新结构体字段的值时，需要记住以下几点：

- 声明结构体的变量必须是可变的。

- 必须将结构体名称指定到等于号(=)操作符的右侧，并按照您希望更新的结构体字段跟随使用大括号({})将值括起来。

- 字段名称必须指定为文本字符串字面量，然后才可以通过冒号(:)赋值。

- 未在更新语句中指定的字段默认为零。
未在更新语句中指定的字段默认被赋值为零，即使它们在之前的初始化中有一些值。让我们看下面这个例子：

在上面的代码中，当我们第二次更新`message`时，`id`字段被忽略了。

输出如下：
```V
Note{

    id: 1

    message: 'updating struct fields demo'

}
```
未指定的id在短结构体类型初始化期间默认为零
```V
Note{

    id: 0

    message: 'updating struct fields demo 2'

}
```
从上述输出可以观察到，`id`字段的值在第二次更新后被归零为默认整数值0，尽管在`n`变量被设置为1的`id`时它被赋值为1。值得一提的是，如果您有其他字段数据类型，如`bool`或字符串，则此类类型的零值将分别为`false`和`''`。

## 定义结构体字段的方法

当声明结构体时，您经常需要限制或控制其字段成员的行为。在本节中，我们将看到如何实现这种行为，其中包括以下内容：

- 将多个可变字段添加到结构体中

- 使用访问修饰符对结构体中的字段进行分组

- 在结构体中定义必需的字段

- 定义具有默认值的结构体字段

- 让我们在以下子章节中详细讨论每个部分

- 将多个可变字段添加到结构体中

我们可以使用`mut`关键字后跟独立一行的:来定义结构体的所有可变字段。定义结构体中可变字段的语法如下所示：
```V
struct Note {

    id      int

mut:

    message string

    status  bool

}
```
在上面的代码示例中，我们为`Note`结构体指定了一个名为`status`的新字段，其数据类型为`bool`。这里需要注意的重要事项是：所有可变类型都在`mut`关键字下使用冒号(:)符号声明。缩进是可选的，有助于提高可读性。唯一值得注意的是，用于声明可变字段的语法`mut:`必须在其自己的单独行上。

因此，很明显，在`mut:`语法之前声明的结构体的所有字段都是不可变的，在`mut:`语法之后声明的字段是可变的。

使用访问修饰符对结构体中的字段进行分组

使用关键字`pub`，用于字段的公共访问，和`mut`，用于指示可变字段，结构体允许在各种访问级别上对其字段进行过滤。您可以控制结构体字段的访问方式。

可应用于结构体字段的各种访问控制级别如下表所示：

| 访问修饰符 | 本模块内 | 外部模块中 |
|----------|---------|----------|
| pub | public | public |
| mut | mutable | immutable |
| pub mut | public，mutable | public，mutable |
| _global | public，mutable | public，mutable |

表8.1-了解结构体字段的访问控制

让我们将各种访问修饰符应用到`Note`结构体的字段中，并详细讨论我们正在做什么：
```V
pub struct Note {

pub:

    id int

pub mut:

    message string

    status  bool

}
```
我们将Note结构体标记为`pub`。我们还通过在`pub`组下声明将`id`字段标记为`public`。`pub`组下的字段是公共的，在模块外是只读的。另外，`message`和`status`字段都标记有`pub`和`mut`访问修饰符。在`pub mut`组下定义结构体字段表示结构体字段在`Note`定义的模块内和外部均可访问和可变。
### 定义结构体中的必需字段

有时候，如果一个结构体中的某些字段没有值，它的存在是没有意义的，例如一个`Note`结构体没有`message`字段就变得无用了。为了防止这种情况的发生，您可以使用方括号(`[]`)将`required`关键字括起来放在字段右侧，以强制标记某些字段。这经常被称为将结构体字段注释为必需的。使用此注释标志，编译器将知道该特定字段标记为必需。以下代码示例显示了`Note`结构体中带有`[required]`注释的`message`字段。

```V
pub struct Note {

pub:

    id      int

pub mut:

    message string [required]

    status  bool

}
```

现在，让我们创建一个`Note`结构体，但实际上不会将任何值初始化到`message`字段中，如下所示：

```V
_ := Note{

    id: 1

    status: false

}
```

您会注意到，V会抛出一个错误，如下所示：

```
error: field `Note.message` must be initialized
```

由于我们已将`message`字段标记为`[required]`，因此我们必须从此时开始初始化`message`字段。完整的代码，包括初始化`message`字段，如下所示：

```V
module main

pub struct Note {

pub:

    id int

pub mut:

    message string [required]

    status  bool

}

fn main() {

    n := Note{

        id: 1

        message: 'a simple struct demo'

        status: false

    }

    println(n)

}
```

### 定义具有默认值的结构体字段

有时，需要为结构体字段定义并初始化一些默认值。这使得程序员可以防止这些字段被显式初始化，除非有必要这样做。V允许您为定义的结构体字段分配默认值。假设我们想要捕获创建注释的时间，每次我们初始化和分配`Note`结构体的其他字段值时。在这种情况下，我们可以定义一个不可变的字段来保存时间信息，并将其赋值为在结构体定义内创建`Note`时的时间。

为了说明这种用例，我们将修改`Note`结构体以适应两个新字段，即`created`和`due`。以下是创建具有分配默认值的结构体字段的语法：

```V
import time

pub struct Note {

pub:

    id      int

    created time.Time = time.now()

pub mut:

    message string    [required]

    status  bool

    due     time.Time = time.now().add_days(1)

}
```

在`Note`结构体中，名为`created`的字段是一个带有`Note`创建时间的不可变字段，并分配了默认值。另一个名为`due`的字段将设置为默认值，以便它标识出特定的`Note`将于从`Note`创建时间起的第一天到期。

现在，我们将创建一个`Note`结构体，比如要订购杂货，然后打印`Note`，如下所示：

```V
n := Note{

    id: 1

    message: 'order groceries'

}

println(n)
```

这里是输出：

```V
Note{

    id: 1

    created: 2021-03-04 02:02:33

    message: 'order groceries'

    status: false

    due: 2021-03-05 02:02:33

}
```

如`Note`结构体定义中所示，输出将显示`created`、`status`和`due`字段的默认值。

需要注意的是，尽管我们没有为`status`分配任何值，但它被赋予了`false`值。这是因为，在V中，结构体字段的值默认情况下被赋为零。在这种情况下，`status`的默认值被赋为`bool`数据类型的默认值`false`。因此，即使未初始化`status`字段的任何值，`status`字段的值也将显示为`false`。
完整的代码现在将显示如下所示：
```V
import time

pub struct Note {

pub:

    id      int

    created time.Time = time.now()

pub mut:

    message string    [required]

    status  bool

    due     time.Time = time.now().add_days(1)

}

fn main() {

    n := Note{

        id: 1

        message: 'order groceries'

    }

    println(n)

}
```
到目前为止，我们已经学习了不同的方法来定义结构体字段，例如定义可变字段、定义带有访问修饰符的字段、将字段标记为必需成员，以及了解了如何定义具有默认值的结构体字段。现在，让我们来看看如何为结构体创建方法。针对结构体的方法允许我们定义特定于结构体的行为并允许我们更新结构体字段。让我们在以下章节中学习定义方法的相关知识。

### 为结构体定义方法

V允许为结构体定义方法。方法是带有特殊接收器参数的函数，接收器参数出现在fn和方法名称之间。它们允许我们以方便的方式向结构体添加函数。方法是一种访问结构体属性以执行某些例程的函数。要为结构体定义方法，请按照以下语法：
```V
fn (r RECEIVER_TYPE) METHOD_NAME(OPTIONAL_INPUT_ARGUMENTS)RETURN_TYPE {

    METHOD BODY

}
```
在上面的语法中，r 接收器类型`RECEIVER_TYPE`表示方法所属的结构体的名称。如果您熟悉C#编程语言，那么此功能类似于C#中的扩展方法概念。在运行时，结构体的方法`METHOD_NAME`可以访问结构体字段中保存的值。因此，方法有助于评估逻辑或对结构体字段执行所需的操作。

### 注意事项

属于结构体的方法需要放置在与结构体相同的模块中。

让我们回到`Note`结构体，并创建一个检查`message`字段是否为空的方法。即使我们可以强制结构体字段为`[required]`，但是`message`可以提供空字符串的情况仍然存在。为了验证这一点，让我们为`Note`结构体创建下面的方法：
```V
pub fn (n Note) is_empty_message() bool {

    return n.message.len < 1

}
```
现在，我们可以在初始化后的`Note`结构体上调用此方法来检查`message`字段是否为空。为了演示目的，将`message`字段设置为空字符串，代码如下所示：
```V
module main

import time

pub struct Note {

pub:

    id      int

    created time.Time = time.now()

pub mut:

    message string    [required]

    status  bool

    due     time.Time = time.now().add_days(1)

}

// is_empty_message 是属于Note的方法

pub fn (n Note) is_empty_message() bool {

    return n.message.len < 1

}

fn main() {

    mut n := Note{

        id: 1

        message: ''

    }

    if n.is_empty_message() {

        println('message is empty')

    } else {

        println('message not empty')

    }

}
```
下面是输出结果：

```
message is empty
```

执行上面的代码将在控制台的标准输出中打印`'message is empty'`。

现在我们已经学会了如何为结构体编写方法，让我们继续学习如何将一个结构体添加到另一个结构体内部作为其字段。

## 在另一个结构体内添加结构体作为字段

想象一下，您需要创建一个新的结构体。在创建结构体过程中，您会发现更多的字段成为了您正在定义的结构体的一部分。可能有一些字段彼此相关，但它们可能不起到表示您正在声明的结构体的重要角色。在这种情况下，这些字段可以移动到单独的结构体中，并作为其一个字段呈现在主结构体内部。在这种情况下，V允许将一个结构体作为另一个结构体的字段添加到内部。将结构体添加到另一个结构体内部的唯一先决条件是，类型为`struct`的字段必须在结构体体的开始处声明。
为了说明，让我们将 `Note` 结构体中的 `created` 和 `due` 字段移动到另一个结构体中(假设是 `NoteTimeInfo`)如下所示，也就是将 `NoteTimeInfo` 结构体添加为 `Note` 结构体的字段：

```V
import time

// NoteTimeInfo is a struct to store time info of Note

pub struct NoteTimeInfo {

pub:

    created time.Time = time.now()

pub mut:

    due time.Time = time.now().add_days(1)

}

// Note is a struct with struct NoteTimeInfo as a field,

// along with other fields

pub struct Note {

    NoteTimeInfo // Struct as another struct field

pub:

    id int

pub mut:

    message string [required]

    status bool

}
```
`NoteTimeInfo` 结构体作为 `Note` 结构体的字段被添加进去了。另外，需要注意的是，`NoteTimeInfo` 字段是一个类型为结构体的字段，在任何其他字段之前在 `Note` 结构体的主体开始处声明。

现在，让我们初始化 `Note` 结构体并访问 `due` 属性，实际上它是 `NoteTimeInfo` 的一部分：

```V
n := Note{

    id: 1

    message: 'adding struct as struct field demo'

}

println('Due date: $n.due')
```

我们可以看到，`due` 是 `NoteTimeInfo` 的字段，但却可以使用语句 `n.due` 在 `Note` 结构体 `n` 变量的属性上进行访问。这是因为 `NoteTimeInfo` 结构体是 `Note` 的一个结构体字段。

让我们尝试使用以下代码打印整个 `Note`：

```V
println(n)
```
输出结果如下：
```
Note{
    NoteTimeInfo: NoteTimeInfo{
        created: 2021-04-21 22:55:29
        due: 2021-04-22 22:55:29
    }
    id: 1
    message: 'adding struct as struct field demo'
    status: false
}
```

### 修改另一结构体中的结构体类型的字段

要更新结构体类型的字段，有两种方法。第一种方法是隐式访问作为另一个结构体值的字段的结构体字段的字段，使用等号 (=) 符号。另一种方法是显式指定结构体字段的名称，然后指定结构体的相应字段并使用等号 (=) 符号更新值。

例如，如果我们想将现有笔记的到期日期延长两天，一种方法是直接访问 `NoteTimeInfo` 的字段而不必明确指定 `NoteTimeInfo` 字段，并更新字段，如下所示：

```V
n.due = n.due.add_days(2)
```

另一种方法是显式指定类型为结构体的字段的名称，这里是 `NoteTimeInfo`，然后指定其字段，如下所示：

```V
n.NoteTimeInfo.due = n.NoteTimeInfo.due.add_days(2)
```

下面展示了访问结构体字段时的两种方法，其中结构体是另一个结构体的结构体字段：

```V
module main

import time

// NoteTimeInfo is a struct to store time info of Note

pub struct NoteTimeInfo {

pub:

    created time.Time = time.now()

pub mut:

    due time.Time = time.now().add_days(1)

}

// Note is a struct with struct NoteTimeInfo as a field,

// along with other fields

pub struct Note {

    NoteTimeInfo

pub:

    id int

pub mut:

    message string [required]

    status  bool

}

fn main() {

    mut n := Note{

        id: 1

        message: 'adding struct as struct field demo'

    }

    println('Due date: $n.due')

    // approach 1: implicit access of struct fields of

    // fields of type struct

    n.due = n.due.add_days(2)

    println('Due date after update: $n.due')

    // approach 2: explicitly specifying the field of type

    // struct and its fields

    n.NoteTimeInfo.due = n.NoteTimeInfo.due.add_days(2)

    println('Due date updated second time: $n.due')

    println(n)

}
```

输出结果如下：

```
Due date: 2021-04-22 23:00:49

Due date after update: 2021-04-24 23:00:49

Due date updated second time: 2021-04-26 23:00:49

Note{
    NoteTimeInfo: NoteTimeInfo{
        created: 2021-04-21 23:00:49
        due: 2021-04-26 23:00:49
    }
    id: 1
    message: 'adding struct as struct field demo'
    status: false
}
```
学习了定义和更新具有结构体类型字段的结构体的方法后，现在我们将学习如何将结构体作为尾随字面量参数传递给函数。

## 将结构体作为函数的尾随字面量参数

由于 V 不支持默认函数参数或命名参数，因此可以使用尾随结构体字面量语法。在 V 中，可以定义接受结构体作为输入参数的函数。因此，我们可以将具有默认值的结构体传递给接受该结构体作为输入参数的函数。

例如，让我们创建一个购买杂货品的 `Note` 结构体的函数。该函数将接受提供为输入参数的 `Note` 结构体，并创建新的 `Note` 结构体，将短语`'Buy Groceries:'`添加到每个新便签正在创建的消息字段之前，如下所示：
```V
fn new_grocery_note(n Note) &Note {

    return &Note{

        id: n.id

        message: 'Buy Groceries: ' + n.message

    }

}
```
现在，我们可以创建一张购买杂货品的便签，如下所示：
```V
g := new_grocery_note(id: 1, message: 'Milk')

println('$g.message is due by $g.due')
```
上述代码片段演示了向接受结构体作为其输入参数的函数传递值的方法。请注意，参数是结构体的字段，以及我们要为这些字段分配的值。还请注意，多个字段名称：值对之间用逗号(,)分隔。可以选择指定结构体的名称，就像我们在先前的代码中看到的那样。或者，您可以明确提到结构体的名称，如下所示：
```V
g := new_grocery_note(Note{id: 1, message: 'Milk'})

println('$g.message is due by $g.due')
```
假设我们想将笔记的截止日期延迟一天。为此，我们可以创建一个函数，以便每次想要扩展到期日期时不必重新编写完整的字段赋值。只需通过传递现有注释来调用此可重用代码，它会通过一天扩展截止日期：
```V
fn extend_due_by_a_day(n Note) &Note {

    return &Note{

        NoteTimeInfo: NoteTimeInfo{

            due: n.due.add_days(1)

        }

        id: n.id

        message: n.message

    }

}
```
您可以在已经创建的 `Note` 结构体上调用此方法，该方法将到期日期再延长一天，或者可以直接创建一个新的 `Note`，其默认到期日期为创建时间的两天后：
```V
n := extend_due_by_a_day(g)

println('After extending due date by a day')

println('$n.message is due by $n.due')
```
上述代码演示了向函数传递结构体的另一种方法。在这里，我们直接将早先通过 `new_grocery_note` 函数获得的新变量 `g` 作为参数传递给 `extend_due_by_a_day` 函数。

将这些组合起来，我们将看到下面的程序，演示了使用尾随结构体字面量的函数工作方式：
```V
module main

import time

// NoteTimeInfo 是存储 Note 时间信息的结构体

pub struct NoteTimeInfo {

pub:

    created time.Time = time.now()

pub mut:

    due time.Time = time.now().add_days(1)

}

// Note 是一个具有 NoteTimeInfo 结构体字段的结构体，

// 另外还有其他字段

pub struct Note {

    NoteTimeInfo

pub:

    id int

pub mut:

    message string [required]

    status  bool

}

fn new_grocery_note(n Note) &Note {

    return &Note{

        id: n.id

        message: 'Buy Groceries: ' + n.message

    }

}

fn extend_due_by_a_day(n Note) &Note {

    return &Note{

        NoteTimeInfo: NoteTimeInfo{

            due: n.due.add_days(1)

        }

        id: n.id

        message: n.message

    }

}

fn main() {

    g := new_grocery_note(Note{ id: 1, message: 'Milk' })

    println('$g.message is due by $g.due')

    n := extend_due_by_a_day(g)

    println('After extending due date by a day')

    println('$n.message is due by $n.due')

}
```
在本节中，我们学习了如何定义一个接受结构体作为参数的函数。然后我们看到了传递结构体给函数的不同方法。我们看到了如何将结构体字段直接作为输入参数传递，也看到了如何将结构体变量传递给一个接受结构体作为输入参数的函数。理解这些方法有助于程序员在使用结构体时变得更加熟练并轻松地编写涉及结构体的程序。

## 总结

在本章中，我们学习了如何声明结构体。我们从定义结构体的语法开始，然后学习了初始化和访问结构体字段。然后，我们学习了如何更新已经初始化的结构体的字段。之后，我们讨论了各种定义结构体字段的方法，包括具有可变性、访问范围和默认值的字段。您还可以定义属于结构体的方法，并学习如何将一个结构体指定为另一个结构体的字段。最后，我们看到了将结构体字段和结构体变量传递给接受结构体作为输入参数的函数的各种方法，以及代码示例。

下一章将帮助您学习如何在V中编写和维护模块化代码。

