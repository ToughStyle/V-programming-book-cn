# 第4章 V的原始数据类型

在上一章中，我们详细介绍了变量和常量的使用。我们还学习了如何在代码中添加单行和多行注释以提高代码可读性。在本章中，我们将学习原始数据类型。

正如我们在上一章中定义变量时所学到的那样，我们定义的变量保存属于特定类型的数据，例如字符串、布尔值或码点，或者它可能是数字类型之一，例如整数类型或浮点类型的系列。本章将详细介绍这些类型的特性以及如何使用它们。

本章我们将学习以下与原始类型相关的各种类型和概念：

- 原始数据类型
- 布尔数据类型
- 数字数据类型
- 数字数据类型的运算
- 字符串数据类型
- 字符数据类型
- 字符串数据类型的运算

通过本章的学习，你将了解各种原始数据类型，例如字符串、码点和包括整数和浮点类型的数字类型。你还将学习字符串操作技巧，了解有符号和无符号整数，以及这些数字范围所具有的最小和最大值。

### 技术要求

本章大部分代码可以通过访问V的REPL来运行，如第2章"安装V编程"中"访问V编程REPL"一节所述。建议为本章中的每个部分重新启动REPL，因为您可能会遇到相同变量名称的用法。

你可以在这里找到本章所有代码片段：https://github.com/ToughStyle/V-programming-book-cn/codes/Chapter04 。

你也可以将代码片段另存为.v扩展名的文件名，然后访问命令行终端以以下方式运行代码：
```
v run filename.v
```
## 介绍原始数据类型

原始数据类型是最纯粹的数据类型，不能表示为其他形式的数据的引用或衍生。V具有许多原始数据类型，例如布尔值、字符, 字符串和数字数据类型，例如整数、无符号整数和浮点类型。

在深入了解数据类型之前，我们将学习如何使用内置的`typeof()`函数确定任何变量的类型，如下所示：
```
typeof(variable).name
```
通过这个简短的原始数据类型介绍，让我们开始深入学习这些类型。我们将从学习布尔数据类型以及产生布尔结果的各种运算符开始，附上代码示例。

## 布尔数据类型

布尔是一种数据类型，用于表示两个可能值中的一个，即真或假。以下代码演示了声明布尔变量：
```
completed := true
```
从上述语句中，我们可以看到名为`completed`的布尔变量被赋值为`true`。请注意，语句右侧没有引号。声明布尔变量只需要将true或`false`之一分配给变量即可。

布尔数据类型用`bool`关键字表示。通常在定义函数的参数或返回类型，或者定义结构体或接口的字段时使用 `bool` 关键字。

### 逻辑运算符

逻辑运算符通常评估两个操作数，并产生布尔结果。逻辑运算符要求其操作数为布尔类型。

|运算符 |名称| 描述|
|----|----|----|
|&&| 逻辑 AND| 当两个操作数都为`true`时，评估为`true`。|
| \|\| | 逻辑 OR| 如果至少有一个操作数为`true`，则评估为`true`。|
|! | 逻辑 NOT| 这是一元运算符，当操作数为`false`时评估为`true`，反之亦然。|

表4.1 - 逻辑运算符

以下代码展示了在布尔变量上执行各种逻辑运算将得到一个布尔结果：
```
module main

fn main() {
    t := true
    f := false

    // 使用 && 操作符的逻辑 AND
    and_tt := t && t
    and_tf := t && f
    and_ft := f && t
    and_ff := f && f
    println('使用 && 操作符的逻辑 AND')
    println('$t && $t = $and_tt')
    println('$t && $f = $and_tf')
    println('$f && $t = $and_ft')
    println('$f && $f = $and_ff')
    println('')

    // 使用 || 操作符的逻辑 OR
    or_tt := t || t
    or_tf := t || f
    or_ft := f || t
    or_ff := f || f
    println('使用 || 操作符的逻辑 OR')
    println('$t || $t = $or_tt')
    println('$t || $f = $or_tf')
    println('$f || $t = $or_ft')
    println('$f || $f = $or_ff')
    println('')

    // 使用 ! 操作符的逻辑 not
    not_t := !t
    not_f := !f
    println('使用 ! 操作符的逻辑 not')
    println('!$t = $not_t')
    println('!$f = $not_f')
}
```
输出如下：
```
使用 && 操作符的逻辑 AND
true && true = true
true && false = false
false && true = false
false && false = false

使用 || 操作符的逻辑 OR
true || true = true
true || false = true
false || true = true
false || false = false

使用 ! 操作符的逻辑 not
!true = false
!false = true
```
### 关系运算符

我们了解了当操作数为布尔类型时进行逻辑运算，并且布尔操作的结果也为布尔类型。我们还可以执行生成指示布尔值的关系或不同数据类型的操作数的操作。我们可以使用关系运算符来实现。

