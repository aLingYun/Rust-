## 变量可变性

在 `C/C++` 中，变量默认是可变的。而在 `Rust` 中，变量默认是不可变的。

```rust
    let x = 10;
    //x = 11;		//错误的
    println!("x = {}", x);
```

它和 `const` 类型不一样，它可以使用 `let` 关键字重新**绑定**新的值，以是同一个变量可以拥有不同的值，并将旧的值隐藏掉了。在 `rust` 中，这种操作被称作 **隐藏**。使用 `let` 时，相当于创建了新的变量，可以改变变量的类型。

```rust
    let x = 9;
    let x = x + 11;
    let x = "Rust";
    println!("x = {}", x);   // x = Rust
```

而 `Rust` 中的常量用法如下，它不能使用 let 关键字，因为他们有本质区别。注意 `Rust` 中的常量声名时必须指定类型，例如 `i32`。

```rust
    const MAX:i32 = 10000;
    //let MAX = 100;
    println!("MAX = {}", MAX);
```

如果想要变量可变，则需要在 `let` 关键字后面加上 `mut` 关键字。这种方式是将绑定到 `x` 的值从 9 改成 10，并没有创建新的变量。与使用 `let` 关键词隐藏有所区别。

```rust
    let mut x = 9;
    x = 10;
    println!("x = {}", x);   // x = 10
```

### 代码及运行结果

```rust
fn main() {
    const MAX:i32 = 10000;
    //let MAX = 100;
    println!("MAX = {}", MAX);

    let x = 10;
    //x = 11;
    println!("x = {}", x);

    let mut x = 9;
    println!("x = {}", x);
    x = 10;
    println!("x = {}", x);

    let x = 9;
    //x = 10;
    let x = x + 11;
    println!("x = {}", x);
    let x = "Rust";
    println!("x = {}", x);
}
```

```bash
C:/Users/uidn2775/.cargo/bin/cargo.exe run --color=always --package study --bin study
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
    Finished dev [unoptimized + debuginfo] target(s) in 0.55s
     Running `target\debug\study.exe`
MAX = 10000
x = 10
x = 9
x = 10
x = 20
x = Rust

进程已结束,退出代码0
```



## 数据类型

Rust 通常可以推断出我们想要的类型，但当多种类型均有可能时，就必须增加注解。因此需要程序员自己去了解想要的类型。

### 标量类型

rust 中的标量类型包括**整形、浮点型、布尔类型和字符类型**。

- #### 整形

| 长度    | 有符号  | 无符号  |
| ------- | ------- | ------- |
| 8-bit   | `i8`    | `u8`    |
| 16-bit  | `i16`   | `u16`   |
| 32-bit  | `i32`   | `u32`   |
| 64-bit  | `i64`   | `u64`   |
| 128-bit | `i128`  | `u128`  |
| arch    | `isize` | `usize` |

前几个一看便知，另外`isize` 和 `usize` 类型依赖运行程序的计算机架构：64 位架构上它们是 64 位的， 32 位架构上它们是 32 位的。

| 数字字面值                    | 例子          |
| ----------------------------- | ------------- |
| Decimal (十进制)              | `98_222`      |
| Hex (十六进制)                | `0xff`        |
| Octal (八进制)                | `0o77`        |
| Binary (二进制)               | `0b1111_0000` |
| Byte (单字节字符)(仅限于`u8`) | `b'A'`        |

- #### 浮点型

`Rust` 中有两种 **浮点数** 类型，默认使用 `f64`，如需使用 `f32` 需要显示指定类型。

```rust
    let f_x = 1.1;
    let f_x1 :f32 = 2.2;
    println!("f_x = {}", f_x);
    println!("f_x1 = {}", f_x1);
```

- #### 布尔型

```rust
    let bo_flag = true;
    println!("bo_flag = {}", bo_flag);
    let bo_flag :bool = false;
    println!("bo_flag = {}", bo_flag);
```

- #### 字符类型

Rust 的 `char` 类型的大小为**四个字节**(four bytes)，并代表了一个 Unicode 标量值（Unicode Scalar Value），这意味着它可以比 ASCII 表示更多内容。在 Rust 中，拼音字母（Accented letters），中文、日文、韩文等字符，emoji（绘文字）以及零长度的空白字符都是有效的 `char` 值。Unicode 标量值包含从 `U+0000` 到 `U+D7FF` 和 `U+E000` 到 `U+10FFFF` 在内的值。不过，“字符” 并不是一个 Unicode 中的概念，所以人直觉上的 “字符” 可能与 Rust 中的 `char` 并不符合。

