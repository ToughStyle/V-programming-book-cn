# 第9章 模块

模块化编程是将相关的功能逻辑分组成模块并进行编程的概念。这种方法使您能够将相关功能封装到模块中，并允许您导入那些模块中可用的所需功能。V提供了模块化编程的概念，允许您创建和导入复合功能或逻辑的模块。为了帮助您了解如何使用模块，在本章中，我们将介绍以下主题:

- 介绍模块

- 使用模块

首先，您将学习定义和导入模块的基本语法。在后面的部分中，您将探索如何在V中创建一个简单的项目，然后学习如何创建和导入定义在我们的简单项目中的模块。接下来，您将学习如何创建多个文件并在模块中使用它们，同时了解访问范围的概念。您还将学习最佳实践，包括初始化器函数的好处和为模块定义初始化器函数的条件。本章还涵盖了在创建模块时出现循环导入问题的解决方法。除此之外，您还将探讨成员作用域和跨模块访问常量、结构体和嵌入式结构体等成员的可访问性。通过本章的学习，您将熟悉模块的概念以及如何在编写程序时利用它们。

### 技术要求

本章的完整源代码可在 https://github.com/ToughStyle/V-programming-book-cn/codes/Chapter09 下载。

## 介绍模块

在V中，模块允许您将相关块或功能逻辑逻辑地分组。模块化的概念为您提供了结构化代码库的能力，并使代码看起来井然有序、易于识别。让我们通过查看定义和导入模块的语法开始学习V中的模块化方法。我们还将学习如何创建具有公共函数的模块并从模块外部访问它们的方式。

### 定义模块的语法

V允许您使用 module 关键字后跟模块名称来定义一个模块。以下代码指定了如何定义一个模块的语法:
```
module MY_MODULE
```
在上述语法中，MY_MODULE 可以是任何名称，表示模块内存在的功能。模块命名约定与变量相似，详见第三章「变量、常量和代码注释」的变量命名约定部分。
### 导入模块的语法

V允许您使用 `import` 关键字导入一个模块，以下代码展示了导入模块的语法：
```
import MY_MODULE
```
上面的语法展示了如何消费名为 `MY_MODULE` 的模块。我们可以看到，关键字 `import` 后必须指定模块名称。接下来，我们来看一下访问已导入模块成员的语法。

### 访问模块成员的语法

一旦您导入了任何模块，它就必须被消费，这意味着我们必须调用至少一个在模块中使用 `pub` 关键字标记的公共成员，例如函数、结构体、常量或枚举。以下是演示如何消费已导入模块的公共成员(例如函数或结构体)的语法：
```
MY_MODULE.PUBLIC_MEMBERS_OF_MY_MODULE
```
通过指定已导入模块的名称后跟一个.(点)，然后是模块内使用 `pub` 关键字标记为公共成员的成员，可以访问已导入模块的公共成员。

如果导入的模块在代码中没有被使用，当我们运行程序时，V编译器会警告我们：
```
warning: module 'MY_MODULE' is imported but never used。
```
现在，我们了解了模块的基础知识，在下一节中，我们将看看在使用模块时需要注意的事项。

## 使用模块

模块化编程是在V中编写遗留项目的重要组成部分。建议您学习如何使用模块并检查如何高效地创建和组织跨模块的应用程序功能。在本节中，您将学习定义和使用模块的各种原则：

- 目录名称必须与模块名称匹配。
- 在代码中必须消费已导入的模块。同一个模块中多个V文件必须定义相同的模块。
- 可以从模块内任意位置访问模块的公共和私有成员。
- 只有模块的公共成员可以在模块外部访问。
- 不允许出现循环导入。
- 定义初始化函数以执行一次性的模块级别初始化功能。

本章中显示的所有V代码都将以注释形式开始，指示文件名以及相对于我们正在工作的项目的目录的路径。这不适用于展示V代码片段的常见命令和片段：
```
// file: hello/file_name.v
```
例如，如果您在代码块开头找到上面示例中展示的注释，则表示该代码属于名为 `file_name.v` 的文件，该文件位于名为 `hello` 的目录下。

还请注意，当提到从终端运行项目时，需要使用命令 `v run .` 在项目的根目录中运行项目。

让我们通过实例了解使用模块时需要遵循的每个原则。

### 创建一个简单的V项目

在开始之前，我们将创建一个名为 `modulebasics` 的演示V项目。然后，我们将通过示例在该项目中实现所有前述原则，并理解它们。以下是创建演示项目的步骤：