使用关系运算符比较两个相同数据类型的变量将产生布尔数据类型的`true`或`false`。布尔字段保存的默认值为`false`。
下表展示了V语言中可用的关系运算符的列表：

| 运算符 | 名称 | 描述 |
| :---: | :---: | :--- |
| < | 小于 | 当左操作数的值小于语句右侧的操作数时，评估为true；否则评估为false。 |
| > | 大于 | 当右操作数的值大于语句左侧的操作数时，评估为true；否则评估为false。 |
| == | 等于 | 如果两个操作数相等，则评估为true；否则为false。 |
| != | 不等于 | 如果操作数不相等，则评估为true；否则为false。 |
| <= | 小于或等于 | 当左操作数的值小于或等于语句右侧的操作数时，评估为true。 |
| >= | 大于或等于 | 当左操作数的值大于或等于语句右侧的操作数时，评估为true。 |

表4.2 - 关系运算符

以下代码演示了在V语言中使用各种关系运算符：

```
module main

struct Note {
      id          int
      detail      string
      completed   bool
}

fn main() {
      mut n := Note{
            id:         1001,
            detail:     'get groceries',
      }

      println(n.completed) // 未赋值给布尔字段，将默认为false

      // 使用关系运算符>进行比较
      if n.id > 1000 { // 将一个整型类型的笔记id与另一个整型类型进行比较，评估结果为布尔型
            println('The note id is greater than 1000')
      } else {
            println('The note id is less than 1000')
      }

      // 使用关系运算符==进行比较
      if n.detail == 'get groceries' {
            println('The note details about groceries')
      }

      // 使用关系运算符!=进行比较
      if n.detail != 'get dairy products' {
            println('The note does not details about dairy products')
      }
}
```

这里是输出结果：

```
false
The note id is greater than 1000
The note details about groceries
The note does not details about dairy products
```

在此代码中，`Note`结构体被初始化，而未赋值给一个布尔值的`completed`字段，该值是一个布尔值字段，而`id`的值已经被分配。现在，如果我们运行代码，我们将看到默认值为`false`的`completed`布尔字段的值。

同样，在前面的代码中演示的两个相同数据类型的值的比较将产生一个布尔结果。在这里，`Note`结构体的`id`字段是`int`数据类型，并与整数值`1000`进行比较。由于`id`的值为`1001`，大于`1000`，因此大于>运算符的比较被评估为`true`，因此代码打印`The note id is indeed greater than 1000`文本。

接下来，让我们详细了解数字类型。

## 数字数据类型

在V语言中，数字类型或数值类型是一个原始的数字类型家族，例如整数和浮点类型。整数和浮点类型根据它们支持的区间进一步分类。让我们详细了解数字类型。

被分配一个整数值的变量将是默认数据类型`int`，表示32位整数：

```
x := 1
typeof(x).name // int
```

V支持分配带下划线`_`作为分隔符的数字。`_`只是为了可读性，不影响定义的数字值。

请考虑以下声明：

```
i := 1_000
j := 1000
println(i == j) // true
```
V语言允许使用十六进制表示法以0x开头、二进制表示法以0b开头、八进制表示法以0o开头来声明整数变量，如以下示例代码所示：

```
module main

fn demo() {
    h1 := 0x64         // 十六进制以 0x 开头
    b1 := 0b1100100    // 二进制以 0b 开头
    o1 := 0o144        // 八进制以 0o 开头

    println('Value of var h1 with hexadecimal value: $h1')
    println('Data type of var h1 with hexadecimal value : ${typeof(h1).name}')
    println('Value of var b1 with binary value: $b1')
    println('Data type of var b1 with binary value: ${typeof(b1).name}')
    println('Value of var o1 with octal value: $o1')
    println('Data type of var o1 with octal value: ${typeof(o1).name}')
}

fn main() {
    demo()
}
```

运行以上代码输出结果为：

```
Value of var h1 with hexadecimal value: 100
Data type of var h1 with hexadecimal value : int
Value of var b1 with binary value: 100
Data type of var b1 with binary value: int
Value of var o1 with octal value: 100
Data type of var o1 with octal value: int
```

从输出结果可以看到，无论使用十六进制、二进制、八进制的表示法，赋值后的变量都被视为整数类型。

### 有符号和无符号整数类型：

首先，我们有i8、i16、int(32位整数类型)和i64等有符号整数类型，以及byte(也就是u8)、u16、u32和u64等无符号整数类型。

有符号整数类型支持正数和负数范围，通常用于表示整数。无符号整数类型不表示任何符号，只表示非负数。下表表示了所有整数类型的范围：