```rust
    let c = 'c';
    println!("c = {}", c);
    let z :char = 'ℤ';
    println!("z = {}", z);
    let a:char = '爱';
    println!("a = {}", a);
    let heart_eyed_cat = '😻';
    println!("heart_eyed_cat = {}", heart_eyed_cat);
```

### 复合类型

`Rust` 有两种复合类型，即 **元组** 和 **数组**。

- #### 元组

元组 是一个及将多个**不同类型**的值进行组合的一个复合类型。元组一旦声明，其长度就固定，不会增大或缩小。

```rust
    let tup = (3,'c',"string");
    let (x,y,z) = tup;
    println!("{}", z);   //输出 string
```

加注解的方式：

```rust
    let tup:(i32,char,&str) = (3,'c',"string");
    let (x,y,z) = tup;
    println!("{}", z);   //输出 string
```

用 `x, y, z` 三个变量绑定元组的三个元素，这叫做 **模式匹配解构**。编译器会进行自动类型推导。除此之外，还可以用 `.` + 索引的方式来直接访问元组的元素。

```rust
    let tup:(i32,char,&str) = (3,'c',"string");
    println!("{}", tup.0);
    println!("{}", tup.1);
    println!("{}", tup.2);
```

- #### 数组

数组 是多个**相同类型**的值的组合。数组一旦声明，其长度就固定，不能增大或缩小。可以像这样编写数组：

```rust
    let str = ["hello", "world", "hello", "Rust"];
    let string :[i32; 4] = [1, 2, 3, 4];		//带类型注解
    let nums = [9; 5];

	//访问数组
    println!("str[0] = {}", str[0]);			//hello
    println!("string[1] = {}", string[1]);		//2
    println!("nums[2] = {}", nums[2]);			//9
```

如果数组访问越界，

```rust
    let nums = [9; 5];						//[9,9,9,9,9]
    println!("nums[2] = {}", nums[5]);
```

则会出现如下错误：

```bash
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
error: this operation will panic at runtime
  --> src\main.rs:58:30
   |
58 |     println!("nums[2] = {}", nums[5]);
   |                              ^^^^^^^ index out of bounds: the len is 5 but the index is 5
   |
   = note: `#[deny(unconditional_panic)]` on by default

error: aborting due to previous error

error: could not compile `study`.

To learn more, run the command again with --verbose.
```

## 函数

`Rust` 中使用 `fn` 关键字来声明新函数，括号的使用和 `C/C++` 类似。不同的是 `Rust` 不关心函数定义在之前或之后。

```rust
fn main() {
    func();
}

fn func() {
    println!("Hello Rust!");
}
```

### 带参数

参数的位置与 `C/C++` 相同，都在函数名后面的小括号里。需要注意的是**必须声明每个参数的类型。**这意味着编译器不需要你在代码的其他地方注明类型来指出你的意图。

```rust
fn main() {
    func(32,46);
}

fn func(x: i64, y: i32) {
    println!("Hello Rust! X= {}", x);
    println!("Hello YYYY = {}", y);
}
```

执行结果如下：

```bash
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
    Finished dev [unoptimized + debuginfo] target(s) in 0.60s
     Running `target\debug\study.exe`
Hello Rust! X= 32
Hello YYYY = 46

进程已结束,退出代码0
```

如果在函数声明时，不确定参数类型，则会报错：

```rust
fn main() {
    func(32,46);
}

fn func(x, y) {
    println!("Hello Rust! X= {}", x);
    println!("Hello YYYY = {}", y);
}
```

```bash
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
error: expected one of `:`, `@`, or `|`, found `,`
 --> src\main.rs:7:10
  |
