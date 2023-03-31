# 第11章 通道 - 高级并发模式

通道一词表示允许您从一端传输某些内容到另一端的媒介或路径。在并发上下文中，通道允许我们通过建立并发任务之间的通信通道来共享数据。这些并发任务通常被称为协程，它们通过通过通道进行通信来共享数据。通道是解决协程之间明确处理数据同步技术的高级并发模式。

在V中，我们可以使用共享的对象来在协程之间进行通信，这些对象可以是结构体、数组或映射。但是这种方法的问题在于你需要采取显式的并发同步技术，例如使用只读 rlock 或读写锁等锁定保护共享对象，以防止数据竞争，正如我们在第10章《并发》的"在主线程和并发任务之间共享数据"一节中学到的那样。

在V中，通道可以比作队列。一般情况下，队列允许物品或元素沿着一个方向通过它。进入队列的第一个元素是第一个离开队列的元素。因此，通过通道发送的值以先进先出(FIFO)的方式被访问。

在编程世界中，队列是一种数据结构，只允许数据以单向流动。在V中，将数据添加到通道的过程被称为push，从通道中取出数据的过程则被称为pop。

在本章中，我们首先了解声明通道语法，然后理解不同类型的通道，如缓冲通道和非缓冲通道。接下来我们将了解通道的属性。最后，我们将学习通道上可用的各种方法。

本章将涵盖以下主题：

- 定义通道的语法
- 通道操作
- 通道属性
- 通道方法
- 使用非缓冲通道
- 使用缓冲通道
- 通道选择

本章将帮助您深入了解非缓冲通道的阻塞行为。通过处理代码示例，您将学习如何处理此类通道。您还将能够详细了解缓冲通道的使用。本章还将帮助您了解select语句，并演示如何将通道操作实现为select语句的条件分支。

通过本章的学习，您将能够编写无缝的V程序，利用通道进行并发编程。

### 技术要求

本章完整的源代码可在https://github.com/ToughStyle/V-programming-book-cn/tree/main/codes/Chapter11 上获得。

建议您在每个本章部分中的新的控制台或.v文件中运行代码示例。这将避免变量名之间的冲突。

## 定义通道的语法

在本节中，我们将查看定义通道的语法。在V中，通道是内置功能，您无需导入任何包即可使用它们。chan关键字用于定义通道。要定义通道，可以使用以下语法：
```v
CHANNEL_VARIABLE := chan DATA_TYPE{OPTIONAL_CAPACITY: CAPACITY_VALUE}
```
在此语法中，通道变量将是chan DATA_TYPE . OPTIONAL_CAPACITY类型的。这是一个接受整数值的cap属性的语法表示形式。cap属性在表示通道变量时可用，表示通道可以容纳的值的容量。类型可以是任何类型，例如原始类型，也可以是结构体、映射或数组等。

在理解基本语法之后，我们将在以下子节中学习非缓冲通道和缓冲通道。

### 非缓冲通道

没有容量定义的通道称为非缓冲通道。非缓冲通道默认具有0容量。以下代码显示如何定义非缓冲通道。该通道接受被推入它的整数值：

```v
uc := chan int{}

println(uc.cap) // 0
```

在上述代码中，uc变量是chan int类型。非缓冲通道uc的容量为0。以下代码将uc的类型打印到控制台上：

```v
println(typeof(uc).name) // chan int
```

从上面的代码中，我们可以看到非缓冲通道uc的类型为chan int。

### 缓冲通道

与非缓冲通道不同，缓冲通道具有非零容量，并将其cap属性分配给整数值。以下代码显示如何定义缓冲通道：
```v
bc := chan string{cap: 2}

println(bc.cap)

println(typeof(bc).name)
```
以下是输出:
```
2

chan string
```
在上述代码中，我们定义了一个容量为2的 `chan string` 类型通道。这意味着该通道最多同时容纳两个字符串值。现在，让我们学习如何在V中执行各种通道操作。

## 通道操作

在本节中，我们将学习可以在通道上执行的基本操作。首先，我们将了解箭头运算符，它由 `<-` 符号表示。这代表了在V中数据流进入通道的过程。然后，我们将学习两个基本的通道操作，包括使用箭头运算符将一个值推入通道和从通道中取出一个值。

箭头运算符 `<-`

如本章介绍时所述，V中的通道只允许信息单向流动，并且它是队列的类比。通常而言，在V中通道中的数据总是从右到左流动。

在V中，语法表示的方式也是一致的，数据流的表示总是从右到左的。即使我们看箭头运算符 `<-` 的符号，它也总是指向左边。

箭头运算符 `<-` 的方向表示值总是从右侧输入通道并从左侧退出通道。

### 推操作

在本节中，我们将了解将数据推入V通道的语法。稍后，我们将查看一个简单的示例，该示例演示将整数值推入整数缓冲通道中。

### 将数据推入通道的语法

在V中，推操作是从右到左进行的。这意味着在箭头运算符 `<-` 的位置确定后，要推入通道中的数据值将放置在右侧，并且通道变量将放置在 `<-` 的左侧。以下是将数据推入通道的语法：
```v
ch := chan VALUE_TYPE {OPTIONAL_CAPACITY: CAPACITY_VALUE}

ch <- VALUE_TO_PUSH
```
上述语法显示将数据推入通道 `ch` 中的方式。在这里，我们可以看到，如果要将值推入通道，则该值将放置在箭头运算符的右侧，通道变量将位于箭头运算符的左侧。