| 类型   | 最小值                       | 最大值                         |
| ------ | ---------------------------- | ------------------------------ |
| `i8`     | -128                         | 127                            |
| `i16`    | -32768                       | 32767                          |
| `i32`    | -2147483648                  | 2147483647                     |
| `i64`    | -9223372036854775807 - 1     | 9223372036854775807            |
| `byte`   | 0                            | 255                            |
| `u16`    | 0                            | 65535                          |
| `u32`    | 0                            | 4294967295                     |
| `u64`    | 0                            | 18446744073709551615           |

从上表中可以看出，对于`类型，最小值表示为`，因为C编译器解析字面值时没有符号，并且`超过了`类型的最大值`。理论上，没有比`类型更大的类型可以容纳大于`值，因此我们将最小值表示为从`中减去`的计算结果。

### 浮点数类型：

浮点数类型用于表示具有小数部分的数字值。在V中，我们有两个浮点数类型，它们是`f32`和`f64`。

对于浮点数类型，我们根据这些类型所包含的最大和最小非零分数来测量其范围，如下表所示：

| 类型 | 最小非零数值 | 最大数值 |
| ---- | ------ | -------- |
| `f32`  | 1.401298464324817070923729583289916131280e-45                   | 3.402823466385288598117041834845169254e+38`                    |
| `f64`  | 4.940656458412465441765687928682213723651e-324                  | 1.797693134862315708145274237317043567e+308                   |

从上表中可以看出，在数学上，我们可以表示浮点数类型支持的最小值和最大值范围。

### 提升数值类型：

由于数值类型构成了一个由其支持的数值数据的范围和限制所定义的特殊数据类型家族，较小的类型可以轻松地提升或转换为较大的类型。理想情况下，它们的层次结构如下图所示：
```
   i8 → i16 → int → i64
                  ↘     ↘
                    f32 → f64
                  ↗     ↗
   u8 → u16 → u32 → u64 ⬎
      ↘     ↘     ↘      ptr
   i8 → i16 → int → i64 ⬏
```
以上图片来自官方 V 文档，表示了数据类型的可能提升路径，从左侧开始到右侧较大的类型，箭头表示它们适合数据类型范围的情况。
考虑以下代码来演示类型提升流程的显示:

```
module main
fn demo() {
    ia := i8(2)
    ib := i16(2)
    ic := int(2)

    println('----type definitions----')
    println('variable ia is of type:
            ${typeof(ia).name}')
    println('variable ib is of type:
            ${typeof(ib).name}')
    println('variable ic is of type:
            ${typeof(ic).name}')
    println('')

    iaa := ia + ia // i8 with i8 results i8
    ibb := ib + ib // i16 with i16 results i16
    icc := ic + ic // int with int results int

    println('----mixing types----')
    println('variable iaa is of type:
        ${typeof(iaa).name}, after adding type
        ${typeof(ia).name} with itself')
    println('variable ibb is of type:
        ${typeof(ibb).name}, after adding type
        ${typeof(ib).name} with itself')
    println('variable icc is of type:
        ${typeof(icc).name}, after adding type
        ${typeof(ic).name} with itself')
    println('')

    iab := ia + ib // i8 with i16 results in i16
    ibc := ib - ic // i16 with i32 results in i32

    println('----type promotion----')
    println('variable iab is promoted to type:
        ${typeof(iab).name}, after adding type
        ${typeof(ia).name} with ${typeof(ib).name}')
    println('variable ibc is promoted to type:
        ${typeof(ibc).name}, after subtracting type
        ${typeof(ib).name} with ${typeof(ic).name}')
    iba := ib / ia // the division of i16 and i8
                    // types
    println('Variable iba is promoted to the higher
            data type ${typeof(iba).name} which is
            carried from ib of type
            ${typeof(ib).name} divided from variable
            ia of type ${typeof(ia).name}')

    fa := f32(2)
    fa_iba := fa + iba // fa is type of f32 and iba
    // is of type i32
    
    println('Variable fa_iba is promoted to the
        higher data type ${typeof(fa_iba).name}
        which is carried from fa of type
        ${typeof(fa).name} when added with
        variable iba of type ${typeof(iba).name}')
}

fn main() {
    demo()
}
```


以下是输出:

```
----type definitions----

variable ia is of type: i8

variable ib is of type: i16

variable ic is of type: int

----mixing types----

variable iaa is of type: i8, after adding type i8 with itself

variable ibb is of type: i16, after adding type i16 with itself

variable icc is of type: int, after adding type int with itself

----type promotion----

variable iab is promoted to type: i16, after adding type i8 with i16

variable ibc is promoted to type: int, after subtracting type i16 with int

Variable iba is promoted to the higher data type i16 which is carried from ib of type i16 divided

Variable fa_iba is promoted to the higher data type f32 which is carried from fa of type f32 when added with variable iba of type i32
```
我们发现，当i8类型的变量与`i16`交互时，它会被提升为`i16`类型。此外，当您在数值数据类型`int`和`f64`之间执行数学操作时，得到的结果将是浮点类型`f64`。现在我们已经了解了数值数据类型，让我们来看看V中可用的运算符。

