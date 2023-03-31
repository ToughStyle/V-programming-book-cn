# 第13章 JSON和ORM简介

在本章中，我们将学习如何使用V内置库，即`json`和`orm`。构建RESTful API等Web服务时，了解与客户端或其他RESTful API交换的内容类型至关重要。JavaScript对象表示法(JSON)已成为现代应用程序交换数据的首选格式。在本章中，我们将简要介绍JSON以及如何在V中使用它。

在构建面向数据的应用程序时，对象关系映射器(ORM)成为建立对象世界和关系型数据库世界之间通信的重要组成部分。在本章中，我们将介绍随V安装程序一起提供的内置库`orm`。

在本章中，我们将学习以下内容：

- 使用JSON入门
- 使用ORM入门

在本章的最后，你将学习内置的json和orm V库。了解这些库将帮助你编写数据驱动的Web API或微服务，以JSON格式交换数据。

### 技术要求

虽然不强制要求，但最好已掌握SQL知识。本章的完整源代码可在 https://github.com/ToughStyle/V-programming-book-cn/codes/Chapter13 上找到。

## 使用JSON入门

JSON是应用程序之间通信数据的最常用格式，例如HTTP API或Web应用程序。其他数据格式包括XML，CSV，TSV和文本文件等。

为了进行演示，请考虑以下`Note`结构体：
```v
struct Note {

    id      int

    message string

    status  bool

}
```
我们可以将这个`Note`结构体表示为JSON格式，如下所示：
```
{

    "id": 1,

    "message": "Plan a holiday",

    "status": false

}
```
在上面的代码中，JSON对象的表示易于阅读和理解：

- JSON对象通常以`{`(左花括号)开头，并以`}`(右花括号)结尾。

- JSON对象具有各种属性，表示为键值对，由`:`(冒号)分隔。

- 键始终用`""`(双引号)括起来。

- 值可以是任何数据类型，例如字符串，整数，浮点数，布尔值，数组或任何其他JSON对象。

一组JSON对象由逗号分隔，并且所有这些对象都包含在`[`和`]`内。例如，我们可以将多个`Note`对象表示为JSON数组，如下所示：
```
[

    {

        "id": 1,

        "message": "Plan a holiday",

        "status": false

    },

    {

        "id": 2,

        "message": "Get groceries",

        "status": false

    }

]
```
如上例所示，JSON对象的文件以`.json`扩展名结尾。这些文件称为JSON文件。

V具有名为`json`的第三方库，用于编码和解码JSON。要处理JSON数据，我们需要在编写编码或解码JSON的代码中导入json库。在以下各节中，我们将看如何在V中解码和编码JSON。

### 解码JSON对象

JSON解码是将JSON数据转换为V中的对象的过程。

`decode`函数接受两个参数。第一个参数是需要将JSON解码为的类型。第二个参数是表示为字符串的JSON数据。

`decode`函数具有可选的返回类型。因此，如果提供了不正确的输入，我们可以使用`or {}`块处理错误。在成功解码的情况下，它返回一个对象，其类型等效于作为`decode`函数输入参数提供的第一个参数。以下示例显示解码JSON对象：
```v
import json

fn main() {

    n := json.decode(Note, '{"id":1,"message":"Plan a

         holiday","status":false}') or {

        panic('invalid json data')

    }

    println(typeof(n).name) // Note

    println(n)

}
```
为简洁起见，在上面的代码中没有指定`Note`结构体。我们可以看到，要解码JSON字符串，我们必须使用`import json`语句，并使用两个输入参数调用解码函数。在此示例中，我们要将JSON字符串解码为`Note`类型。我们还可以看到`decode`函数后面跟着一个`or`块，并且我们正在处理不正确JSON数据的情况。

上面代码的输出结果如下：
```v
Note

Note{

    id: 1

    message: 'Plan a holiday'

    status: false

}
```
如果JSON字符串格式错误，如下面的代码所示，则将在控制台上打印错误消息：
```v
n := json.decode(Note, '{"id":1,"message":"Plan a holiday","status":false') or {

    panic('invalid json data')

}
```
在上述代码中，JSON字符串末尾缺少`}`。因此，在这种情况下，程序将使用错误消息`invalid json data`抛出异常。

接下来，我们将学习如何将V中的对象编码为JSON数据。

### 将对象编码为JSON数据