7 | fn func(x, y) {
  |          ^ expected one of `:`, `@`, or `|`
  ......
```

### 返回值

`Rust` 是一门基于表达式的语言。程序员需要了解表达式和语句的区别以及对函数体的影响。

**语句** 是执行一些操作但不返回值的指令。

语句不返回值。因此，不能把 `let` 语句赋值给另一个变量，如：

```rust
    let x = (let y = 6);  //错误，(let y = 6) 不是一个表达式而是一个语句
```

**表达式** 是计算并产生一个值。

上面代码中语句 `let y = 6;` 中的 `6` 是一个表达式，它计算出的值是 `6`。函数调用是一个表达式。宏调用是一个表达式。我们用来创建新作用域的大括号（代码块），`{}`，也是一个表达式。**表达式的结尾没有分号**。如果在表达式的结尾加上分号，它就变成了语句，而语句不会返回值。

在 `Rust` 中，不需要为返回值命名，但要在小括号后面加 `->` 和声明他的类型。函数的返回值等同于函数体**最后一个表达式的值**。可以使用 `return` 关键字和指定值，从函数中提前返回。如下代码：

```rust
fn main() {
    println!("func return value = {}", func(32,46));
    println!("func1 return value = {}", func1());
}

fn func(x: i32, y: i32) -> i32 {
    println!("x = {}", x);
    println!("y = {}", y);
    let x = 3;
    let y = 99;
    println!("x = {}", x);
    println!("y = {}", y);
    
    //return x + 1;   //如果解注释的话，就会该函数就会返回 x+1, 即 4
    
    {
        y/x
    }
}

fn func1() -> i32 {
    5
}
```

执行结果：

```bash
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
    Finished dev [unoptimized + debuginfo] target(s) in 0.53s
     Running `target\debug\study.exe`
x = 32
y = 46
x = 3
y = 99
func return value = 33		// y/x
func1 return value = 5		// 5

进程已结束,退出代码0
```

## 控制流

### 分支

`if` 分支以 `if` 关键字开头，可选的 `else if` 和 `else`。用法和 `C/C++` 用法类似，不同的是判断条件不需要用小括号括起来。例如：

```rust
fn main() {
    let number = 9;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

需要注意：**`Rust` 中 `if` 条件必须是 bool 类型的**。

#### 在 let 中使用 if

因为 `if` 是一个表达式，我们可以在 `let` 语句的右侧使用它。需要注意的是 **`if` 的每个分支的可能返回值都必须是相同类型**。

```rust
fn main() {
    let condition = true;
    let number = if condition {
        5
    } else {
        6
        //"six"		//会报错
    };

    println!("The value of number is: {}", number);
}
```

```bash
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
    Finished dev [unoptimized + debuginfo] target(s) in 0.51s
     Running `target\debug\study.exe`
The value of number is: 5
```

### 循环

Rust 有三种循环：`loop`、`while` 和 `for`。

#### loop

`loop {}` 相当于`C/C++` 中的 `while(1) {}`，是一个死循环，需要手动停止。

可以用关键字 `break` 结合 `if` 检查从循环中返回，并返回检查结果。例如：

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {}", result);
}
```

 执行结果：

```bash
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
    Finished dev [unoptimized + debuginfo] target(s) in 0.78s
     Running `target\debug\study.exe`
The result is 20

进程已结束,退出代码0
```

#### while

`loop` 结合 `if` 退出循环的方式比较麻烦，可以使用条件循环 `while` 。

```rust
fn main() {
    let mut flag = 5;
    while flag > 0 {
        println!("{} - Hello Rust!", flag);
        flag = flag - 1;
    }
}
```

执行结果：

```bash
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
    Finished dev [unoptimized + debuginfo] target(s) in 0.60s
     Running `target\debug\study.exe`
5 - Hello Rust!
4 - Hello Rust!
3 - Hello Rust!
2 - Hello Rust!
1 - Hello Rust!

进程已结束,退出代码0
```

#### for

`for` 循环一般用来遍历集合。使用 `loop` 或 `while` 循环来遍历集合不安全或比较麻烦。

如果直接用集合的 index 作为条件，容易出现越界或者遍历长度不够的情况：

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("the value is: {}", a[index]);

        index = index + 1;
    }
}
```

结合集合的成员函数获取长度可以避免越界，但是比较麻烦：

```rust
fn main() {
    let numbers = [1,2,3,4,5,6,7,8,9];
    let mut i = 0;
    while i < numbers.len() {
        println!("numbers[{}] = {}", i, numbers[i]);
        i = i + 1;
    }
}
```

用 `for` 循环可以避免这些问题：

```rust
fn main() {
    let numbers = [1,2,3,4,5,6,7,8,9];
    for element in numbers.iter() {
        println!("number = {}", element);
    }
}
```

执行如下：

```bash
   Compiling study v0.1.0 (D:\MyFiles\rustCode\study)
    Finished dev [unoptimized + debuginfo] target(s) in 1.16s
     Running `target\debug\study.exe`
number = 1
number = 2
number = 3
number = 4
number = 5
number = 6
number = 7
number = 8
number = 9

进程已结束,退出代码0
```

