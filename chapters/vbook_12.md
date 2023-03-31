# 第12章 测试

编写测试是开发可维护软件应用程序的重要部分。测试可以确保某个作为软件一部分实现的函数在各种情况下按预期工作。通过使用测试来验证软件应用程序的正确性，可以确保将来对核心逻辑进行任何更改或扩展功能时，现有测试将帮助您确定新更改如何影响现有行为。

本章将介绍以下内容：

- V 中测试的介绍
- 理解测试套件函数
- 编写`AAA`模式的测试
- 为具有可选返回类型的函数编写测试
- 编写和运行测试的方法

通过本章，您将学会为 V 中的简单程序和具有模块的程序编写测试，了解运行测试的不同方法以及使用统计参数在 V 中运行测试的好处。

### 技术要求：

本章的完整源代码可在 https://github.com/ToughStyle/V-programming-book-cn/codes/Chapter12 上获得。

在 V 中编写测试很简单。在本节中，我们将探讨三个简单的概念，以帮助您开始编写测试。它们如下所示：

- 使用 `assert` 关键字比较实际和预期结果。
- 包含测试的文件必须以 `_test.v` 扩展名结尾。
- 每个测试都是一个函数，必须以前缀 `test_` 开始。

在下一小节中，我们将学习 `assert` 关键字的语法及其在 V 中的使用方法。

### `assert` 关键字

在 V 中，您可以使用 `assert` 关键字将要测试的函数的输出与预期输出进行比较。以下是展示如何在 V 中使用 `assert` 关键字的语法，后面跟着一个布尔表达式：
```
assert boolean_expression
```
在上面的语法中，`assert` 关键字后面跟着的是一个表达式，该表达式的输出必须始终评估为布尔结果。如果布尔结果为 `true`，则断言成功，否则将失败。我们通常使用关系运算符(如<、>、!=、==、<=或>=)或字符串数据类型上提供的`.contains()`表达式，这些表达式提供布尔结果。有关关系运算符的详细信息，请参见第 4 章原始数据类型。

`assert` 表达式也可用于普通函数中。当放置在 `assert` 关键字旁边的表达式评估为 `true` 时，程序将继续执行。如果表达式评估为 `false`，则程序将停止，并将错误报告给 `stderr`。下面是一个演示如何在普通函数中使用 `assert` 的代码：

```V
module main

fn main() {

    println('1st assert')

    msg := 'hello there!'

    assert msg.contains('hello') // true

    println('2nd assert')

    assert 'apple' == 'orange' // stops execution

    println('done')

}
```

现在，如果我们使用 `v run filename.v` 命令运行此文件，输出将如下所示：

```
1st assert

2nd assert

s.v:8: FAIL: fn main.main: assert 'apple' == 'orange'

   left value: 'apple' = apple

  right value: 'orange' = orange

V panic: Assertion failed...

v hash: ddc62ab

C:/Users/pavan/AppData/Local/Temp/v/s.1904660688325513120.tmp.c:5830: at _v_panic: Backtrace

C:/Users/pavan/AppData/Local/Temp/v/s.1904660688325513120.tmp.c:10467: by main__main

C:/Users/pavan/AppData/Local/Temp/v/s.1904660688325513120.tmp.c:10825: by wmain

00448e10 : by ???

00448f73 : by ???

7ff86bb87974 : by ???
```

在上述输出中(根据您的操作系统，您可能会看到略有不同的错误消息)，我们注意到该程序执行并打印语句，直到遇到第二个 `assert`，然后是表达式 `"apple" == "orange"`。由于该表达式的结果为 `false`，程序停止并打印错误，因此我们没有在输出中看到字符串值 `done` 的打印。

我们还可以看到，在断言失败时，`assert` 必须打印左值和右值的信息。使用左值和右值中包含的信息，您可以了解测试失败的原因。当 `assert` 使用关系运算符时，就会发生这种情况。

正如我们所见，我们使用 `v run filename.v` 命令运行具有 `main` 函数的文件。在下一节中，我们将看到如何在测试文件中撰写实际的测试，并了解如何运行测试。