将对象编码为JSON是将V中的对象转换为JSON字符串的过程。`encode`函数接受一个输入参数，并返回表示JSON数据的字符串。

以下代码演示了如何使用`encode`方法：
```v
import json

fn main() {

    m := Note{

        id: 2

        message: 'Get groceries'

        status: false

    }

    j := json.encode(m)

    println(j)

}
```
在上述代码中，我们正在对`Note`结构体进行编码，并将输出打印到控制台上，输出结果如下：
```
{"id":2,"message":"Get groceries","status":false}
```
## 使用ORM入门

### 理解ORM属性

有一些属性可用于装饰结构体及其字段，使其类似于关系型数据库中的表格模式。属性必须包含在方括号`[`和`]`中。这些属性如下表所示：

### 属性装饰符 用途

`[table: 'TABLE_NAME'] Struct TABLE_NAME` 可以是正在设计的表的任何描述性名称。如果未指定此属性，则认为结构体名称是表名。

`[primary] Field` 将字段设置为主键。

`[sql: 'COLUMN_NAME'] Field` 设置表中的列名。

`[sql: COLUMN_TYPE] Field` 设置表中列的类型，常见类型为`int`，`string`，`bool`以及称为`serial`的特殊类型，用于表示自动递增。serial类型通常与主键一起使用。

`[unique] Field` 将字段设置为具有在一组行中具有唯一值的字段。

`[unique: 'GROUP_NAME'] Field GROUP_NAME` 可以是任何组名。它使用提供的名称将该字段添加到唯一组中。
### `[nonull]`属性

`[nonull]`属性是V语言中一个用于属性装饰的属性，它指定必须为字段提供一个值或使用基于类型的默认零值。

在使用V的ORM编写针对表的查询时，即使使用`[table:'TABLE_NAME']`属性修饰了结构体，语法也必须引用结构体的名称。对于结构体的字段来说，同样适用，不管是否使用`[sql:'COLUMN_NAME']`属性将其装饰为不同的列名。

### 创建ORM结构体

已经了解了V的ORM属性，现在我们将创建一个简单的结构体，类似于关系型数据库中的表格映射。

考虑以下`Note`结构体，我们想将其与数据库中的Notes表关联起来：
```v
[table:'Notes']
struct Note {
    id      int    [primary; sql: serial]
    message string [sql: 'detail'; unique]
    status  bool   [nonull]
}
```
在上面的代码中，可以看到`table`属性已将表名指定为`Notes`。但是，在操作`Notes`表上的数据时，我们将引用`Note`结构体的名称而不是数据库中表名。

您还可以看到`id`字段使用两个属性`primary`和`sql：serial`进行了指定，并用`;`分隔。`message`字段被标记为`unique`。此外，我们希望`message`字段有一个不同的名称，例如`detail`，作为数据库中相应的列名。因此，我们在`message`字段上指定了`sql:'detail'`作为附加属性。`status`字段使用`nonull`属性进行修饰，表示该注释是否已完成。

现在，我们将了解如何使用V的ORM建立连接并执行数据定义语言(DDL)操作，例如使用V的ORM创建和删除数据表。

### 使用ORM库

尽管ORM是V中的内置库，但我们只是使用其API与数据库进行通信。要使用ORM库，必须按照以下步骤操作：

1. 将SQLite作为第三方库安装到V的安装目录中
2. 设置使用SQLite库的ORM项目
3. 建立与数据库的连接

在以下子部分中，我们将详细介绍这些步骤。

将SQLite作为第三方库安装到V的安装目录中

为了与数据库建立通信，我们将在V的安装目录中将SQLite安装为第三方库。

为此，请转到 https://sqlite.org/download.html ，并下载SQLite的源代码。下载完成后，您将看到档案。提取档案并将文件放置在V安装目录下的`~\v\thirdparty\sqlite`目录中。这是Windows 10操作系统上的样子：
```
C:\V\THIRDPARTY\SQLITE

    shell.c

    sqlite3.c

    sqlite3.h

    sqlite3ext.h
```
如果您在Ubuntu操作系统上，则需要运行以下命令作为最终步骤来完成SQLite安装：
```
sudo apt-get install libssl-dev

sudo apt-get install libsqlite3-dev
```
现在，我们已将SQLite作为第三方库安装到V的安装目录中。下一节中，我们将创建一个项目，以使用SQLite来学习ORM。