值得注意的是，通道上的推操作 `ch <- VALUE_TO_PUSH` 是一个`void`表达式。

### 将数据推入通道

以下代码显示定义和向具有容量为1的 `chan int` 类型缓冲通道推送数据：
```v
ch := chan int{cap: 1}

ch <- 51

println(ch)
```
将上述代码放置在一个文件中，然后使用 `v run filename.v` 命令运行它。您将看到如下输出：
```
chan int{cap: 1, closed: 0}
```
从上面的输出中，我们可以看到我们正在调用 `channel` 变量上的 `str()` 方法。此方法会打印通道的信息，其中包括通道的类型、与通道容量相关的值以及关闭状态。在本章的"通道属性"一节中，我们将详细讨论这些属性。

在非缓冲通道的情况下，推操作(如 `ch <- VALUE_TO_PUSH` 语法中所指定的)是一个阻塞语句。例如，考虑我们用来定义和向具有容量的通道推送值的代码，但这次我们不会指定容量，如下所示：
```v
ch := chan int{}

ch <- 51

println(ch)
```
将上述代码放置在一个文件中，然后使用 `v run filename.v` 命令运行它。您将看到控制台停止并且没有任何输出。这意味着程序一遇到推操作即进入了阻塞状态，即 `ch <- 51`。我们将在本章的"使用非缓冲通道"一节中更详细地讨论为什么在非缓冲通道上使用推操作会阻塞程序的执行。

为了停止程序并从前面的程序的阻塞执行获得控制权，您需要强制停止执行。要执行此操作，请在键盘上按 `Ctrl + C` 组合键，您就可以了。
### 弹出操作

在本节中，我们将了解如何在V中从通道中弹出数据。稍后，我们将查看一个简单的示例，该示例演示将整数值推入整数缓冲通道中后，从整数通道中弹出整数值。

### 将数据弹出通道的语法

与推操作类似，在V中，弹出操作也由右到左进行，因为推操作期间从右边输入的数据从左边退出。

这意味着，使用箭头操作符`<-` 指定位置时，通道变量将放置在箭头运算符的右侧。从通道中弹出的数据被捕获在放置在箭头运算符的左侧的变量中。以下是从通道中弹出数据的语法：
```v
ch := chan VALUE_TYPE{OPTIONAL_CAPACITY: CAPACITY_VALUE}

ch <- VALUE_TO_PUSH

x := <- ch
```
在上述语法中，我们定义了一个通道，然后是将一个值推入通道的语句。在最后一行中，我们可以看到表达式，即`<- ch`。这个表达式是从通道中弹出的值，如果在左侧放置了一个变量，比如这里的 `x`，那么 `x` 变量将赋值为从通道中弹出的值。

### 从通道中弹出数据

以下代码显示将值从通道中弹出：
```v
ch := chan int{cap: 1}

ch <- 51

println('channel after push: $ch.str()')

println('popping value out of the channel and storing it in immutable variable x')

x := <-ch

println('value of x: $x')

println('channel after pop: $ch')
```
将上述代码放置在扩展名为`.v`的文件中，并使用`v run filename.v`命令运行它。

上面程序输出的结果如下所示：
```
channel after push: chan int{cap: 1, closed: 0}

popping value out of the channel and storing it in immutable variable x

value of x: 51

channel after pop: chan int{cap: 1, closed: 0}
```
从上述输出中，我们可以看出，从通道中弹出值后，该值被分配给了 `x` 变量，因为打印 `x` 变量显示了值为 51。到目前为止，我们已经学习了使用箭头操作符<-在通道上执行的各种操作。接下来，我们将学习通道属性。

## 通道属性

通过访问通道变量公开的属性，可以获取有关通道变量的信息。通道的属性包括`len`，`cap`和`closed`。这些属性提供以下有关访问它们时通道的信息：

`cap`是一个整数属性，指示通道的容量。未缓冲的通道为0。在缓冲通道的情况下，`cap`属性表示通道可以容纳的最大值数量。

`len`是一个整数属性，指示访问该属性时通道实际持有的值的数量。在任何给定时间，`len`值只能小于或等于`cap`属性。

`closed`是一个布尔属性，当其值为`true`时，表示通道已关闭。如果通道未关闭，则`closed`属性的值为`false`。

### 使用示例了解通道属性

在本节中，我们将通过简单的示例了解上一节中所述通道的三个属性。考虑以下缓冲通道：
```v
b := chan string{cap: 2}

b <- 'hello'

println('capacity: ${b.cap}')

println('length: ${b.len}')

println('closed: ${b.closed}')
```
在上面的代码中，我们创建了一个具有`cap`属性设置为2的缓冲通道`b`。然后，我们将`hello`值推到缓冲通道`b`中。然后，在每个打印语句中打印三个属性，其结果如下所示：
```
capacity: 2

length: 1

closed: false
```
请注意，缓冲通道的容量为2，因为我们已为其定义了容量。但是，队列长度为1，因为我们仅将一个字符串值推入了通道b中。`closed`属性打印出一个`false`值，这意味着通道`b`是开放的。

现在我们已经了解了通道的属性，让我们看一下可用于通道的各种方法，并学习如何使用它们。

## 通道方法

V暴露了一些公共方法来控制通道的行为。这些方法包括以下内容：

- `close()`

- `try_push()`

- `try_pop()`