### 编写简单测试

如前所述，要编写测试，文件名必须以 `_test.v` 结尾。现在，我们将创建一个名为 `demo_test.v` 的文件，并添加一个简单的测试。

从命令提示符中，导航到要创建简单测试文件的目录并运行以下命令：
```
mkdir v_test_demo

cd v_test_demo

echo '' > demo_test.v
```
虽然您可以在测试文件中包含普通函数，但是必须至少有一个测试函数在测试文件中，否则 V 编译器将抛出错误，如下所示：
```
demo_test.v:1:1: error: a _test.v file should have *at least* one `test_` function

Details: The name of a test function in V, should start with `test_`.
```
测试函数应该不带任何参数，没有返回类型。示例：
```V
fn test_xyz(){ assert 2 + 2 == 4 }
```
在我们开始编写测试之前，先比较一下整数值的关系运算符，让我们看一下测试函数的一些属性：

- 测试函数必须以 `test_` 开始。
- 测试函数不应接受任何输入参数。
- 测试函数应始终是 `void` 函数。这意味着它不能具有返回类型，或者可以标记为返回可选类型。

在了解测试函数的基本属性后，我们将在刚创建的测试文件中编写第一个测试。从您选择的编辑器中打开 `demo_test.v` 文件，并添加以下代码，使 `demo_test.v` 文件如下所示：
```V
fn test_first() {

    assert 2 != 2

}
```
从上面的代码中，我们看到 `demo_test.v` 文件有一个单一的测试函数 `test_first()`。正如我们所学，测试名称以 `test_` 开始。您可以尝试将此函数重命名，以使其不以 `test_` 开头；V 不会将该函数视为测试。

我们有一个带有 `test_first` 测试函数的测试文件，该函数只断言在指定两个值均为2时执行的关系操作符`!=` 的结果。基于表达式 `2！= 2`，我们预计我们的测试将失败。但是，我们还未学习如何运行包含在测试文件中的测试。因此，让我们继续并查看如何在下一小节中运行测试。

### 运行测试

通常，我们使用 `v run filename.v` 命令运行包含在 V 文件中的逻辑。但对于包含测试的文件，您只需使用 `v filename_test.v` 。因此，在这种情况下，我们将运行以下命令以执行 `demo_test.v` 中存在的测试：
```
v demo_test.v
```
上述命令将运行 `demo_tests.v` 中的所有测试。由于 `demo_test.v` 文件中只有一个测试，因此它只会运行 `test_first` 测试。由于断言失败且评估为 `false`，因此运行测试文件后的输出将如下所示：
```
demo_test.v:2: ✗ fn test_first

   > assert 2 != 2

      Left value: 2

     Right value: 2
```
因此，我们的测试在断言 `2！= 2` 的表达式上失败。在上述输出中，我们注意到 `stderr` 显示了 `demo_test.v:2: ✗ fn test_first` 表示测试失败。另外，使用 > 符号指出了测试未能进行断言的行。下一行还显示了表达式保留的值以及标签 `Left value` 和 `Right value`。

由于此测试失败，让我们通过将布尔表达式更改为评估为 `true` 来使该测试通过，如下所示：
```V
fn test_first() {

    assert 2 == 2

}
```
从上述代码中，我们看到紧随 `assert` 关键字的 `2 == 2` 表达式将评估为 `true`。因此，在保存对 `demo_test.v` 文件所做的更改后，我们再次运行以下命令来运行测试：
```
v demo_test.v
```

前面的命令不会将任何内容打印到标准输出。

理解了 assert 的概念以及如何命名测试文件和测试函数后，我们将继续探讨 V 中的测试套件函数。

## 理解测试套件函数

V 允许以测试套件函数的形式编写测试执行前后的例程，分别是 `testsuite_begin` 和 `testsuite_end`：

`testsuite_begin`：当您计划设置某些资源时(例如数据或环境变量)，或者为测试文件中的测试创建文件时，此函数将很有帮助。

`testsuite_end`：或者，可以使用 `testsuite_end` 函数来清除 `testsuite_begin` 引入的资源。`testsuite_end` 函数可以用于清除数据和环境变量，或删除在测试运行期间创建的任何文件。

