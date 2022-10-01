# Effective Modern C++

## 第一章：类别推导

**条款1：理解模板类型推导**

函数声明。

```cpp
template<typename T>
void f(ParamType param);
```

函数调用

```cpp
void f(expr);
```



情况1：`ParamType`含引用或指针。

```cpp
template<typename T>
void f([const] T& param);

template<typename T>
void f([const] T* param);
```

模板推导出来也一定是指针或者引用。

不含`const`模板可以推导出含`const`的`expr`调用。

```cpp
template<typename T>
void f(T* val) {
    printf("void f(T* %d)\n", *val);
}

int main() {
    int* a = new int(1);
    f(a);

    const int* b = new int(2);
    f(b);
}
/* result
void f(T* 1)
void f(T* 2)
*/
```

但注意，如果函数违法了`const`，会报错。

```cpp
template<typename T>
void f(T* val) {
    *val = 10; //Read-only variable is not assignable
    printf("void f(T* %d)\n", *val);
}

int main() {
    const int* b = new int(2);
    f(b);
}
```



**情况2: `ParamType`是个万能引用。**

```cpp
template<typename T>
void f(T&& param);
```

如果`expr`为

* 左值，则参数类型最终为`[const] T&`。
* 右值，则参数类型最终为`[const] T&&`。

即，万能引用能够推导是左值还是右值。

`const`的推到类似情况一，根据`expr`是否有`const`决定。



**情况3: 按值传递**

```cpp
template<typename T>
void f(T param);
```

通过`copy constructor`构造处理

由于是副本，因此该`expr`本身的

* `const`
* 引用
* `volatile`

都会被忽略（对它的副本改变并不破坏它本身的`const`）。

值得一提的是，仅仅会移除形参本身的`const`。

```cpp
const char* const ptr = "Fun with pointers";
f(ptr); //推到类型为 const char*
```

即，函数参数是一个`const char*`，是`ptr`的副本。



 **数组实参**

类似C中语法，数组会退化成首元素指针。

```cpp
const char arr[] = "array";

template<typename T>
void f(T Param)

f(arr);
```

`arr`是个数组，会被推导成指针。



但如果是引用，则正常推导。

```cpp
template<typename T>
void f(T& Param)

f(arr); //推导成 const char (&)[size] 类型，其中size是arr的大小 + 1 (\0)。
```



获取数组大小。

```cpp
template<typename T, std::size_t N>
constexpr std::size_t arraySize(T (&)[N]) {
    retur N;
}
```



**函数实参**

除了数组退化成指针，函数和数组处理相同。

```cpp
void someFunc(int, double);

template<typename T>
void f1(T param);

template<typename T>
void f2(T& param);

f1(someFunc); //函数指针 void (*)(int, double);
f2(someFunc); //函数引用 void (&)(int, double);
```



**条款2：理解`auto`类别推导**

总的来说，`auto`推导和模板推导是一个原理。

仅有一处例外：`auto`能够推导出`initializer_list`,而模板不能。

```cpp
auto x = {1, 2, 3}; //x type : initializer_list
template<typename T>
void f(T param);

f({1, 2 ,3}); // error can't detect.
```

不过，模板可以推导`initializer_list<T>` T 的类型。

```cpp
template<typename T>
void f(std::initializer_list<T> initList);

f({1, 2, 3});
```

但在如下情况，`auto`也不能推导。

1. 函数返回值。

    ```cpp
    auto createInitList() {
        return {1, 2, 3, 4}; //error
    }
    ```

2. lambda函数形参

    ```cpp
    auto resetV = [&v](const auto& newValue) { v = newValue; };
    resetV({1, 2, 3}); //error
    ```



**条款3：理解`decltype`**

一般来说，`decltype`推导出来的就是被推导的类型，包括`const`，`volatile`、引用等特性。

```cpp
template<typename Container, typename Index>
auto authAndAccess(Container& c, Index i) -> decltype(c[i]) { //有问题，auto会移除引用的特性
    return c[i];
}

vector<int> vec{1, 2, 3};
authAndAccess(vec, 1) = 10; //会出问题，因为返回值移除了引用特性，等号左边是一个右值。
//(读者注：本人测试的时候，authAndAccess返回的是引用类型。
```

搭配`decltype`即可保留引用的特性。

```cpp
template<typename Container, typename Index>
decltype(auto) authAndAccess(Container& c, Index i) { //C++14 feature。
    return c[i];
}
```



值得一提的是，`decltype`在如下情况不是预期值。

* C++硬性规定最小取址单位为字节，`vector<bool>`的元素不是`bit`大小，使用的是代理类模拟其他`vector`实现的功能。

* 对对变量加上小括号即为引用类型。

  ```cpp
  int x = 10;
  decltype((x)) ref = x;//ref's type is int&
  ```

  

**条款4：掌握查看类别推到结果的方法**

* 利用IDE、`type_inex`查看类型，但有可能出错。



## 第二章：auto

**条款5：优先选用`auto`，而非显示类别声明**