1. 从终端进入任意目录，并运行以下命令：
```
v new modulebasics
```
您将提示输入项目描述、版本和许可证信息，具体如下：
```
Input your project description: Understanding Modules in V

Input your project version: (0.0.0)

Input your project license: (MIT)

Initialising ...

Complete!
```
2. 提供描述、版本和许可证信息，或通过按 `Enter` 键忽略，您会看到提示显示项目创建状态为完成。

3. 现在，从命令提示符中，将当前目录设置为刚刚创建的新项目，执行以下命令：
```
cd modulebasics
```
完成上述步骤后，您将具有一个名为 `modulebasics` 的新V项目，其中包含三个文件，位于一个名为 `modulebasics` 的目录中。这三个文件分别是：

a)`.gitignore`，用于Git，并保存需要在将我们的项目推送到基于Git的源控件(如GitHub、GitLab或BitBucket)时忽略的文件、文件扩展名和目录的列表。

b)`modulebasics.v`，是我们刚刚创建的应用程序的入口点。`v new`命令还将样板代码添加到`modulebasics.v`文件中，如下所示：
```
// file: modulebasics.v

module main

fn main() {

    println('Hello World!')

}
```
在这里，`modulebasics.v`是我们项目的入口点，并且由模块`main`文件中存在的`main`模块定义所标识。它还由`fn main()`的存在所标识，指示主入口点函数。`v.mod`文件具有模块信息，将公开诸如名称、描述、版本、许可证和依赖项等详细信息，提供给导入此模块的其他项目。

现在，我们将继续在现有项目中创建一个模块。

### 创建模块

我们将在`modulebasics`项目中创建一个模块。创建模块的第一原则是，目录名称必须与模块名称匹配。如果未能找到相似的目录和模块名称，则会导致错误，错误消息为`builder error: bad module definition`。让我们通过运行以下命令在`Terminal`中创建一个名为`mod1`的模块：
```
mkdir mod1
```
上述命令将创建名为`mod1`的目录，这将是模块的名称。在mod1目录中创建一个名为`file1.v`的文件，并包含以下代码：
```
// file: mod1/file1.v

module mod1

pub fn hello(){

    println('Hello from mod1!')

}
```
在上面的代码中，我们可以观察到，在`file1.v`文件中由`module mod1`语句标识模块定义。这表示模块名称与目录名称相同，即`mod1`。此外，在`file1.v`文件中，我们创建了一个名为`hello`的公共函数。使用`pub`关键字标记`hello`函数是公开的，可在模块内外部访问，这使得`hello`函数对导入`mod1`模块的代码可用。

在这个阶段，我们项目的目录结构将如下所示：

        E:\MODULEBASICS
        |   .gitignore
        |   modulebasics.v
        |   v.mod
        |
        \---mod1
                file1.v

现在，我们将继续导入此模块，这将在下一部分中演示。

### 导入模块

由于我们已经成功创建了名为`mod1`的模块，因此我们将继续从`modulebasics.v`文件中的`main`模块导入此模块。更新`modulebasics.v`文件中的代码以导入`mod1`模块，如下所示：
```
// file: modulebasics.v

module main

import mod1

fn main() {

        println('Hello World!')

}
```
请注意，`modulebasics.v`现在导入`mod1`模块，这由`import mod1`语句标识。现在，让我们从终端提供以下命令运行该项目：
```
v run .
```
运行上述命令将显示以下输出和警告：
```
.\modulebasics.v:3:8: warning: module 'mod1' is imported but never used
    1 | // file: modulebasics.v
    2 | module main
    3 | import mod1
      |        ~~~~
    4 |
    5 | fn main() {
Hello World!
```
我们可以观察到最后一行是程序的输出，打印了`main`函数的`"Hello World!"`消息。从开头开始的其余输出是一条警告，详细说明`module mod1`被导入但从未使用过。因此，在下一部分中，我们将看看如何访问导入模块的成员。
### 访问模块的成员

正如我们已经学习到的，第二个原则是导入的模块必须在代码中使用，因此让我们继续使用`mod1`模块的公共成员。

