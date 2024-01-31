# Rust Learning

> author: Haohahahaha (Haorui Zhang)
>
> date: 2024-01-29~31
>
> email: 1259203802@qq.com

!!! note "学习的书籍"
    [The Rust Programming Language](https://doc.rust-lang.org/book)

!!! tip "仅为个人笔记"
    [个人学习仓库 - rust-rustlings-Haohahahaha](https://github.com/LearningOS/rust-rustlings-Haohahahaha)

    本笔记包含 `rustlings` 练习。

## 1. Getting Started

### 1.3 Hello, Cargo

!!! note "命令"

    ```bash
    cargo new   # 新建项目
    cargo build # 构建项目，生成的目标文件要去 target/build/ 查看
    cargo run   # 运行，直接看结果
    cargo check # 检查编译错误
    ```
!!! note "项目结构"
    
    ```bash
    [4.0K]  .
    ├── [ 155]  Cargo.lock  # 自动生成，掌握依赖库版本信息（Chap.2 提到）
    ├── [ 180]  Cargo.toml  # 列出项目信息、依赖等
    ├── [4.0K]  .git
    ├── [   8]  .gitignore
    ├── [4.0K]  src # 源代码
    │   └── [  45]  main.rs
    └── [4.0K]  target  # 生成的目标文件
        ├── [ 177]  CACHEDIR.TAG
        ├── [4.0K]  debug   # 调试生成的文件，生成快但运行慢
        ├── [4.0K]  release   # 发布的文件，生成慢（编译优化）但运行快
        └── [1.5K]  .rustc_info.json
    ```

## 2. Programming a Guessing Game

!!! warning "English Words"
    ```
    comes in handy  # 派上用场
    scope           # 作用域
    parentheses     # 圆括号
    immutable       # 不可变的，其中提取 mut 作为可变变量关键字 
    instance        # 实例（建立某种类型的实例）
    associated      # 关联（ ~ func，关联函数。可理解为成员函数？）
    append          # 附加
    hence           # 因此
    trivial         # 琐碎的，不重要的
    scenario        # 情景、场景   
    ultimately      # 最终
    insatiable      # 贪得无厌的
    ```

**语法部分：**

- `fn`：声明函数体

- `!`：宏

- `let`：创建变量

- `mut`：（变量的）可变属性

- `//`：注释

- `::`：关联函数/子函数/子类/父子关系……

- `use [LIB]`：引用名为 `[LIB]` 的库函数/包

- `io::stdin().readline()`：非覆写的输入函数

- `&`：引用（Reference），可大概理解为“指针”（而非在内存中复制多份）。

    默认不可变，可变写法：

    ```rust
    &mut guess
    ```
- 使用成员函数时，用新的一行或者一些空格会使语法结构清晰，可读性强。

    如：

    ```rust
    // 错误示范
    io::stdin().read_line(&mut guess).expect("Failed to read line");
    
    // 正确示范
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");
    ```

    结果有两种“变体”——`variant`，分别是 `Ok` 和 `Err`。

    对结果返回时，可用 `expect()` 函数来判断返回结果是否合法。如果不合法（`Err`），则输出错误信息（函数内的字符串）；如果合法（`Ok`），则返回值。

- 打印变量值时，变量放在花括号 `{}` 内。

    如果打印的是表达式（的求值结果），则花括号空，然后（像C一样）在逗号后面按顺序写变量名。

- `match`：根据不同结果确定接下来要做什么，可根据不同情况进行处理

    - `Err(_)`：表示匹配所有错误
    >  The underscore, _, is a catchall value; in this example, we’re saying we want to match all Err values, no matter what information they have inside them. 

- `shadow`特性：允许同名不同类型，后者覆盖前者

- `:`：后面加强制转换的变量类型，转换用 `parse()` 子函数

- 循环：

    ```rust
    loop{
        // LOOP CODE
    }
    ```

---

*完成！*

输出如下：

```sh
# oslab @ oslab-virtual-machine in ~/Documents/Rust-Book-remaining/guessing_name on git:master x [21:56:44] 
$ cargo run
   Compiling guessing_name v0.1.0 (/home/oslab/Documents/Rust-Book-remaining/guessing_name)
    Finished dev [unoptimized + debuginfo] target(s) in 0.17s
     Running `target/debug/guessing_name`
Guess the number!
Please input your guess.
89
You guessed: 89
Too big!
Please input your guess.
50
You guessed: 50
Too small!
Please input your guess.
66
You guessed: 66
Too big!
Please input your guess.
59
You guessed: 59
Too small!
Please input your guess.
63
You guessed: 63
You win!
```

## 3. Common Programming Concepts

### 3.1 Variables and Mutability

- `let` 可“遮盖”类型，但 `mut` 只是改变值。

- 常量 `constant` 必须声明类型

### 3.2 Data Types

!!! note "Integer Literals in Rust"

    | Number literals  | Example       |
    | ---------------- | ------------- |
    | Decimal          | `98_222`      |
    | Hex              | `0xff`        |
    | Octal            | `0o77`        |
    | Binary           | `0b1111_0000` |
    | Byte (`u8` only) | `b'A'`        |

- Tuple

    - 多种类型组合而成

    - 可以用 `.[INDEX]` 的形式来访问元组内某个序号 `[INDEX]` 的元素（以0开始）
    
    - 可以解构，分别赋值给各个新建变量

- Array

    - 初始化值：`let a: [初始值:元素个数];`

    - 规定变量类型：`let a: [元素类型:元素个数] = [1, 2, ...];`

### 3.3 Functions

!!! warning "English Words"
    ```
    prevalent  # 盛行、普遍的
    ```

- 函数命名规则：`snake case`——全小写，单词之间用下划线分割

- 只要在调用者可见的作用域中定义函数即可，无先后顺序

- 函数的参数必须标明变量类型，用 `,` 隔开