此外，这些 `testsuite` 函数既不接受输入参数，也不指定返回类型。这些函数只在执行一次，而不是针对测试文件中存在的每个测试都执行。当您开始运行测试时，测试运行器将查找 `testsuite_begin` 的存在并首先执行此函数。同样，如果存在 `testsuite_end` 函数，则会在最后执行该函数。

### 演示使用 `testsuite` 函数

在本节中，我们将以一些示例代码展示 `testsuite` 函数的用法。考虑以下代码，它演示了 `testsuite` 函数的用法：
```V
import os

fn testsuite_begin() {

    os.setenv('foo', 'bar', true)

    println('About to start executing all tests')

}

fn test_env_foo_has_value_bar() {

    println('Executing test')

    // arrange

    inp := 'foo'

    expected := 'bar'

    // act

    actual := os.getenv(inp)

    // assert

    assert actual == expected

}

fn testsuite_end() {

    os.unsetenv('foo')

    println('Finished executing all tests')

}
```
在上面的代码中，我们注意到 `testsuite_begin` 函数使用调用 `os.setenv` 函数设置名为 `foo` 的环境变量和值 `bar`。另外，`testuite_end` 函数通过调用 `os.unsetenv` 函数删除了 `foo` 环境变量。除了在这些 `testsuite` 函数中进行设置和取消设置操作，出于演示目的，我还添加了一个带有消息的打印语句，该消息将显示这些 `testsuite` 函数的执行顺序。同时，有一个名为 `test_env_foo_has_value_bar` 的测试函数，它试图断言 `foo` 环境变量的实际值与期望值 `bar` 是否相等。

要运行上面的代码块，请将其放入名为 `testsuite_demo_test.v` 的文件中，并运行以下命令：
```
v testsuite_demo_test.v
```
上述命令的输出将如下所示：
```
About to start executing all tests

Executing test

Finished executing all tests
```
从上述输出中，我们可以看到测试已通过。我们还观察到从打印消息的顺序中，测试始终首先执行 `testsuite_begin` 函数，然后执行其他测试，最后执行 `testsuite_end` 函数。

在 `test_env_foo_has_value_bar` 测试中需要注意的一点是，它采用了 `AAA` 模式。下一节，我们将简要解释这种模式。

## 编写`AAA`模式的测试

编写测试的 `AAA` 模式指的是单词 `arrange`，`act` 和 `assert` 的首字母缩写。此模式提供了一种清晰的方法来组织成为测试案例一部分的指令。遵循此模式也可以增强测试用例的可读性。以下几点概述了 `AAA` 模式的所有三个阶段的简要说明，这些阶段有助于编写干净、有组织和易读的测试：

- `Arrange`：这是所有阶段中的第一阶段。在这个阶段中，您为要执行操作的函数设置所需的输入。您还可以安排和设置期望值，例如分配给在 `assert` 阶段中可以使用的变量的期望输出。

- `Act`：在我们安排完测试之后，我们就对我们想要测试的函数进行操作。这可以通过调用待测试的函数来完成。如果函数需要任何输入参数，我们可以根据安排阶段中做出的安排将它们传递给待测试的函数。

- `Assert`：最后，我们断言从操作阶段获得的结果是否与安排阶段设置的期望值匹配。


## 为具有可选返回类型的函数编写测试

我们已经学习到测试函数不应该指定返回类型的规则。但是我们也知道，当测试逻辑涉及到有可选返回类型的函数时，可以在测试函数属性上标记符号 "?"。为了证明这一点，考虑以下 greet 函数：
```V
fn greet(name string) ?string {

     if name != '' {

           return 'Hello $name!'

     }

     return error('name not provided')

}
```
greet 函数返回一个 ?string 类型。这意味着，只有在提供非空名称作为输入参数时，greet 才会返回字符串值。如果名称是空字符串，它将返回带有消息"name not provided"的错误。以下代码显示了当提供具有非空字符串的名称参数时的测试用例：
```V
fn test_greet_given_a_name() {

     exp := 'Hello Pavan!'

     assert greet('Pavan') or { err.msg } == exp

}
```
在上述代码中，我们不会看到 assert 的任何失败，因为它使用关系运算符"=="满足左值和右值进行比较的表达式。从逻辑上讲，表达式 greet('Pavan') or {err.msg} 的左值将求值为 "Hello Pavan!"，这与期望值相同。

