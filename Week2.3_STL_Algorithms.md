# Week2.3 STL Algorithms

Course: Advanced C++ Programming (https://www.notion.so/Advanced-C-Programming-03efa34229a7430bba72f70fb8cfa384?pvs=21)
Confidence: Not Confident
Last Edited: June 16, 2024 10:06 PM
Is Completed: No

# STL Algorithms

STL（Standard Template Library）算法是C++标准库中的一部分，用于对容器中的数据进行各种操作和处理

## Examples

1. sum

```python
int sum = std::accumulate(nums.begin(), nums.end(), 0);
```

1. product

```python
int product = std::accumulate(v.begin(), v.end(), 1, std::multiplies<int>());
```

1. 仅对前半部分元素求和

```python
// 计算midpiont
  auto midpoint = v.begin() + (v.size() / 2);
  // This looks a lot harder to read. Why might it be better?
  auto midpoint11 = std::next(v.begin(), std::distance(v.begin(), v.end()) / 2);

  int sum2 = std::accumulate(v.begin(), midpoint, 0);
```

- `std::next` 是 C++ 标准库中的一个函数，用于从给定的迭代器开始，向前移动指定的距离，并返回新的迭代器
1. 检查元素是否存在

```python
auto it = std::find(nums.begin(), nums.end(), 4);
if (it != nums.end()) {
    std::cout << "Found it!" << "\n";
}
```

# Performance & Portability

使用 STL 算法可以使代码更具**可读性**和**可维护性**，因为我们可以在不同的容器类型上使用相同的算法。尽管不同容器的性能特性不同，但这种抽象使得代码**更通用**，**易于移植和修改**。

# Output Sequence Algorithms

## std::transform

使用 STL 算法中的输出序列算法来处理字符串

```python
#include <iostream>
#include <vector>
char to_upper(unsigned char value)
{
return static_cast<char>(std::toupper(static_cast<unsigned char>(value)));
}
int main()
{
std::string s = "hello world";
// Algorithms like transform, which have output iterators,
// use the other iterator as an output.
auto upper = std::string(s.size(), '\0');
std::transform(s.begin(), s.end(), upper.begin(), to_upper);
}
```

- `std::transform` 是一个功能强大的算法，可以用于将函数应用于容器中的每个元素
- **`std::transform`**：接受两个输入范围 `[first1, last1)` 和一个输出范围的起始位置 `first2`
- **`std::transform`**：返回一个迭代器，指向输出范围的末尾，通常用于标识生成的输出的结束位置。

## std::back_inserter

- `std::back_inserter` 是一个标准库函数，它返回一个插入迭代器，该迭代器将元素添加到容器的末尾

## std::for_each

- `std::for_each(first, last, func)` 函数接受一个范围 `[first, last)` 和一个函数对象 `func`。
- 它会对范围内的每个元素调用 `func`，并按顺序处理每个元素

# Lambda

- 一种便捷的方式，用于定义匿名的、可调用的函数对象。
- Lambda 函数可以在其他函数内部定义，也可以作为参数传递给其他函数，或者赋值给变量使用

```python
[capture](parameters) -> return_type {
    // body of lambda function
}
```

- **`capture`**：捕获列表，用于捕获外部作用域的变量。可以是空的 `[]`，也可以按值 `[=]` 或按引用 `[&]` 捕获，或者混合捕获具体的变量 `[x, &y]`
    - **按值捕获 `[=]`**：
        - 捕获所有外部变量的副本（按值捕获）。
        - 在 lambda 函数体内部，修改捕获的变量**不会影响外部变量的值**。
    - **按引用捕获 `[&]`**：
        - 捕获所有外部变量的引用。
        - 在 lambda 函数体内部修改捕获的变量，**会直接影响**到**外部变量的值**
    - **混合捕获具体的变量 `[x, &y]`**：
        - 按值捕获变量 `x`，按引用捕获变量 `y`。
        - 可以根据需要对不同的变量使用不同的捕获方式。
- **`parameters`**：参数列表，与普通函数一样，用于接收传递给 lambda 的参数
- **`return_type`**：返回类型，可选的，用于指定 lambda 函数的返回类型

# Iterator Categories

| Operation 手术 | Output 输出 | Input 输入 | Forward 向前 | Bidirectional 双向 | Random Access 随机访问 |
| --- | --- | --- | --- | --- | --- |
| Read 读 |  | =*p =*p | =*p =*p | =*p =*p | =*p =*p |
| Access 使用权 |  | -> -> | -> -> | -> -> | ->[] ->[] |
| Write 写 | *p= *p= |  | *p= *p= | *p= *p= | *p= *p= |
| Iteration 迭代 | ++ ++ | ++ ++ | ++ ++ | ++ -- ++ -- | ++ -- + - += -=++ -- + - += -= |
| Compare 比较 |  | == != ==！= | == != ==！= | == != ==！= | == != < > <= >=== != < > <= >= |

An algorithm requires certain kinds of iterators for their operations:

1. input: find(), equal()
2. output: copy()
3. forward: replace(), binary_search()
4. bi-directional: reverse()
5. random: sort()

A container's iterator falls into a certain category:

1. forward: forward_list
    1. 只支持单向顺序访
2. bi-directional: map, list
    1. 支持双向顺序访问，但不支持随机访问
3. random: vector, deque
    1. 支持随机访问和修改