`try_push()`和`try_pop()`方法除了`close()`方法外还有一个返回值，这是一个名为`ChanState`的内置枚举类型。`ChanState`枚举有三个枚举值：

- `not_ready`

- `closed`

- `success`

对通道执行`try_push()`或`try_pop()`可以返回上述三种状态之一。在本节中，我们将学习如何在缓冲和非缓冲通道上使用`try_push()`函数，然后介绍如何处理`try_pop()`和`close()`方法。

### 在非缓冲通道上使用`try_push()`

`try_push()` 以优雅的方式将数据推送到通道中，并以`ChanState`枚举值的形式返回状态。`try_push()`方法接受通道接受类型的值。对于未缓冲的通道，当没有协程准备好从通道中弹出值时，`try_push()`操作返回 `ChanState` 枚举的 `.not_ready` 值。为了说明这一点，请考虑以下代码：
```v
v := 'hi'

ch := chan string{} // 非缓冲通道

res := ch.try_push(v)

println(res) // not_ready
```
上面的代码演示了在未缓冲的通道上使用`try_push()`方法。

在非缓冲通道使用`try_push`的注意事项

在上面的代码中，我们使用了`try_push()`操作来在非缓冲通道上使用，它立即将控制权返回给程序。然后，执行控制流程继续进行到下一行，并打印出`res`的值。但是，使用箭头运算符`<-` 的推操作是阻塞性质的，而使用`try_push()`则会失去这种未缓冲通道的阻塞性质。在这种情况下，程序会继续执行下一个语句序列，因此如果将任何数据推送到此通道，则会因执行流程而失去。因此，在使用`try_push()`时必须小心，因为它停止了在多个同时运行的协程之间共享的通道的工作，从而导致意外的行为。本章的"处理未缓冲通道"部分将更详细地解释未缓冲通道的阻塞性质。

### 在缓冲通道上使用try_push()

下面的代码演示了如何在字符串缓冲通道上使用`try_push()`：
```v
x := 'hello'

ch := chan string{cap: 2}

for {

    status := ch.try_push(x)

    if status == .success {

        println('Channel length: $ch.len')

    } else {

        println('channel status: $status')

        break

    }

}
```
在上述代码中，我们定义了一个具有容量为2的`chan string`类型的缓冲通道`ch`。这意味着当推入两个以上的字符串值而没有将它们弹出时，第三个`try_push()`方法的状态将指示未准备好，如下所示：

```
Channel length: 1

Channel length: 2

channel status: not_ready
```
在缓冲通道上使用`try_push`的注意事项

上面的输出表明，`try_push()`操作成功，直到通道的长度等于通道容量。在这个例子中，通道容量是2。当程序尝试推送第三个值时，通道的状态变为`not_ready`。我们最终会遇到竞争条件，如条件if块，以识别通道上`try_push()`操作的结果。在这种情况下，处理不同情况并根据通道状态做出决策将变得复杂。虽然这似乎是干净简洁的代码，但使用`try_push()`并没有通过抛出异常来指示通道已满容量。
### `try_pop()`

`try_pop()`方法可以优雅地从通道中弹出数据，并以`ChanState`枚举值的形式返回状态。`try_pop()`方法接受可变变量作为输入参数，其类型与通道接受的值的类型相匹配。下面的代码演示了在缓冲的`chan int`类型通道上使用`try_pop()`的用法：

```v
ch := chan int{cap: 1}

mut x, mut y := 0, 0

ch <- 101

mut status := ch.try_pop(mut x)

println('try pop resulted in status: $status, Value of x: $x')

status = ch.try_pop(mut y)

println('try pop resulted in status: $status, Value of y: $y')
```

在上面的代码块中，我们创建了容量为1的`chan int`类型的缓冲通道。然后，我们定义了两个可变变量x和y，并将它们初始化为0。接着，我们将101整数值推入通道中。在下一行中，我们调用了`try_pop()`方法，然后打印了操作的状态，以及弹出到x变量中的值。现在，通道已经弹出了唯一的元素，再次执行`try_pop()`方法不会得到正确的结果。因此，在下面的输出中，y的值仍然是0：

```
try pop resulted in status: success, Value of x: 101

try pop resulted in status: not_ready, Value of y: 0
```

### 在通道上使用`try_pop`的注意事项

从输出结果可以看出，变量`y`的值仍为0。无法正确定位分配给`y`的值是否是初始化值，还是分配给弹出自通道的传入值的值。虽然您可以通过查看通道状态或通道长度来找出这一点，但您可能会遇到多个竞争条件，并且可能会在程序需求或功能发生更改时无法添加更多未知条件。因此，`try_pop()`方法剥夺了通道的真正本质，通道的本质是允许您通过在协程之间建立通信通道来共享数据。

### 总结`try_push()`和`try_pop()`的用法

在前面的部分中，我们已经了解了在通道上使用`try_push()`和`try_pop()`方法的注意事项，因此不建议在生产环境中使用这些方法。虽然您可以评估这些方法进行开发和调试，但为了享受通道的真正本质-建立协程之间的通信通道来共享数据，建议使用标准箭头运算符`<-`，该运算符有助于在通道中传输数据。使用`<-`的方法可以导致未处理的情况引发错误，并在协程之间保持数据同步。在处理未处理的异常情况时，我们可以灵活使用`or{}`块，如下一节所示，或者可以选择让异常传播。

### `close()`

`close()`方法用于关闭向通道中推送的传入数据。`close()`方法不接受任何输入参数。`close()`是一个无返回值的方法，它将设置通道的状态为`.closed`。

