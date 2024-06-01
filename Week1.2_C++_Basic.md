# Week1.2 C++ Basic

# Keywords

## auto

## const

- 这个值是不能被更改的
- `auto const meaning_of_life = 42;` , 写在 `type` 的后面

# Value Semantics

使用 `值语义` 的对象在赋值、传递和返回时，会创建对象的**副本**，而**不是传递对象的引用**。

# Type Conversion

## Implicit

```cpp
#include <catch2/catch.hpp>
TEST_CASE()
{
auto const i = 0;
{
auto d = 0.0;
REQUIRE(d == 0.0);
d = i; // Silent conversion from int to double
CHECK(d == 42.0);
CHECK(d != 41);
}
}
```

## Explicit

```cpp
auto const d = static_cast<double>(i);
```

# Functions

有两种函数定义的方法，一种是传统的方法，一种是现代的方法（现代的方法更为直观）

- 传统
    
    ```cpp
    int main() {
    }
    ```
    
- 现代
    
    ```cpp
    auto main() -> int {
    }
    ```
    

使用两种方法都是可以的，只要**在项目中保持一致性**，确保代码风格统一，便于阅读和维护。

## Default arguments

默认参数在当函数有很多参数，但大多是情况下只要指定其中一部分时就很有用，示例代码如下：

```cpp
#include <iostream>

// 函数声明，带有默认参数
void printValues(int a, int b = 20, int c = 30) {
    std::cout << "a: " << a << ", b: " << b << ", c: " << c << std::endl;
}

int main() {
    printValues(10);           // 调用时只提供了一个实际参数，输出: a: 10, b: 20, c: 30
    printValues(10, 40);       // 调用时提供了两个实际参数，输出: a: 10, b: 40, c: 30
    printValues(10, 40, 50);   // 调用时提供了三个实际参数，输出: a: 10, b: 40, c: 50
    return 0;
}
```

## **Function overloading**

是 `Function` 具有**相同的名称**，但是**形参不同。**这种设计提高了代码的灵活性和可读性。接下来我们来看看函数重载解析。

### Function overloading resolution

1. **查找候选函数（Find candidate functions）：**在函数调用时，编译器首先会查找**具有相同名称的所有函数**
2. **选择可行函数（Select viable ones）：**会选择具有一下特征的函数作为可行函数
    1. 形参数量与调用时的实参数量相同
    2. 对于每一个形参，调用时所提供的实参能通过隐式转换或者精准匹配转换为形参的类型
3. **寻找最佳匹配（Find a best-match）：**至少有一个实参的类型在某个候选函数中更加精确匹配

函数匹配中的**错误**通常在**编译时被发现**，而不是在运行时。且**返回类型被忽略**

我们在写代码时，为了提高代码的质量和可维护性，可以：

- 只创建简单易懂的重载版本
- 如果难以理解，请为函数命不同的名

## Function Passing Method

- Pass by value
    
    当你调用一个函数时，实参的值会被**复制**到函数内部用于存储形参的内存空间中
    
- Pass by reference
    
    在调用时传递的是实参的引用，而**不是实参的副本(Copy)**
    
    - 意味着在函数内部对形参的修改都是对实参的修改

**什么时候用引用**：

1. **实参没有拷贝操作**：当实参类型没有定义拷贝操作时，无法通过值传递（pass by value）进行函数调用。引用传递可以解决这个问题。
2. **实参很大**：当实参对象很大时，值传递会导致高昂的拷贝开销。

# Reference & Point

`auto& num` 是一个引用（Reference），是某个对象的别名，存着某个对象的地址，与Point之间的区别：

- 不需要使用 **`>`** 操作符来访问元素。
- 引用不能是空的（即，引用必须初始化为某个对象）。
- 一旦设置，引用**不能更改它所引用的对象**。

# **Declaration & Definition**

## **Declaration**

- 声明只让编译器知道变量的类型和名称。
- 它不分配存储空间，也不创建实际的对象或变量。

```cpp
void declared_fn(int arg); // 函数声明
class declared_type;       // 类声明
```

## **Definition**

定义是一种声明，但还做了额外的事情

- 变量定义分配存储空间并构造变量
- 类定义允许你创建该类类型的变量

声明只是告诉编译器有某个实体存在，而定义则创建该实体并分配内存

# Enum

是一种用户定义的类型，它由一组命名的常量组成。枚举类型可以提高代码的可读性和维护性

# Programming Error

1. Compiler Error

通常是语法错误或类型错误

```cpp
auto main() -> int {
    a = 5; // 编译时错误：未指定类型
}
```

1. Link-time Errors
    
    在编译之后、程序生成可执行文件之前，通常是因为无法找到函数或变量的定义
    
    ```cpp
    #include <iostream>
    
    auto is_cs6771() -> bool;
    
    int main() {
        std::cout << is_cs6771() << "\n";
    }
    ```
    
2. Run-time Errors
    
    通常是由于非法操作，如除以零、数组越界、访问空指针或文件未找到等，运行时错误会导致程序崩溃或异常
    
    ```cpp
    // 尝试打开一个文件...
    if (auto file = std::ifstream("hello.txt"); not file) {
        throw std::runtime_error("Error: file not found.\n");
    }
    ```
    
3. Logic Errors
    
    发生在程序按预期编译和运行，但程序的行为不符合预期
    

**改错策略**：

- **编译时错误**：需要修正代码的语法或类型问题。
- **链接时错误**：需要确保所有的函数和变量定义都能被找到。
- **运行时错误**：需要处理异常情况，确保程序能安全运行。
- **逻辑错误**：需要仔细检查程序逻辑，确保其行为符合预期。