## 数字数据类型的运算

在任何软件程序开发过程中，都需要在某个时候进行数字处理。它可能涉及基本的数学计算或比较数字。V 通过提供可应用于原始类型的各种运算符，使您能够做到这一点。

可以应用于数字数据类型的运算符分为以下几类：

- 算术运算符
- 位运算符
- 移位运算符
- 关系运算符

因为我们在前面学习布尔数据类型时已经了解了关系运算符，所以我们将看一下其他算术、位和移位运算符，并理解如何使用这些运算符，以及使用V来进行示例代码。

### 1. 算术运算符

你可以在V中对数字类型执行基本算术运算，例如加法、减法、乘法、除法和取模。下表表示基本算术运算符并描述了这些运算符在处理数字数据类型时的作用：

| 运算符 | 名称 | 可应用的数据类型 | 描述 |
| ------ | --- | --------------- | ---- |
| +      | sum | integer, float, string | 将两个数字类型相加并返回总和。 |
| -      | difference | integer, float | 从一个数字类型中减去另一个数字类型，并在结果小于零时返回一个符号值。|
| *      | product | integer, float | 将两个数字类型相乘并返回乘积。|
| /      | quotient | integer, float | 将两个数字类型相除并返回商。|
| %      | remainder | integer | 将两个数字类型相除并返回余数。|

以下代码演示了算术运算符的使用：

```
module main

fn main() {
  a := 10
  b := 2
  
  // 使用 + 相加
  sum := a + b
  
  // 使用 - 相减
  diff := b - a
  
  // 使用 * 相乘
  prod := a * b
  
  // 使用 / 求商
  quotient := a / b
  
  // 使用 % 求余数
  remainder := a % b
  
  println('Sum of $a and $b is $sum')
  
  println('Subtracting $a from $b is $diff')
  
  println('Product of $a and $b is $prod')
  
  println('Quotient when $a divided by $b is $quotient')
  
  println('Remainder when $a divided by $b is $remainder')
}
```

以下是输出结果：

```
Sum of 10 and 2 is 12

Subtracting 10 from 2 is -8

Product of 10 and 2 is 20

Quotient when 10 divided by 2 is 5

Remainder when 10 divided by 2 is 0
```

### 2. 位运算符

在查看位运算符之前，让我们先看一下与二进制、位和字节相关的术语。

二进制名称说明了两个值的可能性。在二进制数字系统中，这两个值常被称为位。位表示为0或1。8个位组成一个字节。

正如我们看到的，V可用的整数类型为`i8`、`i16`、`int`(32位)和`i64`，它们表示这些数据类型可以容纳的信息位。在V中，默认的`int`是32位或4字节。最大整数类型为`i64`，它占8字节。为了验证这一点，V提供了一个内置的`sizeof`函数，它接收一个输入参数作为要检查大小的变量。`sizeof`函数返回表示正在检查大小的变量所占用的字节数的整数值。

下面的代码显示了`sizeof`函数的使用：

```
z := 345 // int是32位，因此变量z的大小为4个字节。
println(sizeof(z)) // 4
```

位运算操作只能在整数数据类型上执行。以下是在V中可对整数类型执行的位运算操作：

| 运算符 | 名称 | 描述 |
|------ | ----- | ---- |
| &     | 按位与 | 对两个整数执行按位与操作，返回一个整数值类型。|
| \|    | 按位或 | 对两个整数执行按位或操作，返回一个整数值类型。|
| ^     | 按位异或 | 对两个整数执行按位异或操作，并返回一个整数值类型。|
| ~     | 按位取反 | 对整数执行按位非运算，并返回一个整数值类型。|

下面的代码演示了各种位运算符的用法：

```
module main

fn main() {
  a := i8(6)
  b := i8(2)
  
  // 使用&运算符执行两个整数的按位与操作
  b_and := a & b
  
  // 使用| 运算符执行两个整数的按位或操作
  b_or := a | b
  
  // 使用^运算符执行两个整数的按位异或操作
  b_xor := a ^ b
  
  // 使用~运算符对整数进行按位取反运算
  not_a := ~a // Not运算的值等于-(a+1)
  
  println('Bitwise AND: $a & $b = $b_and')
  
  println('Bitwise OR: $a | $b = $b_or')
  
  println('Bitwise XOR: $a ^ $b = $b_xor')
  
  println('Bitwise NOT: ~$a = $not_a')
}
```

以下是输出:
```
Bitwise AND: 6 & 2 = 2

Bitwise OR: 6 | 2 = 6

Bitwise XOR: 6 ^ 2 = 4

Bitwise NOT: ~6 = -7
```
### 位运算符