我们已经在`mod1`模块中定义了一个名为`hello`的公共函数。可以通过导入的`mod1`模块和`mod1.hello()`语法来访问`mod1`模块的公共`hello`函数。现在，`modulebasics.v`文件中的代码将更改如下：
```
// file: modulebasics.v

module main

import mod1

fn main() {

    mod1.hello()

    println('Hello World!')

}
```
`modulebasics.v`文件导入`mod1`模块，并使用`mod1.hello()`语句使用公共`hello`函数。现在，有了这些更改，让我们使用以下命令从命令行终端运行该项目：
```
v run .
```
上述命令的输出将显示如下：
```
Hello from mod1!

Hello World!
```
`mod1.hello()`语句被执行，程序在控制台打印出`Hello from mod1!`的输出，这是包含在`mod1`模块的`hello`函数中的功能。输出还打印`Hello World`到控制台，这是根据`modulebasics.v`文件中main函数中提到的执行顺序。

### 在模块中使用多个文件

到目前为止，我们只在`mod1`模块中有一个单独的文件。现在，我们将了解如何在单个模块中使用多个文件的详细信息。

如果模块中有多个文件，则所有文件都必须定义相同的模块定义。无法为模块中的所有文件定义相同的模块将导致错误，该错误与`builder error: bad module definition`相似。

让我们创建另一个名为`file2.v`的文件，其位于`mod1`模块中，并包含以下代码：
```
// file: mod1/file2.v

fn hello2() {

    println('Hello 2 from mod1!')

}
```
此时，我们项目的目录结构将显示如下：
```
E:\MODULEBASICS
| .gitignore
| modulebasics.v
| v.mod
|
\---mod1
file1.v
file2.v
```
在这里，file2.v文件有一个名为hello2的私有函数。hello2函数只是将消息打印到标准输出。有了这些更改，请使用以下命令运行该项目：
```
v run .
```
以上命令的输出将显示如下错误：
```
.\modulebasics.v:4:1: builder error: bad module definition: .\modulebasics.v imports module "mod1
2 | module main
3 |
4 | import mod1
  | ~~~~~~~~~~~
5 |
6 | fn main() {
```
这是因为`file2.v`中的代码没有定义`module mod1`。默认情况下，V假定模块将是没有模块定义的文件的主要模块。我们已经定义了`main`模块，这可以通过`modulebasics.v`文件中的`module main`语句标识。

当我们运行项目时，V编译器会遇到`file2.v`文件，该文件没有任何模块定义。编译器将其视为主模块，因此，它会抛出一个带有消息的错误，该消息说
```
builder error: bad module definition: .v imports module  "mod1" but E:12.v is defined as a module.
```
根据第三个原则，在模块中的多个V文件必须定义相同的模块，我们可以通过在`file2.v`中定义模块定义为`module mod1`来消除此错误，这类似于`file1.v`。

现在，更新后的`file2.v`文件将显示如下：
```
// file: mod1/file2.v

module mod1

fn hello2() {

    println('Hello 2 from mod1!')

}
```
在这里，`file2.v`还具有与`file1.v`中标识的相同模块定义语句`module mod1`。

现在，如果我们在终端上使用`v run .`命令运行该项目，则输出将显示如下内容：
```
Hello from mod1!

Hello World!
```
我们可以观察到，输出打印了`hello`和`main`函数的`println`语句。这是因为我们当前尚未开始使用刚刚定义的新`hello2`函数。此外，请注意，`hello2`函数未标记为公共函数，只能从`mod1`模块内部访问。

在下一节中，我们将探讨如何从`mod1`模块内部访问`hello2`函数。我们将详细探讨模块成员范围内和外部的成员作用域。
### 模块中的成员范围

模块成员的默认作用域是`private`。这些私有成员可以从任何文件内部在模块内部访问。具体来说，在模块中定义的函数、结构、常量或枚举等成员可在整个模块中访问。只有使用`pub`关键字标记为公共的成员可在模块外部访问。

到目前为止，我们的`modulebasics`项目具有名为`mod1`的模块，其中包含两个文件：`file1.v`和`file2.v`。该项目的树形结构如下所示：
```
E:\MODULEBASICS
|   .gitignore
|   modulebasics.v
|   v.mod
|
\---mod1
        file1.v
        file2.v
```
让我们尝试从`main`模块访问`hello2`函数，代码如下：
```
// file: modulebasics.v

module main

import mod1

fn main() {

    mod1.hello()

    mod1.hello2()

}
```
在这里，`hello2`未被标记为`public`，从`main`模块访问它会抛出异常：
```
error: function mod1.hello2 is private
```
根据模块使用的第四个原则，模块的私有和公共成员都可以从模块内的任何地方访问。因此，让我们尝试通过将`mod1`模块的公共`hello`函数更新为调用`hello2`私有函数来理解此原则：
```
// file: mod1/file1.v

module mod1

pub fn hello() {

    println('Hello from mod1!')

    // hello2 is not a public but accessible within mod1

    hello2()

}
```
按照模块使用的第五个原则，只有模块的公共成员可以在模块外部访问。因此，让我们更新`modulebasics.v`文件，使其调用公共`hello`函数，代码如下所示：
```
module main

import mod1

fn main() {

    mod1.hello()

}
```
以下是输出结果：
```
Hello from mod1!

Hello 2 from mod1!
```
`hello2`函数虽然未被标记为`public`，但在`mod1`模块中可访问。因此，在公共的`hello`函数中调用了`hello2`函数，而公共的`hello`函数通过使用`pub`关键字标记为`public`，已经可以在`main`模块中访问。