但是，考虑当函数提供空字符串时的情况。有两种方法可以编写这种场景的测试。第一种方法是让被测试的函数(在此示例中为 greet)传播错误，从而导致测试失败。所以，为了实现这一点，让我们创建一个名为 test_greet_propagates_error 的测试函数，如下所示：
```V
fn test_greet_propagates_error() ? {

     greet('') ?

}
```
注意，上述测试函数标记了返回类型"？"，因为我们只是使用空字符串值在名称参数中调用待测试的 greet 函数。这个测试应该失败，而从以下输出中显而易见：
```
demo_test.v:14: ✗ fn test_greet_propagates_error failed propagation with error: name not provide

   14 |            greet('') ?
```
如果您想要清理测试并捕获由 greet 函数返回的确切错误消息，我们可以编写以下测试：
```V
fn test_greet_when_empty() {

    exp := 'name not provided'

    assert greet('') or { err.msg } == exp

}
```
在上述代码中，我们没有标记测试函数的返回类型"？"。相反，我们断言使用空字符串值返回的错误消息与 exp 变量持有的预期值相同。

以下是在本节中学到的所有代码的完整工作代码：
```V
fn greet(name string) ?string {

    if name != '' {

        return 'Hello $name!'

    }

    return error('name not provided')

}

fn test_greet_given_a_name() {

    exp := 'Hello Pavan!'

    assert greet('Pavan') or { err.msg } == exp

}

fn test_greet_propagates_error() ? {

    greet('') ?

}

fn test_greet_when_empty() {

    exp := 'name not provided'

    assert greet('') or { err.msg } == exp

}
```
我们可以将上述代码放入一个名为 optional_demo_test.v 的测试文件中，并使用 v optional_demo_test.v 命令运行所有三个测试。执行上述命令将产生以下输出，我们可以看到只有名为 test_greet_propagates_error 的测试失败：
```
demo_test.v:14: ✗ fn test_greet_propagates_error failed propagation with error: name not provide

   14 |            greet('') ?
```
在接下来的部分中，我们将学习编写一个简单项目以及带有模块的项目的测试方法。我们还将学习不同的运行测试的方式，例如在单个`_test.v`文件中、在模块内部运行以及运行项目中的所有测试。此外，我们还将看到在使用`stats`参数时优势以及测试输出中产生的信息。

### 为简单程序编写测试

让我们从为V语言中的一个简单问候应用程序编写测试开始。在这种情况下，我们只有一个模块，即主模块。主模块将具有一个名为 `greet.v` 的文件，其中包含一个私有函数 `greet` 和主函数，该函数打印由`greet`函数返回的响应：
```V
module main

fn greet(name string) string {

    return 'Hello $name!'

}

fn main() {

    msg := greet('Bob')

    println(msg)

}
```
接下来，我们将在包含`greet.v`的目录中添加一个名为`greet_test.v`的`_test.v`文件。然后我们添加测试，如下所示：
```V
module main

fn test_greet() {

    // Arrange

    name := 'Bob'

    exp_msg := 'Hello Bob!'

    // Act

    act_msg := greet(name)

    // Assert

    assert act_msg == exp_msg

    assert act_msg.contains(name)

}
```
上述代码具有一个名为`test_greet`的测试功能，断言`greet`函数返回的值，该值未标记为`public`，因为没有`pub`关键字。由于`greet_test.v`文件定义了与`greet.v`相同的主模块`main`，因此无论它们是公共的还是非公共的，所有函数都将对`greet_test.v`中包含的测试可用。此外，在我们的`test_greet`测试中，我们正在作用于这个接受字符串的`greet`函数。调用具有已知名称参数的该函数后，我们希望该函数通过前缀`Hello`将名称返回为消息，如`Hello Bob！`。