在V语言中，整数数据类型的移位操作是逻辑性质的。移位运算符基于`<<`和`>>`运算符，作用于整数的比特分配，即向左或向右移动比特，要求用0填充移动后的位置。

这里，符号`<<`和`>>`分别代表左移运算符和右移运算符。

执行整数移位的语法如下：

`INTEGER << POSITIONS_TO_SHIFT`

前面语句的左侧需要是整数类型的值，后跟`<<`或`>>`，再接着需要指定在两个移位运算符之间所述方向上要左移或右移的位数。`<<`表示将位移到左侧，而`>>`表示将其移到右侧。移位语句的右侧需要是非负整数。因此，在编程时，建议在移位运算符的右侧指定无符号整数。

我们考虑下面的代码，演示了对8位整数的移位操作：
```
module main

fn main() {

    // 声明8位整数值为3
    a := i8(3)

    // 8位等于1字节
    println('a is ${sizeof(a)} byte(s)') // a is 1 byte(s)

    // 声明8位无符号整数，要左移1位
    pos := byte(1)

    // 向左移动值为3的数1位
    a_left_shift := a << pos

    println('${a} << ${pos} = ${a_left_shift}')

}
```
这里是输出结果：
```
a is 1 byte(s)
3 << 1 = 6
```
在上面的代码中，变量`a`具有分配值为3的`i8`类型，代表8位整数。在8位格式中，值3的表示如下：

|2^7 |2^6 |2^5 |2^4 |2^3 |2^2 |2^1 |2^0 |
|---|---|---|---|---|---|---|---|
|0 |0 |0 |0 |0 |0 |1 |1 |

图4.2-以8位格式表示数字3

在8位块中，其中位值为1的幂的值的总和等于3，如下所示：
```
0 * 27 +  0 * 26 + 0 * 25 + 0 * 24 + 0 * 23 + 0 * 22 + 1 * 21 + 1 * 20
1 * 21 + 1 * 20
1 * 2 + 1 * 1
2 + 1
3
```
我们定义了类型为`byte`的`pos`变量，并将它赋值为1，以指示要移动的位置数。然后，使用`<<`运算符对包含值为3的变量`a`执行左移操作。向左移动1个位置的位现在如下所示：

|2^7 |2^6 |2^5 |2^4 |2^3 |2^2 |2^1 |2^0 |
|---|---|---|---|---|---|---|---|
|0 |0 |0 |0 |0 |1 |1 |0 |

图4.3-通过左移1位后数字3变为6，以8位格式表示

在移位操作后，其中位值为1的幂的值的总和等于6，如下所示：
```
0 * 27 +  0 * 26 + 0 * 25 + 0 * 24 + 0 * 23 + 1 * 22 + 1 * 21 + 0 * 20
1 * 22 + 1 * 21
1 * 4 + 1 * 2
4 + 2
6
```
理解了移位运算符的工作原理后，让我们看下面的例子，展示了将初始类型为`i8`且值为1的整数进行左移，并在从0到7的移位位置上对其值进行评估：
```
module main

fn main() {
    val := i8(1)
    bits := sizeof(val) * 8
    println('Performing left shift using << Operator')

    for i in 0 .. bits {
        after_shift := val << i
        println('$val << $i = $after_shift
        \/\/ type after shift operation: 
        ${typeof(after_shift).name}')
    }
}
```
这里是输出结果：
```
Performing left shift using << Operator
1 << 0 = 1 // type after shift operation: i8
1 << 1 = 2 // type after shift operation: i8
1 << 2 = 4 // type after shift operation: i8
1 << 3 = 8 // type after shift operation: i8
1 << 4 = 16 // type after shift operation: i8
1 << 5 = 32 // type after shift operation: i8
1 << 6 = 64 // type after shift operation: i8
1 << 7 = -128 // type after shift operation: i8
```
注意，对于最后一次迭代，整数变量i8的值为1，当它向左移动7个位置时，值变为 `-128`。这是因为i8所包含的值的范围是`-128`到`127`。由于`i8`是一个有符号整数数据类型，它包含正数和负数，所以移位操作结果为`-128`。如果执行 `1 << 8`，则结果将为0，这最终将最左侧的最高位推出8位桶，导致所有位变为0。因此，值将评估为0。

现在我们已经学习了数字运算符，让我们来看看字符串数据类型。

## 字符串数据类型

字符串用于表示单词、短语或段落的文本。它可以容纳所有字母数字字符、特殊字符和符号。

声明字符串数据类型的变量的语法如下所示：

`<VARIABLE_NAME> := '<TEXT>'`