### 设置使用SQLite库的ORM项目

让我们使用以下命令在命令提示符中创建一个名为`orm_demo`的新项目：
```
v new orm_demo

cd orm_demo
```
运行`v new orm_demo`命令后，您将被提示提供描述、版本和许可证信息(可选)。然后，您可以`按Enter`键。

我们的`orm_demo`项目将如下所示：
```
C:\ORM_DEMO

    .gitignore

    orm_demo.v

    v.mod
```
现在，将当前目录设置为我们刚刚创建的新项目的位置。随着我们在本节的相关主题中逐步进行修改项目`gradually`。因此，如果要查看每个更改的输出，可以使用以下命令从命令提示符运行该项目：
```
v run .
```
由于它是一个新项目，所以您将看到`Hello World！`的输出或类似于应用程序的`main`函数中的`print`语句的输出。现在，让我们继续学习如何使用刚刚安装的SQLite第三方库连接到数据库。
### 建立与数据库的连接

要连接到数据库，我们需要导入sqlite库，然后使用`connect`方法，该方法在导入sqlite后将可用。

现在，让我们开始编辑`orm_demo.v`文件。替换`orm_demo.v`文件的内容，以使其如下所示：
```v
module main

import sqlite

[table: 'Notes']
struct Note {

    id      int    [primary; sql: serial]

    message string [sql: 'detail'; unique]

    status  bool   [nonull]

}

fn main() {

    db := sqlite.connect('NotesDB.db') or { panic(err) }

}
```
由于我们将SQLite安装为第三方库，因此我们将导入它以建立与SQLite数据库的连接。因此，在上面的代码中，我们可以看到`import sqlite`语句。在导入后，我们将sqlite数据库连接实例分配给主函数内的db变量。

现在，我们将学习使用V的ORM执行各种数据库操作。

### 简要了解使用V的ORM的数据库操作

在本节中，我们将了解各种数据库操作。这些操作包括DDL操作(例如创建和删除表)。我们还将学习如何使用V的ORM执行数据操作(DML)操作，例如插入、`select`、`update`和`delete`。

请注意，上述DDL和DML不构成完整列表。为简洁起见，我们将只关注一组有限的DDL和DML操作，这些操作将帮助我们构建微服务。

### 使用V的ORM执行DDL操作

在本节中，我们将学习如何使用V的ORM执行基本和最常见的DDL操作，称为创建和删除。

### 创建表格

由于我们已经使用V定义了`Notes`表的外形，如`Note`结构体所示，因此我们将修改`main`函数以使用以下代码创建表格：
```v
fn main() {

    db := sqlite.connect('NotesDB.db') or { panic(err) }

    db.exec('drop table if exists Notes')

    sql db {

        create table Note

    }

}
```
从上面的代码中，我们确保使用提供给db连接的`exec`函数的普通SQL语法字符串参数删除`Notes`表，如果该表已存在。这表示您可以直接使用db.exec命令运行SQL查询。在此命令之后，我们正在执行一个表格创建操作，使用V语法执行。`create table Note`操作将生成以下等效的SQL语法：
```sql
CREATE TABLE IF NOT EXISTS `Notes` (
    `id` INTEGER, 
    `detail` TEXT, 
    `status` INTEGER NOT NULL, 
    PRIMARY KEY(id)
)
```
接下来，我们将学习如何执行删除表格操作。

### 删除表格

在上面的代码中，我们看到如何通过调用使用`db.exec`的基于SQL的命令直接删除表格。或者，您可以使用V的ORM语法删除表格，如下所示：
```v
sql db {

    drop table Note

}
```
现在，让我们学习如何执行DML操作。

### 使用V的ORM执行DML操作

在本节中，我们将学习如何在Notes表上执行基本的DML操作。我们将要学习的V中的ORM语句可以放置在`main`函数的`create table`语句之后，以我们即将学习的顺序。为了减少重复的代码，我们将只关注以下各节中ORM的DML语句。

### 插入记录

要执行插入操作，我们将构建一对Note结构类型的对象，如下所示：
```v
n1 := Note{

    message: 'Get some milk'

    status: false

}

n2 := Note{

    message: 'Get groceries'

    status: false

}
```
从上面的代码中，我们可以看到n1和n2对象是Note类型的。现在，我们将使用V的ORM语法执行插入操作，如下所示：
```v
sql db {

    insert OBJECT_VAR into STRUCT_NAME

}
```
从上述语法中，`OBJECT_VAR`一词指的是`STRUCT_NAME`类型的对象。 `STRUCT_NAME`一词将是`Note`结构体的名称。由于我们有两个`Note`对象`n1`和`n2`，因此我们将对这两个对象执行插入操作，如下所示：