### 循环引用的影响

有些情况下，程序员可能会遇到两个模块彼此使用功能的情况。在这种情况下，这些模块的导入可能具有循环性质。例如，假设您在`modulebasics`项目中有两个名为`m1`和`m2`的模块，其中包含以下代码：
```
// file: m1/file1.v

module m1

import m2

pub const greet_from_m1 = 'Greetings from m1'

pub fn hello() {

        println(m2.greet_from_m2)

}
```
在这里，前面的代码属于`m1`模块，其中具有使用`pub`关键字标记的`greet_from_m1`常量。它还具有打印来自导入的`m2`模块的常量的公共`hello`函数。

现在，让我们看一下具有以下代码的`m2`模块：
```
// file: m2/file1.v

module m2

import m1

pub const greet_from_m2 = 'Greetings from m2'

pub fn hello() {

    println(m1.greet_from_m1)

}
```
上述代码属于`m2`模块，其中具有使用`pub`关键字标记的`greet_from_m2`常量。它还具有打印来自导入的`m1`模块的常量的公共`hello`函数。因此，`m1`模块引用了`m2`模块，而`m2`模块引用了`m1`模块，这会引入循环或循环引用。

现在，如果我们更新`modulebasics.v`文件，它将类似于以下内容：
```
// file: modulebasics.v

module main

import m1

import m2

fn main() {

        m1.hello()

        m2.hello()

}
```
`modulebasics.v`文件导入了`m1`和`m2`两个模块，并在主函数中调用了这些模块中的`hello`函数。现在，让我们使用`v run .`命令运行`modulebasics.v`并查看输出结果。根据使用模块的第六个原则，即不允许循环导入，您将观察到输出结果包含以下错误信息：
```
builder error: error: import cycle detected between the following modules:

 * main -> m1 -> m2 -> m1

 * m1 -> m2 -> m1

 * m2 -> m1 -> m2
```
从输出结果中，我们可以看到执行控制首先进入main模块，并具有以下执行控制流程：

- `m1 -> m2 -> m1`：执行控制进入`m1`模块的`hello()`函数。它然后从`m2`访问`greet_from_m2`常量。随后，控制流返回到`m1`并打印该常量。

- `m2 -> m1 -> m2`：执行控制进入m2模块的`hello()`函数。它然后从`m1`访问`greet_from_m1`常量。随后，控制流返回到m2并打印该常量。

这两个流都是循环的。两个`m1`和`m2`模块正在尝试导入和访问彼此的成员，这导致了一个错误：`"builder error: error: import cycle detected between the following modules"`。

### 模块的初始化函数

使用模块的最后一个原则要求您定义初始化函数以执行一次性的模块级别初始化功能。在V中，可以定义一个名为`init`的函数，在导入该函数所在的模块时会自动执行。如果有的话，模块的`init`函数作为某些功能的初始化器，例如建立数据库连接或初始化C库或模块特定的设置。要定义初始化器函数，必须满足以下条件：

1. 只能在模块内部一次定义`init`函数。

2. 不能将`init`函数标记为`public`。

3. `init`函数不能接受任何输入参数。

4. `init`函数不能有返回类型。

虽然可以定义一个至少具有一个输入参数的公共函数`init`，但它不会像模块的初始化器函数那样起作用。