* `auto`变量必须初始化，可以避免一些打字出错导致的隐式类型转换。也可以简化重构流程。
* `auto`变量具有条款2和条款6所说的毛病。



**条款6：当auto推导不符合要求时，显式声明**

* 隐私类型转换会导致`auto`推导不符合预期。如条款2中提到的`vector<bool>`元素，不能使用`auto`。
* 使用`static_cast`强制类型转换成预期类型。



## 第三章： 转向现代C++

**条款7：在创建对象时区分`()`和`{}`**

* 大括号初始化可以阻止隐式窄化类别转换，还有解析语法友好。
* 在构造函数重载决议期间，含`initializer_list`的f构造函数有特别高的匹配优先级。



**条款8：优先选用`nullptr`，而非`0`或`NULL`**

* 防止模板推导和一些隐式转换的问题。



**条款9：优先选用别名声明，而非`typedef`**

* `tyepdef`不支持模板化，但类型别名`using`支持。
* 别名模板可以免写`::type`后缀，内嵌时也需要加上`typename`前缀。



**条款10：优先选用限定作用域的枚举类型，而非不限作用域的枚举类型**

* 限定作用域的枚举类型仅在枚举类别内可见。它们只能通过强制类型转换到其他类型。
* 两种都支持指定底层类型。限定作用域的默认为`int`，而不限的则没有默认的。
* 限定作用域的可以前置声明，而不限范围的不行。



**条款11：优先选用删除函数，而非`private`未定义**

* 任何函数都可以删除，包括非成员函数和模板特例。



**条款12：为意在重写的函数添加`override`**

* 成员函数引用饰词（`&` 和 `&&`）使得对于左值和右值对象（`*this`）能够区分。

  ```cpp
  auto val = makeWidget().data(); //对于右值应该有不一样的行为。
  
  class Widget{
    void f() &; //自身是左值Widget左值调用
    void f() &&; //自身是右值Widget右值调用
  };
  ```

  

**条款13：优先选用`const_iterator`，而非`iterator`**

* 由于C++11未对某些容器做`const_iterator`(C++14有), 对于特定情况下可用非成员版本的`begin`，`end`和`rbegin`等。



**条款14：只要函数不会发射异常，就为其加上`noexcept`声明**

* 对于移动操作，`swap`，内存释放函数和析构函数最有价值。



**条款15：只要有可能使用`constexpr`，就用**



**条款16：保证`const`成员函数的线程安全性**



**条款17：理解特殊成员函数的生成机制**

* 含默认构造函数，析构函数，复制操作（复制构造和复制赋值），移动操作（移动构造和移动赋值）。生成的前提都是没有显式生成。
  * 默认构造函数：没有显示声明任何构造函数。
  * 复制构造：没有声明移动操作，但已经存在析构或复制赋值情况下生成已成为被废弃（deprecated）的行为
  * 复制赋值：没有声明移动操作，但已经存在析构或复制构造情况下生成已成为被废弃（deprecated）的行为。
  * 移动操作：五个函数都不含。
* **成员模板在任何情况下都不会抑制特殊成员函数的生成（如完美转发可能出现的问题）**。



## **第四章：智能指针**

**条款18：使用`unique_ptr`**

* 小巧、高速、只移的智能指针。
* 推荐使用工厂方法`make_unique`生成，但不能指定删除器。
* 可转化为`shared_ptr`



**条款19：使用`shared_ptr`**

`std::enable_shared_from_this<T>`的使用。

```cpp
class Widget;
vector<shared_ptr<Widget>> task;

class Widget {
    void process(vector<int>& ) {
        /*
         * do some init work;
         */
        task.emplace_back(this); // wrong!!! 
        // this will create more than one shared_ptr from the same raw ptr;
        // delete more than once.
    }
};
```

用法

```cpp
class Widget;
vector<shared_ptr<Widget>> task;

class Widget: public std::enable_shared_from_this<Widget> {
    void process(vector<int>& ) {
        /*
         * do some init work;
         */
        task.emplace_back(shared_from_this); 
    }
};
```



**条款20：对于类似但可能空悬的指针使用`std::weak_ptr`**

Usage:

```cpp
std::weak_ptr<Widget> wpw(spw);
spw = nullptr; //deconstructor.
if (wpw.expired()) {
    ...
}

std::shared_ptr<Widget> spw1 = wpw.lock(); //若wpw失效， 则spw1为nullptr。
atuo spw2 = wpw.lock(); //同上

std::shared_ptr<Widget> spw3(wpw); //若wpw失效，则抛出异常。

```

**优势**

1. 缓存读取
2. 观察者列表
3. 避免环路



**条款21：优先选用`std::make_unique`和 `std::shared_ptr`而非直接构造**。

* 将`new`和智能指针的步骤整合。但无法自定义析构器
* 由于经过了一次完美转发，因此无法使用大括号初始化（见条款2）。



**条款22：使用Pimpl习惯用法时，将特殊成员的定义放在实现文件中**