当通道关闭时，实际上意味着以下内容：

- 无法向关闭的通道中推送数据。
- 如果有剩余的数据可供弹出，则允许从关闭的通道中弹出数据。
- 在通道上执行`try_push()`操作会导致状态为`.closed`。

下面的代码演示了在缓冲通道上使用`close()`方法的用法：
```v
module main

fn main() {

    ch := chan int{cap: 2}

    // push using arrow operator: <-

    ch <- 123 // Push 1st element into the channel

    ch <- 222 // Push 2nd element into the channel

    println(<-ch) // pop using: <- First in is the first to

                  // out. So prints 123

    ch.close() // Close channel

    // Push after closed

    ch <- 333 or { println('cannot push into a 
    closed channel') }

    // try_push will result .closed

    new_val := 999

    status := ch.try_push(new_val)

    println('try_push on a closed channel resulted 
    in status: $status')

    // We still have one more element to pop

    println(<-ch) // 222

}
```
从前面的代码可以看出，我们对缓冲通道进行了各种操作，类型为`chan int`。我们推入了两个值，然后在调用`ch.close()`语句关闭通道之前从通道中弹出了一个值。

在关闭通道后，我们执行了各种操作，例如使用箭头操作符<-推入值，并使用`or{}`块打印行为以将案例处理到控制台。然后，我们执行了`try_push()`并将状态写入控制台。此外，在最后一个`print`语句中，我们弹出通道中唯一剩余的值。所有这里显示的操作的输出都是不言自明的:
```
123

cannot push into a closed channel

try_push on a closed channel resulted in status: closed

222
```
有时，您需要在退出程序之前关闭通道。要以延迟的方式关闭通道，您可以利用`defer{}`块，在其中可以关闭通道，如下所示：

```v
module main

fn main() {

    ch := chan int{cap: 2}

    defer {

        ch.close()

    } // Deferred execution to Close channel

    // push using arrow operator: <-

    ch <- 123 // Push 1st element into the channel

    ch <- 222 // Push 2nd element into the channel

    println(<-ch) // pop using: <- First in is the first to

                  // out. So prints 123

    // Push after closed

    ch <- 333 or { println('cannot push into a closed

                  channel') }

    // try_push will result .closed

    new_val := 999

    status := ch.try_push(new_val)

    println('try_push on a closed channel resulted in

            status: $status')

    // We still have one more element to pop

    println(<-ch) // 222

}
```

关于defer块的详细信息在第7章函数的函数允许您使用defer块推迟执行流的部分中进行了介绍。

了解了`try_push()`、`try_pop()`和`close()`通道方法，让我们学习如何使用缓冲通道。

## 使用非缓冲通道

默认情况下，除非您指定容量，否则在V中，通道是非缓冲的。在本节中，我们将学习如何使用非缓冲通道。使用非缓冲通道的主要方面是，在没有协程弹出非缓冲通道中的值的情况下，它们上面的推送操作会阻塞代码。

### 理解非缓冲通道的阻塞性质

在本节中，我们将学习为什么非缓冲通道是阻塞性的。考虑以下代码，它演示了非缓冲通道的阻塞行为:

```v
module main

fn main() {

    ch := chan int{}

    defer {

        ch.close()

    }

    ch <- 3

    x := <-ch

    println(x)

    println('End main')

}
```

在上述代码中，我们拥有主函数，第一行有`ch`变量声明为非缓冲通道。

在下一行中，我们推迟关闭通道。然后是`ch <- 3`语句，在其中值被推入非缓冲通道`ch`中。此操作阻塞程序的执行，直到有人(可能是协程)弹出该值。

简而言之，非缓冲通道在推入值时会阻塞执行。因为通道会等待直到有其他协程弹出值。这相当于在通道上执行`try_push()`时，`ChanState`枚举的`.not_ready`值，直到有一个协程主动弹出通道中的值。

虽然上述代码有`x <- ch`表达式，但它在`ch <- 3`阻塞语句之后。

因此，除非有其他协程从通道中弹出这个值，否则执行将永远停在`ch <- 3`语句处。
将前面的代码放在扩展名为.v的文件中，并使用`v run filename.v`命令运行它将停止执行，您将无法看到任何输出打印到控制台。要从命令行终端中杀死程序，需要按下`Ctrl + C`键组合。

因此，这就需要一个具有访问通道并在值被推入通道后立即弹出该值的并发例程。

### 处理非缓冲通道的阻塞行为

为了防止程序阻塞代码的执行，我们需要添加一个协程，在将值推入非缓冲通道时立即弹出该值。让我们修改前面的代码，以便具有接收器协程作为输入参数接受通道，如下所示：
```v
module main

fn receiver(ch chan int) {

    println('Received value from the channel ${<-ch}')

}

fn main() {

    ch := chan int{}

    defer {

        ch.close()

    }

    go receiver(ch)

    ch <- 3

    println('End main')

}
```
接收器函数接受一个通道作为输入参数。然后，它从`ch`通道中弹出值并将其打印到控制台输出中，每当将整数推入非缓冲通道`ch`时。

查看前面代码中的`main`函数，我们正在声明一个`chan int`类型的非缓冲通道，该类型接受要推入其中的整数值。在下一行中，我们正在生成一个并发协程，该协程接受`ch`作为输入参数。

