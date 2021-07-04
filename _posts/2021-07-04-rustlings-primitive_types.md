---
layout: post
title:  "rustlings-primitive_types"
date:   2021-07-04 13:14:31 +0800
tags:
      - Rust
---
# primitive_types1
```rust
// primitive_types1.rs
// Fill in the rest of the line that has code missing!
// No hints, there's no tricks, just get used to typing these :)


fn main() {
    // Booleans (`bool`)

    let is_morning = true;
    if is_morning {
        println!("Good morning!");
    }

    let is_evening = true;// Finish the rest of this line like the example! Or make it be false!
    if is_evening {
        println!("Good evening!");
    }
}
```

简单的布尔型。
# primitive_types2
```rust
// primitive_types2.rs
// Fill in the rest of the line that has code missing!
// No hints, there's no tricks, just get used to typing these :)
// I AM NOT DONE

fn main() {
    // Characters (`char`)
    let my_first_initial = 'C';
    if my_first_initial.is_alphabetic() {
        println!("Alphabetical!");
    } else if my_first_initial.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
    let your_character = 'N'; // Finish this line like the example! What's your favorite character?
    // Try a letter, try a number, try a special character, try a character
    // from a different language than your own, try an emoji!
    if your_character.is_alphabetic() {
        println!("Alphabetical!");
    } else if your_character.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
}

```

自定义个your_character。
# primitive_types3
```rust
// primitive_types3.rs
// Create an array with at least 100 elements in it where the ??? is.
// Execute `rustlings hint primitive_types3` for hints!

// I AM NOT DONE

fn main() {
    let a = ???

    if a.len() >= 100 {
        println!("Wow, that's a big array!");
    } else {
        println!("Meh, I eat arrays like that for breakfast.");
    }
}
```

这里涉及到的概念是数组的定义。Rust中的数组是固定长度的，一整块分配在栈上的内存。一旦声明，它们的长度不能增长或缩小。平时访问数组是通过索引访问，这个和其他语言差不多。

数组标记为[T; size] T为元素类型，size表示数组的大小。

# primitive_types4
```rust
// primitive_types4.rs
// Get a slice out of Array a where the ??? is so that the test passes.
// Execute `rustlings hint primitive_types4` for hints!!

#[test]
fn slice_out_of_array() {
    let a = [1, 2, 3, 4, 5];

    let nice_slice = ???

    assert_eq!([2, 3, 4], nice_slice)
}

```

这里涉及到一个没有所有权的类型：Slice类型。Slice允许你引用集合中一段连续的元素序列，而不用引用整个集合。切片的类型和数组类似，但其大小在编译时是不确定的。相反，切片是一个双字对象。第一个字是一个指向数据的指针，第二个字是切片的长度。这个"字"的宽度和usize相同，由处理器架构决定，比如在x86-64平台上就是64位。Slice 的类型标记为 &[T]。

Slice有很多种类型，先来说一说**字符串Slice**。

字符串Slice是String中一部分值的**引用**。引用的概念极其重要，后面也会专门讲到。它看起来像这样：

```rust
let s = String::from("hello world");
let hello = &s[0..5];
let world = &s[6..11];
```

可以使用一个由中括号中的[starting_index..ending_index]指定的range创建一个Slice，其中starting_index是Slice的第一个位置，ending_index则是Slice最后一个位置的后一个值。在其内部，Slice的数据结构存储了slice的开始位置和长度，长度对应于ending_index减去starting_index的值。所以对于let world = &s[6..11]的情况，world将是一个包含指向s第7个字节(从1开始)的指针和长度值5的slice。

对于Rust的“..”语法，如果想要从第一个索引(0)开始，可以不写两个点号之前的值。
```rust
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
```

上面两个语句是完全相同的。依此类推，如果Slice包含String的最后一个字节，也可以舍弃尾部的数字。这里不再过多赘述。

之前我们说过字符串字面值被储存在二进制文件中，现在我们可以重新理解一下字符串字面值。
```rust
let s = "Hello, world!";
```