在这种情况下，这个简单项目中包含的文件结构如下所示：

    +---01_simple_test
    |         greet.v
    |         greet_test.v


### 运行_test.v文件中包含的测试

要运行`greet_test.v`中包含的测试，我们需要从命令提示符更改当前工作目录为`01_simple_test`，然后运行`v greet_test.v`命令。

执行上述命令将在出现错误时产生详细的输出。详细的错误输出将包括诸如测试名称以及帮助确定测试失败原因的信息。但是在我们的情况下，测试将通过，因此我们不会在控制台上报告任何错误。

V语言不允许为主模块的主函数编写测试。如果我们尝试在任何一个测试中添加测试或调用它，则会看到一条错误信息：
```
Error: the main function cannot be called in the program。
```
### 为带有模块的项目编写测试

现在，我们将看到如何为带有模块的项目编写测试。为了举例说明，我们将有一个名为`modulebasics`的简单项目，如第9章模块中的访问模块成员所述。在继续添加任何测试之前，以下是该项目的目录结构：

    02_modulebasics
    |   .gitignore
    |   modulebasics.v
    |   README.md
    |   v.mod
    |
    \---mod1
        file1.v

正如前面提到的，我们向第9章的V语言代码进行了引用，以简化适合此主题的代码，我们将进行轻微修改。第一个更改是将包含在`file1.v`中的`mod1`的`hello()`函数更改为返回字符串而不仅仅是将消息打印到控制台。这将有助于编写一个测试并断言返回值。因此，`hello`函数将如下所示：

```V
// file: mod1/file1.v

module mod1

pub fn hello() string {

    return 'Hello from mod1!'

}
```
第二个更改是将主函数更改为将从`mod1.hello`函数返回的返回值打印到控制台。因此，位于`modulebasics.v`中的`main`函数将更新如下：
```V
module main

import mod1

fn main() {

    res := mod1.hello()

    println(res)

}
```
现在，我们将为`mod1`的`hello`函数添加测试。首先要做的是在`mod1`目录内的`mod1`模块中添加测试。因此，我们将在`mod1`目录中创建一个名为`mod1_test.v`的文件，并在其中实现以下带有`AAA`模式的测试：
```V
// file: mod1/mod1_test.v

module mod1

fn test_hello() {

    // arrange

    exp := 'Hello from mod1!'

    // act

    act := hello()

    // assert

    assert act == exp

}
```
上面的代码展示了一个简单的测试，它在执行`hello`函数后断言实际和期望值。由于`mod1_test.v`正在定义模块`mod1`并位于相同的目录`mod1`中，因此`mod1_test.v`中包含的测试可以访问`mod1`模块的所有公共和非公共功能。

### 运行模块中包含的测试

正如前面所学，要运行`_test.v`文件中包含的测试，我们只需要运行`v mod1_test.v`命令。但是要运行此命令，必须确保当前工作目录与`mod1_test.v`的位置相同。或者，还可以通过提供`_test.v`文件的相对路径来运行测试。

为避免混淆，V语言允许在使用`v test MODULE_NAME`命令时指定模块名称。因此，从我们`modulebasics`项目的根目录运行以下命令以执行仅存在于`mod1`中的测试：
```
v test mod1
```
上述命令的输出将如下所示：
```
---- Testing... ---------------------------------------------------------------------------------

 OK     2597.143 ms C:/Learn-V-Programming/Chapter12/02_modulebasics/mod1/mod1_test.v

-------------------------------------------------------------------------------------------------
```

### 编写测试来测试子模块的成员函数

现在，我们将进一步添加项目级别的测试，其中我们将访问包含在模块内的函数。为此，我们将在`modulebasics`项目的根目录中添加一个名为`main_test.v`的文件。然后，我们添加一个测试来操作`mod1`模块的`hello`函数：
```V
// file: main_test.v

module main

import mod1

fn test_hello() {

    // arrange

    exp := 'Hello from mod1!'

    // act

    act := mod1.hello()

    // assert

    assert act == exp

    assert mod1.hello().contains('Hello')

}
```
在上述代码中，正如我们所看到的，在我们定义模块`main`的项目级别(根目录)中，这个`test`文件是。为了访问在`mod1`文件夹中的`file1.v`内标记为`pub`的`hello`函数，我们需要在`main_test.v`文件中导入`mod1`。因此，测试现在可以使用调用`mod1.hello()` 来操作`hello`函数。

