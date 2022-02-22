# C++之Lambda表达式

## 1.概述

C++ 11 中的 Lambda 表达式用于定义并创建匿名的<mark style="color:blue;">**函数对象**</mark>，以简化编程工作。

语法形式如下：

```
[函数参数对象](操作符重载函数参数) mutable 或 exception 或 attribute 声明 -> 返回值类型{函数体}
[capture-list] ( params ) mutable(optional) exception(optional) attribute(optional) -> ret(optional) { body } 
```

## 2.语法分析

### 2.1 \[函数对象参数]

标识一个 Lambda 表达式的开始，这部分必须存在，不能省略。

函数对象参数是传递给编译器自动生成的函数对象类的构造函数的。

函数对象参数只能使用那些到定义 Lambda 为止时 ，Lambda 所在作用范围内可见的局部变量。其实就是将局部自动变量保存到 Lambda 表达式内部（Lambda 表达式不能捕获全局变量或 static 变量）。

函数对象参数有以下形式：

* <mark style="color:red;">空</mark>。没有任何函数对象参数。
* <mark style="color:red;">=</mark>。函数体内可以使用 Lambda 所在范围内<mark style="color:blue;">**所有**</mark>可见的局部变量，并且是<mark style="color:blue;">**值传递**</mark>方式（相当于编译器自动为我们按值传递了所有局部变量）。
* <mark style="color:red;">&</mark>。函数体内可以使用 Lambda 所在范围内<mark style="color:blue;">**所有**</mark>可见的局部变量，并且是<mark style="color:blue;">**引用传递**</mark>方式 （相当于是编译器自动为我们按引用传递了所有局部变量）。
* <mark style="color:red;">this</mark>。在成员函数中的 Lambda 表达式可以捕获当前对象的 this 指针，让 Lambda 表达式拥有和当前类成员同样的访问权限，可以修改类的成员变量，使用类的成员函数。只能按值捕获 `[this]` ，不能按引用捕获 `[&this]`&#x20;

不建议直接使用 \[&] 或 \[=] 捕获所有参数，而是按需显示捕获。

### 2.2 （操作符重载函数参数）

和普通函数一样的参数。

没有参数时，这部分可以省略。

参数可以通过按值（如: (a, b)）和按引用 (如: (\&a, \&b)) 两种 方式进行传递。

### 2.3 mutable 或 exception 或attribute声明

<mark style="color:blue;">**默认情况下函数是const的**</mark>，按值传递函数对象参数时，加上 mutable 修饰符后，可以修改传递进来的拷贝（注意是能修改拷贝，而不是值本身）。

exception 声明用于指定函数抛出的异常，如抛出整数类型的异常，可以使用 throw(int)。

略。

### 2.4 ->返回值类型

标识函数返回值的类型。

当返回值为 void，或者函数体中只有一处 return 的地方（此时编译器可以自动推断出返回值类型）时，这部分可以省略。

### 2.5 {函数体}

标识函数的实现，不可省略，但函数体可为空。

## 3.示例

```
struct SBook {
    int _Id; 
    std::string _Title;
    double _Price;
};

std::vector<Book> Books;

std::string Target = "C++";  // 找出其中 title 包含“C++”的书本的数量
```

* 按值捕获

```
auto Cnt =
    std::count_if(Books.begin(), Books.end(), [Target](const SBook& Book) {
        return Book._Title.find(Target) != std::string::npos;
    });
```

`[Target]` 表示按值捕获 Target。Lambda 表达式内部会保存一份 Target 的副本，名字也叫 Target。

* 按引用捕获

```
auto Cnt =
    std::count_if(Books.begin(), Books.end(), [&Target](const SBook& Book) {
        return Book._Title.find(Target) != std::string::npos;
    });
```

`[&Target]` 表示按引用捕获 Target——不会复制多一份副本。

## 4. 其他

### 4.1 C++14支持 lambda capture initializer

```
// 按值捕获 Target，但是在 Lambda 内部的变量名叫做 V
auto Cnt =
    std::count_if(Books.begin(), Books.end(), [v = Target](const SBook& Book) {
        return Book._Title.find(V) != std::string::npos;
    });

// 按引用捕获 Target，但是在 Lambda 内部的名字叫做 R
auto Cnt =
    std::count_if(Books.begin(), Books.end(), [&R = Target](const SBook& Book) {
        return Book._Title.find(R) != std::string::npos;
    });
```

{% hint style="info" %}
std::string::npos是一个常数，它等于size\_type类型可以表示的最大值，用来表示一个不存在的位置,类型一般是std::container\_type::size\_type。
{% endhint %}

Lambda 捕获列表初始化最最最重要的一点是“支持 Capture by Move”。

在 C++14 之前，Lambda 是不支持捕获一个 Move-Only 的对象的，比如：

```
std::unique_ptr<int> pUptr = std::make_unique<int>(123);
auto callback = [pUptr]() {                   // 编译错误，pUptr is move-only
    std::cout << *pUptr<< std::endl;
};
```

按引用捕获虽然可以编译通过，但往往是不符合要求的。比如下面的例子，离开作用域之后 pUptr会被析构掉。但是 callback 对象已经被传给另一个线程。

```
std::unique_ptr<int> pUptr= std::make_unique<int>(123);
auto callback = [&pUptr]() {                               
    std::cout << *pUptr<< std::endl;
};  
// ... 将 callback 传给另一个线程
// return => pUptr delete 掉指向的内存
```

通过捕获列表初始化，完成 Move-Only 对象的“Capture by Move”。

```
std::unique_ptr<int> pUptr= std::make_unique<int>(123);
auto callback = [pUptr = std::move(pUptr)]() {// 将 pUptr 移动给 Lambda 表达式中的参数
    std::cout << *pUptr<< std::endl;
};
// ... 将 callback 传给另一个线程
// return => pUptr是 nullptr
```