当协程并发运行时，只有在将值推入通道后它才会等待。然后它将打印并退出例程。回到主函数，执行流程转到推送值3到ch的下一行。

我们知道从前面的代码示例中，对非缓冲通道执行推送操作会阻塞执行。但是前面的代码已经直接在`ch <- 3`表达式之后生成了一个名为`receiver`的协程。在接收器函数中，使用`${<-ch}`表达式作为打印语句的一部分，打印了消息，以及它从通道中弹出值的位置的消息。

通过这个解释，我们将得到以下两个输出之一。

以下是第一个输出:
```
End main

'Received value from the channel ${<-ch}
```
以下是第二个输出:
```
End main
```
如果多次运行程序，您将看到其中一个输出显示的变化。在输出1中，程序花费了更多时间退出，因此不得不在打印`End main`后打印来自接收器函数的消息。但是期望的是先打印来自接收器函数的消息，然后是`End main`。您是否想知道为什么协程未能将消息打印到输出控制台中的输出2？这是因为调用线程未等待协程执行完毕。

正如我们所知，为了使此程序按预期输出，我们需要等待协程完全执行其工作。因此，在主函数中，我们将等待从生成的接收器协程获取的句柄，如下所示：
```v
module main

fn receiver(ch chan int) {

    println('Received value from the channel ${<-ch}')

}

fn main() {

    ch := chan int{}

    defer {

        ch.close()

    }

    t := go receiver(ch)

    ch <- 3

    t.wait()

    println('End main')

}
```
现在，我们正在等待协程完成执行代码，前面代码的输出将如下所示：
```
Received value in the channel 3

End main
```
在前面的代码中，我们仅向通道推送一次数据。接收器被设计为接收推送到`chan int`类型的非缓冲通道中的第一个数据项。
### 通过非缓冲通道在协程之间同步数据

当有一个协程不停地发送值时会发生什么？您是否认为接收器被设计为与发送者同步以及它如何向通道发送数据？让我们考虑以下代码，我们正在修改以前的代码，并引入了一个名为`sender`的协程，通过通信通道与`receiver`共享数据：
```v
module main

const (

    count = 4

)

fn sender(ch chan int) {

    for i in 0 .. count {

        ch <- i // since the push operation is a void

        // expression, this cannot be placed in a println

        println('Sent $i into the channel')

    }

}

fn receiver(ch chan int) {

    println('Received value from the channel ${<-ch}')

}

fn main() {

    ch := chan int{}

    defer {

        ch.close()

    }

    t := go receiver(ch)

    go sender(ch)

    t.wait()

    println('End main')

}
```
在前面的代码中，我们将将数据推入`ch <- 3`通道的逻辑移动到`sender`协程中。这个协程不是推送值3，而是迭代数字范围`0..4`，因此对于每次迭代，预期将推送相应的值，即0、1、2和3。话虽如此，`receiver`仍然是相同的函数，它只是打印推送到通道上的值。这段代码将打印以下输出，这不是我们想看到的：
```
Sent 0 into the channel

Received value from the channel 0

End main
```
从前面的输出中，我们期望看到范围内所有发送和接收的值，即0、1、2和3。这是因为在接收器协程接收到该值后，它将控制权返回到主程序。这是因为接收器仅设计为从通道中接收一个值。因此，在`sender`将0推到通道后，接收器将其弹出。对于下一次迭代，`sender`然后推送1并阻塞执行，希望有其他协程准备将1弹出它。

因此，为了实现同步，接收器协程可以在无限循环中弹出通道，这有点过度并且不建议。实现完美同步的另一个可能的方法是了解被推入通道的数据值的数量。在这种情况下，`sender`协程正在迭代`0..count`的范围，而`count`被定义为常量，这足以使接收器协程迭代4次。以下代码演示了让协程通过彼此之间建立的非缓冲通道共享数据的正确同步机制：
```v
module main

const (

    count = 4

)

fn sender(ch chan int) {

    for i in 0 .. count {

        ch <- i // since the push operation is a void

          // expression, this cannot be placed in a println

        println('Sent $i into the channel')

    }

}

fn receiver(ch chan int) {

    for _ in 0 .. count {

        println('Received value from the channel ${<-ch}')

    }

}

fn main() {

    ch := chan int{}

    defer {

        ch.close()

    }

    t := go receiver(ch)

    go sender(ch)

    t.wait()

    println('End main')

}
```
在前面的代码中，这次数据同步在协程之间顺利进行，而不会阻止程序执行。这体现在输出中，其中`sender`和`receiver`协程共享数据，如预期所示：
```
Received value in the channel 0

Sent 0 into the channel

Received value from the channel 1

Sent 1 into the channel

Received value from the channel 2

Sent 2 into the channel

Received value from the channel 3

Sent 3 into the channel

End main
```
到目前为止，我们已经学习了如何使用非缓冲通道，并学习了如何处理它们的阻塞行为。现在，我们将学习如何使用缓冲通道。

## 使用缓冲通道进行工作

在本章早些时候，我们学习了缓冲通道将使用非零整数值定义cap属性。在本节中，我们将学习如何使用缓冲通道以及如何通过建立协程之间的通信来共享数据。

### 了解缓冲通道的行为