这里s的类型是&str：它是一个指向二进制程序特定位置的Slice。这也就是为什么字符串字面值是不可变的；&str是一个不可变引用。

在知道了能够获取字面值和String的Slice后，我们可以把他们作为参数传入函数。如
```rust
fn first_word(s: &str) -> &str {
```

如果有一个字符串Slice，则可以直接传递它。如果有一个String，则可以传递整个String 的Slice。

下面是数组和Slice的相关例子，字符串Slice的类型声明写作&str，不过下面这个例子用的是更通用的Slice类型，即&[i32]。它跟字符串Slice的工作方式一样，通过存储第一个集合元素的引用和一个集合总长度。我们也可以对其他所有集合使用这类Slice
```rust
use std::mem;  //验证栈上内存引入的模块
//此函数借用一个slice
fn analyze_slice(slice: &[i32]) {
    println!("first element of the slice: {}", slice[0]);
    println!("the slice has {} elements", slice.len());
}
fn main() {
    //定长数组(类型标记是多余的)
    let xs: [i32; 5] = [1, 2, 3, 4, 5];
    //所有元素可以初始化成相同的值
    let ys: [i32; 500] = [0; 500];
    //下标从 0 开始
    println!("first element of the array: {}", xs[0]);
    println!("second element of the array: {}", xs[1]);
    //`len` 返回数组的大小
    println!("array size: {}", xs.len());
    //数组是在栈中分配的
    println!("array occupies {} bytes", mem::size_of_val(&xs));
    //数组可以自动被借用成为 slice
    println!("borrow the whole array as a slice");
    analyze_slice(&xs);
    //slice可以指向数组的一部分
    println!("borrow a section of the array as a slice");
    analyze_slice(&ys[1 .. 4]);
    //越界的下标会引发致命错误（panic）
    //println!("{}", xs[5]);
```

输出：
```rust
first element of the array: 1
second element of the array: 2
array size: 5
array occupies 20 bytes
borrow the whole array as a slice
first element of the slice: 1
the slice has 5 elements
borrow a section of the array as a slice
first element of the slice: 0
the slice has 3 elements
```

所以，将？？？那里替换为
```rust
let nice_slice = &a[1..4];
```

# primitive_types5
```rust
// primitive_types5.rs
// Destructure the `cat` tuple so that the println will work.
// Execute `rustlings hint primitive_types5` for hints!

// I AM NOT DONE
fn main() {
    let cat = ("Furry McFurson", 3.5);
    let /* your pattern here */ = cat;

    println!("{} is {} years old.", name, age);
}

```
这里涉及到了元组以及元组的解构。

前面在第一节简单提过元组属于复合类型，它是一个将多个其他类型的值组合进一个复合类型的主要方式。元组长度固定：一旦声明，其长度不会增大或缩小。所以也不会涉及到所有权的问题。我们使用包含在圆括号中的逗号分隔的值列表来创建一个元组。元组中的每一个位置都有一个类型，而且这些不同值的类型也不必是相同的。cat内部包含String字面值和f64两种类型。

为了从元组中获取单个值，我们可以使用模式匹配(pattern matching)来解构(destructure)元组值。除了使用模式匹配解构外，也可以使用点号(.)后跟值的索引来直接访问。