```v
sql db {

    insert n1 into Note

    insert n2 into Note

}
```

V的ORM为`Notes`表格执行插入操作生成的相应SQL代码如下：

```sql
INSERT INTO `Notes` (`detail`, `status`) VALUES (?1, ?2);
```

### 注意：

前面代码中的SQL语句不会在控制台上打印出来，但它们是V ORM内部生成的。我提到它们是为了方便理解。

要获取ORM已执行任何DML操作的记录的主键标识符，我们可以通过调用last_id()函数访问它，如下所示：

```v
println(db.last_id())
```

该函数返回格式为`orm.Primitive(<VALUE>)`的值。 `last_id()`函数，可以在数据库连接上访问，其类型为`Primitive`，可以通过将其强制转换为相应的基本数据类型(包括`time.Time`类型)，来访问 `VALUE`。

由于`Note`中的id主键是整数类型，我们可以将其转换为`int`类型，如下所示：

```v
println(db.last_id() as int)
```

前面的打印语句将`last_id()`转换为`int`数据类型。

### 选择记录

现在，我们已经成功地将记录插入了数据库，我们将查询数据库并使用`select`语句验证记录。以下语法显示如何使用V的ORM编写select查询：

```v
ROWS_VAR := sql db {

    select from STRUCT_NAME

}
```

在前面的语法中，选择操作返回与结构类型相对应的数组。我们将使用V的ORM在`Notes`表格上执行`select`操作，如下所示：

```v
all_notes := sql db {

    select from Note

}
```

V ORM作为上一段代码的一部分生成的SQL语句如下：

```sql
SELECT `id`, `detail`, `status` FROM `Notes` ORDER BY `id` ASC;
```

从上面的SQL语句中，我们可以看到`select`语句隐式指定了表的所有列。我们还可以看到，在我们的情况下，默认应用于主键(即`id`)的升序排序。

现在，让我们使用以下代码打印all_notes变量及其类型：

```v
println(all_notes)

println('Type of all_notes is : ${typeof(all_notes).name}')
```

前面代码的输出如下所示：

```
[Note{

    id: 1

    message: 'Get some milk'

    status: false

}, Note{

    id: 2

    message: 'Get groceries'

    status: false

}]

Type of all_notes is : []Note
```

在这里，我们可以看到使用ORM时选择语句的结果是`[]Note`类型。现在，让我们学习如何使用`order by`子句排序结果。

### 使用`order by`子句进行选择

在前面的代码中，我们学习到，使用ORM默认情况下，结果按`id`的升序排列，即`Notes`表的主键列。如果我们希望结果在`id`列上按降序排列，则必须编写以下代码：

```v
notes_sorted := sql db {

    select from Note order by id desc

}
```

V的ORM作为前面查询的一部分生成的SQL语句如下所示：

```sql
SELECT `id`, `detail`, `status` FROM `Notes` ORDER BY `id` DESC;
```

从前面的输出中，我们可以看到结果按`Notes`表的`id`列的降序排序。现在，让我们使用`limit`子句选择记录。

### 使用`limit`子句进行选择

`limit`子句指定要作为查询执行结果返回的记录数。您可以将`limit`与`order by`或`where`子句一起使用。我们将在下一节中学习`where`子句。假设您想要检索最后一个插入`Notes`表格的记录。为此，我们可以使用以下代码：

```v
notes_limited := sql db {

    select from Note order by id desc limit 1

}
```
在前面的代码中，我们将我们的`select`语句限制为仅返回一条记录。这可以从`limit 1`中看出，它成为整个`select`语句的一部分。

作为前一个查询的一部分生成的V ORM生成的SQL语句将如下所示：

```sql
SELECT `id`, `detail`, `status` FROM `Notes` ORDER BY `id` DESC LIMIT ?1;
```

在这里，我们可以看到ORM生成的SQL根据`id`以降序排序注释，并将结果限制为只有1条。

现在，让我们使用以下代码打印`notes_limited`的值和类型：

```v
println(notes_limited)

println('Type returned by select when limit is 1: 
${typeof(notes_limited).name}')
```