与非缓冲通道不同，缓冲通道是非阻塞通道，在其声明中指定非零容量。以下代码演示了缓冲通道的简单示例：
```v
module main

fn main() {

    ch := chan int{cap: 1}

    defer {

        ch.close()

    }

    ch <- 3

    x := <-ch

    println(x)

    println('End main')

}
```
在前面的代码中，我们创建了一个`chan int`类型的缓冲通道，并将容量设置为1。我们需要做的就是从通道中推送和弹出数据。如果回顾"使用非缓冲通道"部分中的第一个代码示例，则该部分中的代码块与所示代码块类似。唯一的例外是此代码块中的`ch`通道被指定为`{cap:1}`的容量。

我们还可以看到，在缓冲通道的情况下，推操作是非阻塞的，直到缓冲区已满，程序不会停止执行。在这里，我们可以看到执行控制权继续执行到下一行，从而将值打印到输出控制台中，如下所示：
```
3

End main
```


### 在协程之间建立缓冲通信通道

现在，让我们学习如何实现允许协程通过缓冲通道共享数据的简单程序。考虑以下代码，它在协程之间建立了缓冲通信通道：
```v
module main

fn sender(ch chan int) {

    val := 3

    println('Sending value: $val in the channel')

    ch <- val

    println('sent value: $val in the channel')

}

fn receiver(ch chan int) {

    println('Received value from the channel ${<-ch}')

}

fn main() {

    ch := chan int{cap: 1}

    defer {

        ch.close()

    }

    t := go receiver(ch)

    go sender(ch)

    t.wait()

    println('End main')

}
```
在前面的代码中，主函数创建了一个`chan int`类型的通道，并将其作为输入参数传递给`sender`和`receiver`函数。`sender`将值3推入通道，而`receiver`弹出通道中的数据。此外，请注意，主函数将函数`sender`并发运行。由于我们有兴趣打印弹出通道的值，因此我们将`go receiver`协程的处理程序分配给变量t，并等待`coroutine`执行完毕，调用`t.wait()`语句。前面代码的输出将如下所示：
```
Sending value: 3 in the channel

sent value: 3 in the channel

Received value from the channel 3

End main
```
### 在通过缓冲通道进行通信的协程之间同步数据

在前面的代码中，我们发现缓冲通道容量为1，因此它只能保存一个元素。现在，让我们考虑容量为2的缓冲通道，并且我们的`sender`协程在迭代`0..count`范围内不断发送消息，其中`count`是4的常量值：
```v
module main

const (

    count = 4

)

fn sender(ch chan int) {

    for i in 0 .. count {

        ch <- i

        println('sent value: $i in the channel')

    }

}

fn receiver(ch chan int) {

    println('Received value from the channel ${<-ch}')

}

fn main() {

    ch := chan int{cap: 2}

    defer {

        ch.close()

    }

    t := go receiver(ch)

    go sender(ch)

    t.wait()

    println('End main')

}
```
让我们看一下前面代码的输出并尝试分析问题：
```
sent value: 0 in the channel

sent value: 1 in the channel

Received value from the channel 0

sent value: 2 in the channel

End main
```

在这里，我们可以看到上面的代码产生了两个问题：

第一个问题是`receiver`仅能够打印0，即使缓冲通道的容量为2。

第二个问题是`sender`在将值2推入缓冲通道后停止了执行。

第一个问题的原因是sender将数据推入缓冲通道4次，而`receiver`仅从通道中弹出数据1次。

第二个问题的原因是`sender`在将0、1和2推入到缓冲通道之后停止了执行。背后发生的事情是一旦0被推入通道，`receiver`函数就会弹出0，因此通道的长度再次变为0。由于`receiver`已经完成了弹出并打印通道中的值的唯一过程，所以执行控制流返回到调用线程。但是，`sender`协程继续将下一个范围内的值(如1和2)推入。在推入1和2之后，缓冲通道已达到容量，因为其定义的容量为2。因此，根据前面的输出，我们可以看到发送了0、1和2，但只接收到了0。

要解决这两个问题，我们需要修改代码，使得`receiver`协程将从通道中弹出的数据与`sender`协程推入通道的数据匹配。因此，`receiver`协程必须进行以下修改：
```v
module main

const (

    count = 4

)

fn sender(ch chan int) {

    for i in 0 .. count {

        ch <- i

        println('sent value: $i in the channel')

    }

}

fn receiver(ch chan int) {

    for _ in 0 .. count {

        println('Received value from the channel ${<-ch}')

    }

}

fn main() {

    ch := chan int{cap: 2}

    defer {

        ch.close()

    }

    t := go receiver(ch)

    go sender(ch)

    t.wait()

    println('End main')

}
```
前面的代码现在与推操作和弹出操作的次数同步，因此输出将如下所示：
```
sent value: 0 in the channel

sent value: 1 in the channel

Received value from the channel 0

sent value: 2 in the channel

Received value from the channel 1

sent value: 3 in the channel

Received value from the channel 2

Received value from the channel 3

End main
```

## 通道选择

V具有`select`语句，您可以使用它来等待多个通道及其操作。`select`语句可以具有多个分支和情况，所有这些都可以用于表示通道推送或弹出操作。它还可以具有超时情况，我们可以定义超时情况以在没有触发任何通道操作情况的情况下退出选择。

`select`语句在语法上类似于匹配块，但是`select`的情况不必是接受相似数据类型的通道。但是对于`match`，如第6章"条件和迭代语句"中所解释的，所有条件分支都必须是相似的数据类型。

以下是在使用`select`语句时需要注意的几点：