从前面的语法中，我们注意到 `:=` 符号左侧的变量名，以及由变量保存的值置于右侧。我们还注意到，变量保存的值用单引号(')括起来。V也允许您声明将值用双引号(")括起来的变量。

例如，请考虑以下代码片段：
```
h:= 'hello'
println(h) // hello
println(h.len) // 5
typeof(h).name // string
```
上面的代码演示了如何声明字符串变量以及如何使用名为len的默认字段，该字段表示字符串的长度。

### 使用字符串数据类型

需要了解有关字符串数据类型的某些属性以使用它。字符串数据类型具有以下属性：

- 字符串是只读字节数组。
- 字符串默认情况下是不可变的。
- 可以使用`mut`关键字声明可变字符串。
- 字符串的元素无法被突变。

让我们详细介绍字符串数据类型的每个属性。

### 字符串是只读字节数组

在V中，字符串是使用`struct`类型实现的，它具有两个带有`pub`关键字标记的字段`str`和`len`，其中`str`字段是具有默认值0的字节指针，`len`字段是`int`类型，表示字符串的长度。因此，在V中，字符串是只读字节数组。我们将在第8章"V中的Structs"中详细了解`struct`和`struct`字段。简而言之，在V中，字符串通常表示为只读字节数组。请考虑以下代码：
```
fruit:= 'Orange'
```
在这里，变量`fruit`被赋予`Orange`字符串，该字符串长度为6个字符。请考虑以下代码：
```
println(typeof(fruit[0]).name) // byte
println(fruit[0]) // 79
```
在V中，数组元素的索引从0开始。因此，作为V中的只读字节数组，如果我们在每个位置索引每个字符并检查其数据类型，它将返回`byte`。在每个位置保存的值返回字符的代码点表示。在这种情况下，大写字母O的十进制代码点为79。这表明V中的字符串数据采用UTF-8编码。

###  字符串默认情况下是不可变的

字符串在声明时默认为不可变的。这意味着一旦声明了字符串，就无法修改或更新它。让我们看以下示例：
```
s:= 'hello' //变量s是不可变的
s ='Hello!' //这会导致错误
```
在上面的代码段中，我们声明了一个名为s的变量，并将其赋值为字符串值`hello`。然后，当我们尝试使用`Hello!`更新`s`变量时，此操作将导致一个错误，指出s是不可变的，请使用`mut`使其可变。

因此，该错误具有描述性，并建议我们使用`mut`关键字声明变量为可变。让我们看看如何声明可变字符串并与它们一起使用。

###  声明可变字符串

您可以使用`mut`关键字声明可变字符串，并使用:= 分配变量的值，如下所示：
```
mut msg:='Hello Friend!'
要更改可变变量的值，请使用=而不是`:=`。现在我们定义了一个可变字符串，让我们尝试用其他内容替换`msg`的值，如下所示：

```
msg = 'Hope you are doing good.'
println(msg) // Hope you are doing good.
```

使用可变的`msg`变量，您还可以执行其他各种字符串操作，例如使用+将其与其他字符串连接起来更新`msg`变量，如下所示：

```
msg = msg + " There is a surprise for you."
println(msg) // Hope you are doing good. There is a surprise for you.
```

我们可以看到，通过连接`msg`和额外字符串的结果，我们已经更新了`msg`，使用连接运算符`+`。

### 字符串的元素无法被改变

正如我们所看到的，字符串的元素可以被索引，从位置0开始；然而，程序员有时候倾向于在特定位置更新字符串中字符的值。即使它们被声明为可变的，因为字符串是只读字节数组，所以这是不允许的：

```
mut greet := 'good Day'
greet[0] = 'G' // this results in error
```

如果我们尝试更新变量`greet`中小写字母`g`所在位置0的元素为大写字母`G`，它会抛出一个错误，指出字符串`s`是不可变的。请注意，变量可能是可变的，但是字符串不是。

在了解如何对字符串数据类型执行各种操作之前，我们将介绍V中的一种新原始类型称为`rune`。

## `rune`字符数据类型

在学习如何在V中使用符文之前，让我们看看为什么您可能需要使用符文类型。在UTF-16编码中，小于216的代码点使用16位代码单元进行编码，该代码单元等于代码点的数值。大于或等于216的较新的代码点通过两个16位代码单元的复合值进行编码。例如，西里尔字母小写版本`а́`是`U+0430`和`U+0301`的组合。在UTF-16中不使用此类值作为字符，并且没有办法将它们编码为个别代码点。为了解决这个问题，我们在V中有符文类型。使用符文类型，我们可以将复合代码点表示为范围在0到4294967295之间的单个整数值，如表4.3中为`u32`指定的那样。

因此，使用符文，它可以是任何`u32`值，包括代理代码点和不合法的Unicode代码点。

简而言之，字符字面量具有称为符文的特定数据类型。符文表示Unicode代码点。符文类型是V中无符号整数`u32`的别名。

用反引号( ` )括起来的值声明符文数据类型的变量，如下所示：

```
l := `a`
typeof(l).name // rune
```

通过将符文转换为字符串类型，您可以执行字符串操作，例如将符文类型与字符串连接，检查字符串是否包含符文等。请考虑以下代码：