下面是元组的一些输入输出的例子，方便理解。
```rust
//元组可以充当函数的参数和返回值
fn reverse(pair: (i32, bool)) -> (bool, i32) {
    //可以使用let把一个元组的成员绑定到一些变量
    let (integer, boolean) = pair;
    (boolean, integer)
}

#[derive(Debug)]
struct Matrix(f32, f32, f32, f32);
fn main() {
    // 包含各种不同类型的元组
    let long_tuple = (1u8, 2u16, 3u32, 4u64,
                      -1i8, -2i16, -3i32, -4i64,
                      0.1f32, 0.2f64,
                      'a', true);

    //可以通过元组的下标来访问具体的值
    println!("long tuple first value: {}", long_tuple.0);
    println!("long tuple second value: {}", long_tuple.1);
    //元组也可以充当元组的元素
    let tuple_of_tuples = ((1u8, 2u16, 2u32), (4u64, -1i8), -2i16);
    //元组可以打印
    println!("tuple of tuples: {:?}", tuple_of_tuples);
    // 但很长的元组无法打印
    // let too_long_tuple = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13);
    // println!("too long tuple: {:?}", too_long_tuple);
    // 试一试 ^ 取消上面两行的注释，阅读编译器给出的错误信息。
    let pair = (1, true);
    println!("pair is {:?}", pair);
    println!("the reversed pair is {:?}", reverse(pair));
    // 创建单元素元组需要一个额外的逗号，这是为了和被括号包含的字面量作区分。
    println!("one element tuple: {:?}", (5u32,));
    println!("just an integer: {:?}", (5u32));
    // 元组可以被解构（deconstruct），从而将值绑定给变量
    let tuple = (1, "hello", 4.5, true);
    let (a, b, c, d) = tuple;
    println!("{:?}, {:?}, {:?}, {:?}", a, b, c, d);
    let matrix = Matrix(1.1, 1.2, 2.1, 2.2);
    println!("{:?}", matrix)
}

```

输出：
```rust
long tuple first value: 1
long tuple second value: 2
tuple of tuples: ((1, 2, 2), (4, -1), -2)
pair is (1, true)
the reversed pair is (true, 1)
one element tuple: (5,)
just an integer: 5
1, "hello", 4.5, true
Matrix(1.1, 1.2, 2.1, 2.2)
```

回到题目，我们添加下列语句即可。
```rust
let (name, age) = cat;
```

即可完成解构。

在例子中，我添加了Debug注解，后续会讲到。

# primitive_types6
```rust

// primitive_types6.rs
// Use a tuple index to access the second element of `numbers`.
// You can put the expression for the second element where ??? is so that the test passes.
// Execute `rustlings hint primitive_types6` for hints!

#[test]
fn indexing_tuple() {
    let numbers = (1, 2, 3);
    // Replace below ??? with the tuple indexing syntax.
    let second = ???;

    assert_eq!(2, second,
        "This is not the 2nd number in the tuple!")
}
```

这里即考察了元组的第二种访问方式：通过索引值访问。
```rust
let second = numbers.1; //Index
```

最后补充一下Rust的四则运算。

其中整数1、浮点数1.2、字符'a'、字符串"abc"、布尔值true和单元类型()可以用数字、文字或符号之类的字面量(literal)来表示。另外，通过加前缀0x、0o、0b，数字可以用十六进制、八进制或二进制记法表示。为了改善可读性，可以在数值字面量中插入下划线，比如：1_000等同于1000，0.000_001等同于0.000001。

```rust
//整数相加
println!("1 + 2 = {}", 1u32 + 2);
//整数相减
println!("1 - 2 = {}", 1i32 - 2);
//短路求值的布尔逻辑
println!("true AND false is {}", true && false);
println!("true OR false is {}", true || false);
println!("NOT true is {}", !true);
//位运算
println!("0011 AND 0101 is {:04b}", 0b0011u32 & 0b0101);
println!("0011 OR 0101 is {:04b}", 0b0011u32 | 0b0101);
println!("0011 XOR 0101 is {:04b}", 0b0011u32 ^ 0b0101);
println!("1 << 5 is {}", 1u32 << 5);
println!("0x80 >> 2 is 0x{:x}", 0x80u32 >> 2);
//使用下划线改善数字的可读性
println!("One million is written as {}", 1_000_000u32);
```

输出：
```rust
1 + 2 = 3
1 - 2 = -1
true AND false is false
true OR false is true
NOT true is false
0011 AND 0101 is 0001
0011 OR 0101 is 0111
0011 XOR 0101 is 0110
1 << 5 is 32
0x80 >> 2 is 0x20
One million is written as 1000000
```