- `select`语句随机选择已准备执行的情况。
- `select`语句在活动情况完成并退出情况之前阻止其他情况。
- `select`语句的情况可以是任何通道操作，例如推送或弹出。
- 对于给定的`select`语句，情况可以具有任何类型的缓冲和非缓冲通道。
- `select`语句可以用作布尔表达式，并在通道打开时返回`true`，在所有通道关闭时返回`false`。

让我们了解在处理通道时使用`select`语句的用法。为了说明这一点，我创建了两个函数，如下所示：
```v
fn process1(ch chan int) {

    for i in 1 .. 6 {

        sq := i * i

        println('process1: value being pushed on ch1: $sq')

        ch <- sq

    }

}

fn process2(ch chan string) {

    msg := 'hello from process 2'

    println('process2: value being pushed on ch2: $msg')

    ch <- msg

}
```
从前面的代码中，我们可以看到`process1`函数接受`chan int`类型的输入参数。它在`1..6`的范围内进行迭代，并使用`ch<-sq`语句将值的平方推入通道。类似地，`process2`函数接受`chanstring`类型的输入参数。它还向传递给函数的通道推送一些消息。
现在，我们将编写主函数，以便并发运行这两个函数。由于这两个函数需要输入通道参数，因此我们将在主函数中定义它们，如下所示：
```v
fn main() {

    ch1 := chan int{cap: 5} // 缓冲通道

    ch2 := chan string{} // 非缓冲通道

    defer {

        ch1.close()

        ch2.close()

    }

    go process1(ch1)

    go process2(ch2)

}
```
继续使用前面的代码，在并发运行`process1`和`process2`协程之后，我们将添加`select`语句，其中包含情况，如以下代码所示：
```v
select {

    a := <-ch1 {

        println('main: value popped from ch1: $a')

    }

    b := <-ch2 {

        println('main: value popped from ch2: $b')

    }

}
```
前面的代码仅显示了`select`语句，它成为我们到目前为止实现的主函数的一部分。

如果我们将迄今为止实现的代码放在一个`.v`文件中，并使用`v run filename.v`命令运行它，我们将看到以下输出：
```
process1: value being pushed on ch1: 1

process2: value being pushed on ch2: hello from process 2

process1: value being pushed on ch1: 4

process1: value being pushed on ch1: 9

main: value popped from ch1: 1

process1: value being pushed on ch1: 16
```
从前面的输出中，我们可以看到`process1`正在推送值。但是，只有`a := <-ch1`情况在`select`语句中被执行一次。我们还可以看到没有迹象表明`b := <-ch2 case`代码块正在执行。

为了跟踪流经ch1和ch2通道的数据，我们需要使我们在`select`语句中指定的两个情况工作。我们可以通过在一个无限的`for`循环中包装我们到目前为止实现的`select`语句来做到这一点。在`for`循环之后，我们必须编写一个带有消息的`print`语句`done`，我们期望在无限的`for`循环之后运行。此时，主函数将如下所示：
```v
fn main() {

    ch1 := chan int{cap: 5} // 缓冲通道

    ch2 := chan string{} // 非缓冲通道

    defer {

        ch1.close()

        ch2.close()

    }

    go process1(ch1)

    go process2(ch2)

    for {

        select {

            a := <-ch1 {

                println('main: value popped from ch1: $a')

            }

            b := <-ch2 {

                println('main: value popped from ch2: $b')

            }

        }

    }

    println('done')

}
```
在使用上述更改执行程序之后，它将如下所示：
```
process2: value being pushed on ch2: hello from process 2

process1: value being pushed on ch1: 1

main: value popped from ch2: hello from process 2

process1: value being pushed on ch1: 4

main: value popped from ch1: 1

process1: value being pushed on ch1: 9

main: value popped from ch1: 4

process1: value being pushed on ch1: 16

process1: value being pushed on ch1: 25

main: value popped from ch1: 9

main: value popped from ch1: 16

main: value popped from ch1: 25
```
从前面的输出中，我们可以看到，即使选择语句的所有情况都完成了其各自的代码块的执行，程序也不会进一步执行。这是因为它被阻止并在无限的`for`循环中继续执行，等待选择语句条件分支中的任何操作成功。因此，我们永远不会到达打印`done`到控制台输出的语句。

正如我们之前提到的，我们可以在`select`语句中指定一个条件，该条件每2秒执行一次。让我们修改`select`，使新情况在2秒的不活动时间内继续执行：

```v
mut sec := 0

for {

    select {

        a := <-ch1 {

            println('main: value popped from ch1: $a')

        }

        b := <-ch2 {

            println('main: value popped from ch2: $b')

        }

        2 * time.second {

            /* this case executes for every 2 seconds of inactivity by any other channels in this */

            sec = sec + 2

            println('main: more than ${sec}s passed without

                    a channel being ready')

            if sec >= 6 {

                println('exiting out of select after $sec

                   seconds of inactivity amongst channels')

                break

            }

        }

    }

}
```
从前面的代码中，我们可以看到，除了其他通道操作情况之外，我们添加了一个名为`2 * time.second`的情况。如果属于`select`语句的其他通道之间没有任何活动，此情况每2秒执行一次。因此，我们可以利用这种情况，并尝试基于某些预定义的超时(例如6秒)退出无限`for`循环。为实现这一点，我们创建了一个可变变量称为`sec`，每次通过`select`语句执行此情况时，该变量都会增加2。每当sec的值变为大于或等于6时，我们可以选择打破无限`for`循环并退出它。由于我们使用`time.second`，必须确保将`import time`语句添加到您正在使用的代码文件中。