```
beverage := 'café'
s := `é` //declare rune
beverage.count(s.str()) // 1
```

在上面的代码片段中，我们使用变量s声明了一个符文，并将其强制转换为字符串类型以计算`s`变量持有的值的出现次数。在此处，我们正在计算变量名为`beverage`的变量中的出现次数，在其中的值为`café`。现在，让我们了解可以对字符串数据类型执行的各种操作。

### 字符串数据类型上的操作

在编程时，我们经常需要操纵字符串的表示形式，无论是编写自定义输出结果还是操纵字符串以将其用于进一步评估。

### 字符串内插

字符串内插是表示字符串的方法，同时包含在运行时评估为其值并转换为字符串数据类型的不同类型的变量的混合。

### 基本数据类型的字符串内插

可以通过以下语法实现字符串内插：

```
println('SAMPLE TEXT $primitive-data-type')
```

前面的语法展示了如何使用 `$` 符号作为变量名的前缀来对基本数据类型进行字符串插值。考虑以下代码：
```
a := "coding"

b := "fun"

println('$a is $b')
```
这里是输出结果：
```
coding is fun
```
当访问结构体字段时，建议使用 `{` 和 `}` 双大括号将它们包裹起来，并加上 `$` 前缀符号，如下所示：
```
println('${STRUCT1.FIELD1} is ${STRUCT1.FIELD_2}')
```
上面的代码展示了如何使用字符串插值的方法来访问结构体字段。我们将在第8章"V中的结构体"中学习更多关于结构体和访问结构体字段的知识。

### 字符串操作技巧

我们将学习V中常用的字符串操作和插值技术。

### 转义特殊字符

