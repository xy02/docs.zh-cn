---
title: F# 中的函数编程简介
description: 了解中F#函数编程的基础知识。
ms.date: 10/29/2018
ms.openlocfilehash: e1a0edc61dbe13012c48e166d490e22ebc70d6a0
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424701"
---
# <a name="introduction-to-functional-programming-in-f"></a>F 中的函数编程简介\#

函数编程是一种编程风格，它强调函数和不可变数据的使用。 类型化函数编程是指将函数编程与静态类型（如）结合F#在一起。 一般情况下，函数编程中强调了以下概念：

* 用作你使用的主构造
* 表达式而不是语句
* 变量上的不可变值
* 通过命令式编程进行声明性编程

在此系列中，你将使用F#在功能编程中探索概念和模式。 在此过程中，您还将F#了解某些情况。

## <a name="terminology"></a>术语

函数编程与其他编程范例一样，附带了一个最终需要学习的词汇。 下面是你将看到的一些常见术语：

* **函数**-函数是在给定输入时将生成输出的构造。 更正式地说，它将一个项从一个集_映射_到另一个集。 这种形式以许多方式提升为具体，尤其是在使用对数据集合进行操作的函数时。 它是函数编程中最基本的概念。
* **Expression** -表达式是代码中生成值的构造。 在F#中，此值必须绑定或显式忽略。 表达式可以替换为函数调用完全。
* **纯度**-纯度是函数的一个属性，它的返回值对于相同的参数总是相同的，并且其计算没有任何负面影响。 纯函数完全取决于其参数。
* **引用透明度**-引用透明度是一种表达式属性，以便可以将其替换为其输出，而不会影响程序的行为。
* 不**可变性意味着**值不能就地更改。 这与变量不同，后者可以就地更改。

## <a name="examples"></a>示例

下面的示例演示这些核心概念。

### <a name="functions"></a>函数

函数编程中最常见和最基本的构造是函数。 下面是一个简单的函数，它将1添加到整数：

```fsharp
let addOne x = x + 1
```

其类型签名如下所示：

```fsharp
val addOne: x:int -> int
```

签名可以读取为，"`addOne` 接受名为 `x` 的 `int` 并生成 `int`"。 更正式地说，`addOne` 是将一个值从一组整数_映射_到整数集。 `->` 标记表示此映射。 在F#中，您通常可以查看函数签名，以了解它的作用。

那么，为什么签名很重要呢？ 在类型化函数编程中，函数的实现通常没有实际的类型签名重要！ 在运行时，`addOne` 将值1添加到整数很有趣，但当你构造程序时，它接受并返回 `int` 的事实就是通知你将如何实际使用此函数。 此外，一旦您正确使用此函数（即其类型签名），诊断任何问题只能在 `addOne` 函数的主体中完成。 这是类型化函数编程的推动力。

### <a name="expressions"></a>表达式

表达式是计算结果为值的构造。 与执行操作的语句不同，表达式可以被认为是执行返回值的操作。 表达式几乎始终用于支持函数编程中的语句。

请考虑前面的函数 `addOne`。 `addOne` 的主体是一个表达式：

```fsharp
// 'x + 1' is an expression!
let addOne x = x + 1
```

这是定义 `addOne` 函数的结果类型的表达式的结果。 例如，构成此函数的表达式可以更改为其他类型，例如 `string`：

```fsharp
let addOne x = x.ToString() + "1"
```

函数的签名现在为：

```fsharp
val addOne: x:'a -> string
```

由于中F#的任何类型都可以对其调用 `ToString()`，因此 `x` 的类型已变为泛型（称为[自动通用化](../language-reference/generics/automatic-generalization.md)），并且生成的类型为 `string`。

表达式不仅仅是函数的主体。 您可以使用表达式来生成在其他地方使用的值。 一个常见的 `if`：

```fsharp
// Checks if 'x' is odd by using the mod operator
let isOdd x = x % 2 <> 0

let addOneIfOdd input =
    let result =
        if isOdd input then
            input + 1
        else
            input

    result
```

`if` 表达式将生成一个名为 `result`的值。 请注意，您可以完全省略 `result`，使 `if` 表达式成为 `addOneIfOdd` 函数的主体。 需要记住的重要一点是，它们会生成一个值。

有一种特殊类型，`unit`，当没有要返回的内容时使用。 例如，请看下面这个简单的函数：

```fsharp
let printString (str: string) =
    printfn "String is: %s" str
```

签名如下所示：