让我们使用示例探索`init()`函数。在我们的演示项目`modulebasics`中，让我们修改`mod1`模块，将`file1.v`更新为以下代码：
```
// file: mod1/file1.v

module mod1

pub fn hello() {

    println('Hello from mod1!')

}

fn init() {

    println('Initializing mod1')

}
```
在这里，我们在`mod1`的`file1.v`文件中添加了新的私有的`init`函数。在这种情况下，我们只是向控制台打印一个消息，显示 `Initializing mod1`。现在，我们将更新`modulebasics.v`文件的代码，指示以下主模块：
```
// file: modulebasics.v

module main

import mod1

fn main() {

    mod1.hello()

}
```
在主模块中，我们使用`pub`关键字调用标记为`public`的`hello`函数。私有的`init`函数在`mod1`中定义，如我们所了解的，在`mod1`模块的任何其他函数之前，这将是将执行器函数执行的初始化器函数。为了查看这一点，我们将运行`v run .`命令来运行项目。上述代码的输出结果将如下所示：
```
Initializing mod1

Hello from mod1!
```
请注意，在执行其他任何函数之前，输出结果会从`init`函数中打印出消息 `Initializing mod1`。之后，它会从`hello`函数打印 `Hello from mod1!` 消息。

### 访问模块的常量

从另一个模块访问模块的常量是一种简单的方法。只要用`pub`关键字标记为`public`，就可以访问模块的常量。

在我们的示例`modulebasics`项目中，假设具有以下代码的`mod1`模块中的`file1.v`：
```
// file : mod1/file1.v

module mod1

pub const greet_msg = 'Greeting from mod1!'
```
在上面的代码中，我们在mod1模块中定义了一个名为`greet_msg`的常量。另外，我们使用pub关键字将该常量标记为`public`。我们将学习如何从主模块访问此常量。更改主模块中的代码，使其如下所示：
```
// file: modulebasics.v

module main

import mod1

fn main() {

    println(mod1.greet_msg)

}
```
上述代码导入mod1模块。在作为执行控制点的main函数中，我们从mod1模块访问greet_msg常量并将其打印到控制台上：
```
Greeting from mod1!
```
上述输出显示打印自mod1模块的greet_msg常量分配的值。

### 访问模块的结构体和嵌入式结构体

在本节中，我们将详细探讨如何从主模块访问mod1模块的结构体字段和嵌入式结构体字段。我们将更新modulebasics项目，以便在mod1模块中定义两个结构体NoteTimeInfo和Note，如下所示：
```
module mod1

import time

// NoteTimeInfo是用于存储Note时间信息的结构体

pub struct NoteTimeInfo{

pub:

        created time.Time = time.now()

pub mut:

        due     time.Time = time.now().add_days(1)

}

// Note是一个具有嵌入式结构体NoteTimeInfo和其他字段的结构体

pub struct Note {

        NoteTimeInfo // 嵌入式结构体

pub:

        id      int

pub mut:

        message string [required]

        status  bool

}
```
使用pub关键字将NoteTimeInfo和Note两个结构体都标记为public。对于NoteTimeInfo结构体，将created结构体字段标记为public，将due标记为public mut，使用pub mut关键字。NoteTimeInfo结构体的created和due字段都有初始化默认值。

类似地，对于Note结构体，将id结构体字段标记为public，将message和status字段标记为public和mutable。

现在，让我们更新主模块以访问结构体字段和嵌入式结构体字段如下：
```
// file: modulebasics.v

module main

import mod1

fn main(){

        n := mod1.Note {

                id: 1

                message: 'Accessing structs of module demo'

        }

        println('Accessing struct field value Note id:

                $n.id')

        println('Accessing embedded struct field value

                NoteTimeInfo: $n.NoteTimeInfo')

}
```
上述代码显示了我们如何在主模块中访问mod1模块的Note和NoteTimeInfo两个结构体，并初始化Note结构体。然后，我们打印它的id字段和嵌入式结构体字段NoteTimeInfo。上述代码显示以下输出结果：
```
Accessing struct field value Note id: 1

Accessing embedded struct field value NoteTimeInfo: mod1.NoteTimeInfo{

    created: 2021-05-30 01:45:36

    due: 2021-05-31 01:45:36

}
```
上述输出显示了访问Note结构体的id结构体字段和嵌入式NoteTimeInfo结构体字段的结果。该结构体使用pub访问修饰符在mod1模块中定义。

## 总结

在本章中，我们清楚地理解了V中模块化编程的概念。我们学习了如何创建和导入模块以及有助于我们处理它们的各种概念。通过代码示例，我们了解到，使用模块可以使属于项目的代码看起来更易于访问和组织。我们还学习了使用模块的各种方法，包括访问模块成员，例如结构体，函数和常量。

此外，我们涉及如何在模块内部和外部定义的成员的范围，初始化器函数以及创建循环导入的影响。最后，我们通过使用代码示例了解了如何访问模块的结构体和嵌入式结构体。

了解了模块之后，在下一章中，我们将继续探索V中的并发。