在这个阶段，我们的`modulebasics`项目的目录结构将被更新并显示如下：

    02_modulebasics
    |   .gitignore
    |   main_test.v
    |   modulebasics.v
    |   README.md
    |   v.mod
    |
    \---mod1
        file1.v
        mod1_test.v

### 运行项目中包含的所有测试

让我们从`main_test.v`和`mod1_test.v`文件运行所有测试。从命令提示符，将工作目录设置为该项目的根目录。接下来，我们将运行以下命令，以执行在所有文件夹中递归查找以`_test.v`结尾的文件并执行以`test_`开头的测试的测试：
```
v test .
```
上述命令将输出如下内容：
```
---- Testing... ---------------------------------------------------------------------------------

 OK     [1/2]  2801.779 ms C:/Learn-V-Programming/Chapter12/02_modulebasics/main_test.v

 OK     [2/2]  2816.950 ms C:/Learn-V-Programming/Chapter12/02_modulebasics/mod1/mod1_test.v

-------------------------------------------------------------------------------------------------
```

在下一节中，我们将看到如何启用`stats`标志来运行测试。

### 使用`stats`标志运行测试

V允许您运行生成测试执行结果详细输出的测试。您可以通过传递`-stats`参数来启用它，在本节中进行了演示。使用`stats`参数，您将获得详细信息，例如编译代码所花费的时间，每个测试运行的`vlines/sec`编译速度单位。除此信息外，您还可以看到每个测试的时间和断言数。

要为我们的`modulebasics`项目启用捕获统计信息功能，请从命令提示符中导航到根目录并运行以下命令：
```
v -stats test .
```
上述命令的输出如下：
```
---- Testing... ---------------------------------------------------------------------------------

-------------------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------------------

           V  source  code size:        19308 lines,      519623 bytes

generated  target  code size:        17395 lines,      588891 bytes

compilation took: 2126.836 ms, compilation speed: 9078 vlines/s

           V  source  code size:        19319 lines,      519726 bytes

generated  target  code size:        17401 lines,      588992 bytes

compilation took: 2223.528 ms, compilation speed: 8688 vlines/s

running tests in: C:Learn-V-ProgrammingChapter12_modulebasicsmod1mod1_test.v

        OK         0.041 ms      1 assert  | mod1.test_hello()

      Summary for running V tests in "C:Learn-V-ProgrammingChapter12_modulebasicsmod1mod1_test.v"

running tests in: C:Learn-V-ProgrammingChapter12_modulebasicsmain_test.v

        OK         0.016 ms      1 assert  | main.test_hello()

      Summary for running V tests in "C:Learn-V-ProgrammingChapter12_modulebasicsmain_test.v": 1

-------------------------------------------------------------------------------------------------
```
从上面的输出信息，我们可以看出，在运行测试时使用`stats`标志，输出中将显示测试执行的详细信息。

## 总结

在本章中，我们学习了如何在V中编写测试。我们看到了`assert`关键字的语法和用法，学习了如何编写简单的测试并运行它们。然后，我们学习了关于`testsuite_begin`和`testsuite_end`函数，并学习了如何分别在执行测试前和测试后使用这些函数进行预处理和后处理活动。本章还介绍了编写测试的流行`AAA`模式。我们还学习了如何为具有可选返回类型的函数编写测试。

在本章的后半部分，我们看到了编写和运行测试的各种方法，从简单的程序到含有模块的程序。我们还学习了运行单个文件中包含的测试、属于模块的测试以及项目中包含的所有测试的不同方法。最后，我们看到了如何使用`stats`参数查看测试运行程序生成的详细输出。通过学习如何在V中编写和运行测试，您现在可以编写涵盖测试的V应用程序。

在下一章中，我们将学习如何在V中构建微服务。