* Pimpl模式降低类的设计者和使用者的依赖性
* 使用采用`std::unique_ptr`来实现pImpl指针，须在类的头文件中声明特种成员函数，但在实现文件中实现他们。即使默认的函数具有正确的行为。
* 上述建议仅仅适用`unique_ptr`但不适用`shared_ptr`



## 第五章：右值引用、移动语义和完美转发

**条款23：`std::move`和 `std::forward`**

* `move`实施的是无条件的强制右值转换，就本身而言，并不执行移动操作。
* 仅当传入的实参被绑定到右值时，`std::forward`才执行该实参向右值类型的强制类型转换。
* 运行期，`std::move`和`std::forward`都不会执行任何操作。



**条款24：区分万能引用和右值引用**

* 如果函数模板具备`T&&`类型并且T的类别由推导而来，或如果对象使用`auto&& `推导，则为万能引用。
* 反之为右值引用。
* 万能引用：区分左值右值，左值推导为左值，右值推导为右值。



**条款25：针对右值引用使用`std::move`,万能引用使用`std::forward`**



**条款26：避免依万能引用进行重载**

* 把万能引用作为重载候选类别时，**几乎总会意外被调用**。
* 特别是完美转发构造函数。



**条款27：条款26的替代方案**

略...



**条款28：理解引用折叠**

* 在四种语境下：模板实例化，auto类型推导，创建和使用`typedef`和别名(`using`)声明，以及`decltype`
* 上述语境下，引用的引用会折叠为一个引用。



**条款29：假定移动操作不存在、成本高、未使用**

* 提高兼容性
* 如果明确支持移动，则无需假定。



**条款30：完美转发失败的情形**

* 大括号初始化物`{}`
* `0`和`NULL`作空指针。
* 仅有声明的整形`static const`成员变量（常量无法取址，由于引用底层实际是指针，需要地址）。
* 转发给重载的函数名字和模板名字（可手动指定`static_cast`成指定函数）。
* 位域（位域单位为`bit`，C++规定非`const`不能指定到单个`bit`，因为操作最小单位为1byte）。



## 第六章：lambda表达式

**条款31：避免默认捕获模式**

在类中捕获成员，实际捕获的是`this`。当该对象被析构时，行为未定义。

（经测试，有些编译器会给出编译错误: "'this' cannot be implicitly captured in this context"）。

```cpp
vector<function<void()>> vec;
//经测试，读者的编译器会给出编译错误: "'this' cannot be implicitly captured in this context"。
class Widget {
public:
    int val = 10;
    void f()  {
        vec.emplace_back([] () {
            cout << val << endl;
        });
    }
};

//error
class Widget {
public:
    int val = 10;
    void f()  {
        vec.emplace_back([=] () {
            cout << val << endl;
        });
    }
};
```

**解决办法:**

```cpp
//本地copy
class Widget {
public:
    int val = 10;
    void f()  {
        int copy = this->val;
        vec.emplace_back([=] () {
            cout << copy << endl;
        });
    }
};

//初始化捕获，也称广义捕获
class Widget {
public:
    int val = 10;
    void f()  {
        vec.emplace_back([val = this->val] () {
            cout << val << endl;
        });
    }
};
```



**条款32：使用初始化捕获将对象移入闭包**

```cpp
//初始化捕获，也称广义捕获
class Widget {
public:
    int val = 10;
    void f()  {
        vec.emplace_back([val = this->val] () {
            cout << val << endl;
        });
    }
};
```



**条款33：对`auto&&`类别的形参使用`decltype`和`forward`**

```cpp
forward<decltype(param)>(param);
```



**条款34：优先选用lambda，而非`bind`**



## 第七章：并发API

这部分建议阅读：*C++ Concurrency in Action: Practical Multithreading* by Anthony Williams.

My Note: [messenger1th/Concurrency (github.com)](https://github.com/messenger1th/Concurrency)



**条款35：优先选用基于任务而非基于线程的程序**

* 任务是更高层的API，可以获取运算线程的结果。而基于线程，则无返回值。

* 基于线程，需要手动管理线程耗尽，超订，负载均衡，以及新平台适配。
* 基于任务，启动方式多样，更自由。



**条款36：如果异步是必要的，则指定`std::launch::async`**

**条款37：使`std::thread`型对象所在路径皆不可联结**



**条款38：对变化多端的线程句柄析构函数行为保持关注**

**条款39：考虑针对一次性时间通信使用`void`为模板类型实参的`promise`**

**条款40：对并发使用`std::atomic`,特种内存使用`volatile`**



## 第八章：微调

**条款41：针对可复制的形参，在移动成本低且一定会被复制的前提，考虑按值传递**

* 实际上，需要对应用场景进行具体评估。



**条款42：考虑`emplace`而非插入**

* 直接使用参数构造，而非构造后移动或复制。



## 读者总结

前面几章能够理解，后面的并发有些抽象和底层了，我感觉不如*C++ Concurrency In Action*来的透彻。