此时的输出如下所示：
```
process1: value being pushed on ch1: 1

process2: value being pushed on ch2: hello from process 2

process1: value being pushed on ch1: 4

main: value popped from ch1: 1

process1: value being pushed on ch1: 9

process1: value being pushed on ch1: 16

process1: value being pushed on ch1: 25

main: value popped from ch2: hello from process 2

main: value popped from ch1: 4

main: value popped from ch1: 9

main: value popped from ch1: 16

main: value popped from ch1: 25

main: more than 2s passed without a channel being ready

main: more than 4s passed without a channel being ready

main: more than 6s passed without a channel being ready

exiting out of select after 6 seconds of inactivity amongst channels

done
```
但是，如果其中一个进程响应时间过长怎么办？例如，`process1`从现在开始需要3秒才能将值推入通道，如下所示：
```v
import time

fn process1(ch chan int) {

    for i in 1 .. 6 {

        sq := i * i

        time.sleep(3 * time.second)

        println('process1: value being pushed on ch1: $sq')

        ch <- sq

    }

}
```
在这种情况下，输出将如下所示：
```
process2: value being pushed on ch2: hello from process 2

main: value popped from ch2: hello from process 2

main: more than 2s passed without a channel being ready

process1: value being pushed on ch1: 1

main: value popped from ch1: 1

main: more than 4s passed without a channel being ready

process1: value being pushed on ch1: 4

main: value popped from ch1: 4

main: more than 6s passed without a channel being ready

exiting out of select after 6 seconds of inactivity amongst channels

done
```
在这里，我们可以看到，每当选择语句的一个情况中有任何活动时，我们都没有重置超时计数器变量`sec`。因此，我们需要在`select`语句的每种情况下重置`sec`为0。此时，完整的源代码将如下所示：

```v
module main

import time

fn process1(ch chan int) {

    for i in 1 .. 6 {

        sq := i * i

        time.sleep(3 * time.second)

        println('process1: value being pushed on ch1: $sq')

        ch <- sq

    }

}

fn process2(ch chan string) {

    msg := 'hello from process 2'

    println('process2: value being pushed on ch2: $msg')

    ch <- msg

}

fn main() {

    ch1 := chan int{cap: 5} // buffered channel

    ch2 := chan string{} // unbuffered channel

    defer {

        ch1.close()

        ch2.close()

    }

    go process1(ch1)

    go process2(ch2)

    mut sec := 0

    for {

        select {

            a := <-ch1 {

                sec = 0

                println('main: value popped from ch1: $a')

            }

            b := <-ch2 {

                sec = 0

                println('main: value popped from ch2: $b')

            }

            2 * time.second {

                /* this case executes for every 2 seconds of inactivity by any other channels in

                sec = sec + 2

                println('main: more than ${sec}s passed

                        without a channel being ready')

                if sec >= 6 {

                    println('exiting out of select after

                            $sec seconds of inactivity

                            amongst channels')

                    break

                }

            }

        }

    }

    println('done')

}
```
以上代码的输出如下所示：

```
process2: value being pushed on ch2: hello from process 2

main: value popped from ch2: hello from process 2

main: more than 2s passed without a channel being ready

process1: value being pushed on ch1: 1

main: value popped from ch1: 1

main: more than 2s passed without a channel being ready

process1: value being pushed on ch1: 4

main: value popped from ch1: 4

main: more than 2s passed without a channel being ready

process1: value being pushed on ch1: 9

main: value popped from ch1: 9

main: more than 2s passed without a channel being ready

process1: value being pushed on ch1: 16

main: value popped from ch1: 16

main: more than 2s passed without a channel being ready

process1: value being pushed on ch1: 25

main: value popped from ch1: 25

main: more than 2s passed without a channel being ready

main: more than 4s passed without a channel being ready

main: more than 6s passed without a channel being ready
```
在前面的输出中，我们可以看到，在任何通道操作情况之间的2秒钟不活动后，超时计时器会被初始化。但是，经过3秒钟后，它会在每三秒钟重置为0。这个重置是由于 `a := <-ch1` 情况触发的，因为它会在 `process1` 将值推入范围 `1..6` 的 `ch1` 中的每3秒钟调用一次。一旦所有值都已推入 `ch1`，由于包含在 `select` 语句中的任何其他通道操作情况的不活动状态，`sec` 计数器将继续递增，直到它达到6秒。最后，它退出无限 `for` 循环并将`done`打印到控制台。

## 总结

在本章中，我们学习了如何通过通道之间进行通信来共享数据。我们首先学习了定义无缓冲和缓冲通道的语法。然后，我们学习了如何使用 `<-` 运算符在通道上执行推送和弹出操作。接着，我们学习了通道变量可用的各种属性。我们还了解了如何使用 `try_push()`、`try_pop()` 和 `close()` 通道方法。

随后，我们通过在 V 中编写代码示例来学习如何使用无缓冲通道，并且我们也理解了无缓冲通道的阻塞性质以及如何处理它们。然后，我们介绍了如何在无缓冲通道的协程之间同步数据。同样，我们通过查看代码示例了解了如何使用缓冲通道及其行为。

最后，我们学习了如何使用 `select` 语句，并编写了成为 `select` 语句条件分支一部分的通道操作。通过学习这些概念，您现在可以使用通道帮助在 V 中编写强大的并发程序。

在下一章中，我们将学习如何向 V 代码添加测试。