前面代码的输出如下所示：

```
Note{

    id: 2

    message: 'Get groceries'

    status: false

}

Type returned by select when limit is 1:  Note
```

从前面的输出中，我们可以看到，选择语句仅返回一个记录，正如预期的那样。我们还可以看到，具有值1的限制子句自动导致值为`Note`类型的值，而不是其他选择语句中获得的`[]Note`。

### 使用`where`子句进行选择

`where`子句用于基于条件语句过滤记录数量。我们将使用`select`语句，连同`where`子句，后跟条件语句。这些条件用于过滤与某些列值匹配的记录。以下代码显示使用>关系运算符根据`id`过滤选择的行：

```v
notes_latest := sql db {

    select from Note where id > 1

}
```

作为前面查询的一部分生成的V ORM生成的SQL语句将如下所示：

```sql
SELECT `id`, `detail`, `status` FROM `Notes` 
WHERE `id` > ?1 ORDER BY `id` ASC;
```

在这里，由ORM生成的SQL根据`id > 1`条件过滤记录。如果我们打印`notes_latest`，我们将看到以下输出：

```v
[Note{

    id: 2

    message: 'Get groceries'

    status: false

}]
```

以下是关于V的ORM中`where`子句的几个注意点：

- `where`子句可以与`select`、`update`或`delete`一起使用。
- `where`子句的条件语句使用关系运算符，该运算符求值为布尔结果。
- 您可以使用逻辑运算符(即`&&`或`||`)将多个条件组合在一起。
- 可以将`where`子句与`order by`和`limit`子句中的一个或两个组合使用。在这种情况下，`where`子句出现在语句的第一部分，然后是`order by`，然后是`limit`。

### 更新记录

要更新Notes表格中的任何记录，我们可以使用`update`。使用V的ORM，以下代码将`status`设置为`true`，以用于`id`为2的`Notes`表格：

```v
sql db {

    update Note set status = true where id == 2

}
```

作为前面的查询的一部分生成的V ORM生成的SQL语句将如下所示：

```sql
UPDATE `Notes` SET `status` = ?1 WHERE `id` = ?2;
```

要查看更新后的记录，我们可以进行以下查询：

```v
notes_updated := sql db {

    select from Note where id == 2

}

println(notes_updated)
```

前面的代码将生成以下输出，我们可以看到状态为`id`为2的`Note`已设置为`true`：

```v
Note{

    id: 2

    message: 'Get groceries'

    status: true

}
```

从前一个输出中，除了将`status`字段更新为`true`之外，V ORM还自动返回单个记录，而不是只有一个与`where`子句持有的`id == 2`条件匹配的记录`[]Note`。

### 删除记录

删除操作从数据库中删除记录。要根据条件删除记录，我们可以使用`where`子句。以下代码显示如何删除其`id`为2的记录：

```v
sql db {

    delete from Note where id == 2

}
```

作为前面的查询的一部分生成的V ORM生成的SQL语句将如下所示：

```sql
DELETE FROM `Notes` WHERE `id` = ?1;
```
现在，让我们通过运行ORM选择查询来查看表中剩余的记录，如下所示：

```v
notes_leftover := sql db {

    select from Note

}

println(notes_leftover)
```

上述代码的输出将如下所示：

```
[Note{

    id: 1

    message: 'Get some milk'

    status: false

}]
```

在对状态标记为`true`的`Note`执行删除操作之后，我们只剩下了一条`Note`，如上面的输出所示。

## 总结

在本章中，我们简要介绍了JSON和ORM。然后我们详细学习了在V中使用`json`和`orm`库的方法。在本章的前几节中，我们学习了如何使用json库执行JSON编码和解码。在本章后面的部分中，我们重点介绍了内置的`orm`库。我们学习了如何使用`orm`库，并设置了一些要求，例如手动安装第三方`sqlite`库以与SQLite数据库一起工作。然后，我们看到了如何使用`orm`库在V中执行DDL和DML操作。本章为您编写数据驱动的Web API使用`orm`和`json`库提供了基础。在本章中获得的ORM知识将帮助您访问和与数据库交互。此外，在V中使用JSON数据格式的知识将帮助您在各种基于Web的API或服务之间交换数据。

在接下来的章节中，我们将学习如何使用`vweb`、`orm`、`json`和`sqlite`库构建微服务。