```fsharp
val printString: str:string -> unit
```

`unit` 类型指示没有返回实际值。 这在具有必须 "完成工作" 的例程时非常有用，因为这样做不会因工作而返回值。

这与命令式编程非常鲜明，其中等效 `if` 构造是一个语句，生成值通常是通过转变变量来完成的。 例如，在中C#，代码可能如下所示：

```csharp
bool IsOdd(int x) => x % 2 != 0;

int AddOneIfOdd(int input)
{
    var result = input;

    if (IsOdd(input))
    {
        result = input + 1;
    }

    return result;
}
```

值得注意的是， C#和其他 C 样式语言都支持[三元表达式](../../csharp/language-reference/operators/conditional-operator.md)，这允许基于表达式的条件编程。

在函数编程中，很少用语句改变值。 尽管某些功能语言支持语句和转变，但在函数编程中使用这些概念并不常见。

### <a name="pure-functions"></a>纯函数

如前文所述，纯函数是函数：

* 对于同一输入，始终计算为相同的值。
* 没有副作用。

在此上下文中，将数学函数视为非常有用。 在数学中，函数仅依赖于其自变量，并且没有任何副作用。 在数学函数 `f(x) = x + 1`中，`f(x)` 的值仅依赖于 `x`的值。 函数编程中的纯函数是相同的。

编写纯函数时，该函数必须仅依赖于其参数，而不会执行任何导致副作用的操作。

下面是一个非纯函数的示例，因为它依赖于全局、可变状态：

```fsharp
let mutable value = 1

let addOneToValue x = x + value
```

`addOneToValue` 函数明显非纯，因为可以随时更改 `value`，使其值不同于1。 此模式根据全局值避免在函数编程中使用。

下面是一个非纯函数的另一个示例，因为它会产生副作用：

```fsharp
let addOneToValue x =
    printfn "x is %d" x
    x + 1
```

尽管此函数不依赖于全局值，但它会将 `x` 的值写入程序的输出。 尽管在执行此操作时不会出现任何错误，但它意味着函数不是纯函数。 如果程序的其他部分依赖于程序的外部内容（如输出缓冲区），则调用此函数可能会影响程序的其他部分。

删除 `printfn` 语句会使函数纯粹：

```fsharp
let addOneToValue x = x + 1
```

尽管此函数在本质上比使用 `printfn` 语句的以前版本_更好_，但它确实保证了所有此函数确实会返回一个值。 如果调用此函数任意多次，就会产生相同的结果：仅生成一个值。 纯度提供的可预测性是许多功能程序员所要做的。

### <a name="immutability"></a>不可变性

最后，类型化函数编程的最基本概念之一就是不永久性的。 在F#中，默认情况下，所有值都是不可变的。 这意味着，除非显式将它们标记为可变的，否则无法就地改变它们。

在实践中，使用不可变值是指将编程方式从 "我需要更改某些内容" 改为 "我需要生成新值"。

例如，将1添加到值意味着产生新的值，而不是改变现有值：

```fsharp
let value = 1
let secondValue = value + 1
```

在F#中，以下代码不**会改变**`value` 函数;相反，它会执行相等性检查：

```fsharp
let value = 1
value = value + 1 // Produces a 'bool' value!
```

一些功能编程语言根本不支持转变。 在F#中，它是受支持的，但它不是值的默认行为。

此概念甚至进一步扩展到数据结构。 在函数编程中，不可变的数据结构（如集（及更多）的实现方式不同于最初预期的实现。 从概念上讲，将项添加到集的操作不会更改集，它会生成一个具有已添加值的_新_集。 在这种情况下，这通常是通过不同的数据结构来实现的，该结构允许有效地跟踪值，以便可以为数据提供适当的表示形式。

这种使用值和数据结构的风格非常重要，因为它强制您对待任何修改内容的操作，就像它创建新版本一样。 这样就可以在您的程序中保持一致，例如相等性和比较。

## <a name="next-steps"></a>后续步骤

下一节将全面介绍函数，探索在函数编程中使用它们的不同方式。

[第一类函数](first-class-functions.md)会深入探讨函数，说明如何在各种上下文中使用它们。

## <a name="further-reading"></a>其他阅读材料

[思考功能](https://fsharpforfunandprofit.com/posts/thinking-functionally-intro/)系列是另一个有用的资源，用于了解的F#函数编程。 它以一种简单且易于阅读的方式介绍函数编程的基础知识，其中F#使用功能来说明这些概念。