虽然推荐在一个字符串变量的赋值值中用单引号( ' )将其包裹起来，但当值本身包含一个单引号时，这就变得很棘手了。例如，考虑以下句子：
```
It's my Daughter's birthday .
```
在这个句子中，单引号( ' )作为两个单词 `It's` 和 `Daughter's` 的一部分出现。为了声明这样的字符串，我们需要使用反斜杠 `\` ，一个向后的斜杠符号来转义引号，如下所示：
```
sen := 'It\'s my Daughter\'s birthday!'

println(sen)
```
这里是输出结果：
```
It's my Daughter's birthday!
```
我们注意到，包含有单引号( ' )内容的字符串被转义了，并作为值的一部分提供。

### 声明原始字符串

有时，字符串包含一个反斜杠符号( \ )作为分配给变量的文本的一部分。如果没有转义，`\` 将不会出现，并且有时会在反斜杠旁边出现 `n` 和 `t` 等字符时导致特殊行为。这意味着 `\n` 和 `\`t 具有特殊含义，分别表示新行和制表符空间。在这种情况下，您可以添加额外的反斜杠 `\\` ，或在单引号之前用小写字母 `r` 指示它为原始文本。以下代码将展示如何声明原始字符串以及声明和未声明原始字符串输出的差异：
```
I :='hi, \\how are you?'

println(i)
```
这里是输出结果：
```
hi, how are you?
```
注意到输出结果中缺少 `\` ，为了防止这种情况的发生，我们可以将其声明为原始字符串，如下所示：
```
I := r'hi, \how are you?'

println(i)
```
注意变量 `i` 的声明，其值以 ' 开始，表示将原始文本分配给变量。

这里是输出结果：
```
hi, \how are you/?
```
### 连接字符串

连接是将两个字符串合并在一起的过程。使用V允许您使用 `+` 运算符连接字符串：
```
a := "con"

b := "cat"

println(a + b) // concat
```
使用 + 进行字符串连接需要将要连接的文字文本直接形成字符串数据类型的字面量。以下类型的连接会导致错误：
```
i := 1

j := "man army"

println(i + j) // i 是 int 类型，j 是 string 类型，会抛出错误
```
上述连接将无法执行，因为 `i` 和 `j` 变量不是同一数据类型。i 是 `int` 类型，而 `j` 是 `string` 数据类型。`i + j` 将抛出一个错误，其中说明：
```
infix expr: cannot use string (right expression) as int .
```
但是，您可以使用字符串插值技术实现不同数据类型的变量连接，从而生成字符串字面量。代码将有效地编写如下：
```
i := 1

j := "man army"

println('${i} ${j}')
```
这里是输出结果：
```
1 man army
```
上面的代码片段演示了不同数据类型的变量连接，它使用了字符串插值技术，生成字符串类型。

### 从字符串字面量中提取子字符串

可以通过 `substr` 函数来操作一个字符串，从中提取出一部分。`substr` 函数接受两个输入参数。第一个参数是 `int` 类型，表示字符串中的起始位置。第二个参数也是 `int` 类型，表示结束位置，但不包括它本身。

考虑以下代码片段：
```
a :='Came'

b := a.substr(0,3)

println(b) // Cam
```
截取子串的用法示例:

在这段代码中，我们演示了如何使用子串，从单词"Camel"中取出从起始索引0到索引3结尾的子串，并将其赋值给变量`b`。

现在，变量`b`将保持值"Cam"。

### 字符串分割：

要对字符串进行分割，我们使用`split`函数，它接受一个字符串类型的单个输入参数，该参数表示要使用的分隔符值。分割的结果将返回一个字符串数组，每个元素都是根据提供的分隔符进行拆分操作的结果:

```
sp :='The tiny tiger tied the tie tighter to its tai'

res := sp.split(' ') // 使用空格作为分隔符进行分割

println(typeof(res).name) // []string

println(res) // ['Th','tin','tige','tie','th','ti','tighte','t','it','tai']
```
从上面的代码可以看出，当使用空格作为分隔符对sp字符串变量进行分割时，结果是一个字符串数组，其中每个元素都表示该句子的一个单词。

### 将字符串转换为字符数组：

我们已经知道，在V语言中，字符串是使用一个结构体来实现的，并且它具有一个`runes`方法，该方法返回一个数组，其中每个元素都是`rune`类型。您将在第8章结构体中的定义接收器函数部分了解更多信息。

考虑以下代码：

```
doge_moon := "` `+` `=` `"

doge_moon_runes := doge_moon.runes()

println(doge_moon_runes)
```

在上面的代码中，我们将`doge_moon`声明为字符串类型的变量，并调用`.runes()`函数。我们使用`doge_moon`上的`.runes()`的结果来捕获一个变量`doge_moon_runes`，然后将其输出到控制台。上面的代码给出以下输出：

```
[` `+` `= `]
```

从输出中可以看出，数组的每个元素都用反引号(\`)括起来，因此它表示结果数组的类型是`[]rune`。我们还可以使用以下代码检查`doge_moon_runes`的类型：

```
println(typeof(doge_moon_runes).name) // []rune
```

### 统计字符串中子串的出现次数：

`count`函数用于识别特定字符或字符序列在给定字符串中的存在。`count`需要一个字符串数据类型的参数，并返回出现次数作为`int`数据类型。如果未找到匹配项，`count`将返回0.

考虑以下示例：

```
sp := 'The tiny tiger tied the tie tighter to its tail'

println(sp.count('t'))        // 10

println(sp.count('T'))         // 1

println(sp.count('tie'))     // 2

println(sp.count('-'))         // 0
```

以上代码演示了在`sp`字符串变量上使用`count`的用法。然后，我们计算小写字母"t"、大写字母"T"、文字"tie"和符号"-"的出现次数，其结果分别为10、1、2和0。

值得注意的是，`count`方法区分大小写，并为"t"和"T"提供不同的计数。

### 使用contains检查字符串是否存在：

我们可以使用`contains`检查给定字符串中是否存在子串。`contains`函数接受一个字符串类型的输入参数，并评估提供的输入是否存在，并返回`bool`类型的`true`或`false`。如果在给定字符串中找到所提供的子字符串，`contains`将评估为`true`，否则它将评估为`false`。

以下代码演示了`contains`的用法：

```
module main

fn main() {

    hs := 'monday'

    if hs.contains('mon') {

        println('$hs contains mon')

    } else {

        println('$hs does not contains mon')

    }

}
```

输出如下：

```
monday contains mon
```

从上面的代码可以看出，我们正在检查`monday`中是否存在子串"mon"，这将评估为`true`，因此它打印出`monday contains mon`的输出。

现在，让我们略微更改代码，并将变量的值更新为`Monday`，其中大写字母`M`，而不是`monday`，并尝试运行以下代码：

```
module main

fn main() {

    hs := 'Monday'

    if hs.contains('mon') {

        println('$hs contains mon')

    } else {

        println('$hs does not contains mon')

    }

}
```

以下是输出结果：

```
Monday does not contains mon
```

很明显，`contains`操作是区分大小写的，而且在这种情况下，它评估为`if`条件的`else`部分，打印出结果文字`Monday does not contains mon`。

## 总结

在本章中，我们介绍了各种原始数据类型，如布尔型、数字类型(如整数和浮点数)、字符串和符文。我们还展示了各种操作，如布尔类型上的逻辑和关系操作。然后我们介绍了数字类型，学习了在数字类型上执行操作的方法，包括算术、位和移位操作，以及详细的例子。

在本章后面的部分，我们学习了字符串数据类型，并理解了与字符串相关的各种概念，包括字符串的可变性和不可变性。我们还介绍了符文数据类型以及示例。在本章的最后一部分，我们看到了如何执行各种操作，如字符串插值和不同的技术来操作字符串类型。

在了解V的基本类型之后，我们将在下一章中学习如何处理复杂类型，例如数组和映射。
