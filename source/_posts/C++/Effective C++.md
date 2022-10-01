# Effective C++

## 熟悉C++ Accustoming Yourself to C++

**条款1：View C++ as a federation of languages.**

* C
* Objected-Oriented C++
* Template C++
* STL

根据要使用的C++部分，来遵守相应的高效编程守则。



**条款2：Prefer consts， enums, and inlines to #defines.**

`#define`是预处理命令，编译器看不它的存在。

* 对于常量， 最好用`const`对象或者`enums`替换`#define`
* 对于形似函数的宏，改用`inline`函数。



**条款3：Use const whenever possible**

```C++
char greeting[] = "Hello";
char* p1 = greeting;      //non-const pointer, non-const data
const char* p2 = greeting;//non-const pointer, const data 
char* const p3 = greeting;//const pointer, non-const data
const char* const p4 = greeting; //const pointer, const data
```

必要时，使用`mutable`修饰变量，使其不受`const`限制。



**条款4：Make sure that objects are initialized before they're used.**

* 区分`assignment`和`initialization`，并推荐使用列表初始化。

* 为了避免跨单元之初始化次序的问题，请使用`local static`对象替换`non-local`



## 构造/析构/赋值运算 Constructor，Destructor，and Assignment Operators

**条款5：Know what functions C++ silently writes and calls.**

编译器可以默认创建`default constructor`,`copy constructor`,`copy assignment`,`destrutor`

除了**含有const或者是声明为private的父类函数需要被调用时**.



**条款6：Explicitly disallow the use of complier-generated functions you do not want.**

* 为了防止编译器自动生成上述函数，可以将成员函数声明为private并不予实现。

 

**条款7：Declare destructors virtual in polymorphic base classes.**

* 让声明`virtual`析构函数被动态绑定，防止子类资源没有释放。



**条款8：Prevent exceptions from leaving destructors.**

以防止

* 未定义的行为
* 程序过早地结束



**条款9： Never call virtual functions during construction or destruction.**

在构造期间， `virtual`函数不是`virtual`函数。

因为基类构造时，意味着子类还未初始化， 如果调用的是子类`virtual`版本，则会导致未定义的行为。

析构函数同理。

如果要实现`virtual`函数的功能，可在构造时，由子类向父类传递信息。



**条款10：Have assignment operators return a reference to *this.**

如 operator=，+=, /=, *=/, -=。

```cpp
int x, y, z;
x = y = z = 15;
```

为了实现连锁复制。



**条款11：Handle assignment to self in operator=.**

考虑**异常安全**和**自我赋值安全**（释放资源前保证以后不再使用，如释放自己再使用自己的资源）；

使用copy and swap计数。



**条款12：Copy all parts of an object**

* Copying函数应该确保复制“对象内的所有成员变量”以及“所有base class”成分。

* 不要尝试以某个copying函数实现另一个copying函数。应该将共同部分放在第三个函数， 并由Copying函数调用。



## 资源管理 Resource Management

**条款13：Use objects to manage resources.**

* 为了防止资源泄露，请使用RAII对象。构造时获取资源，析构时自动释放资源。
* 两个常用的RAII是`tr1::shared_ptr`和`auto_ptr`。`auto_ptr`复制动作会使原对象指向`null`；注：**`unique_ptr`已经取代`auto_ptr`**



**条款14：Think carefully about copying behavior in resource-managing classes.**

* 复制RAII对象必须一并复制它所管理的资源，其copying行为由资源的copying行为决定。
* 常见RAII class copying 行为是：抑制copying、使用reference counting



**条款15：Provide access to raw resource-managing classes.**

* APIs往往要求访问raw resource，所以RAII需要提供接口。
* 对*row resource*的访问可能仅由显示转换或隐式转换。



**条款16：Use the same form in corresponding uses of new and delete.**

* 如`new int[]` 对应`delete[] `



**条款17：Store newed objects in smart pointers in standalone statements.**

* 为了防止资源`new`之后，还未放入`RAII`的时间内， 由于其他函数的异常导致的资源泄露
* 请将资源`new`和放入`RAII`的操作放在独立语句中。



## 设计与声明 Design and Declaration

**条款18：Make interfaces easy to use correctly and hard to use incorrectly**

* 使用已经成熟类封装数据，如`Month`,`Day`,`Year`类来封装日期。
* 一旦类型的值是确定的，就可以限制其取值。如一年12个月，使用预定义的12个`static`函数返回对象。
* 限制类型内可做的事情。如`a * b = c`，让`a * b`返回一个`const`.
* 保持接口一致性，如命名规范，大小统一命名为`size`.
* 预判使用者的行为导致的一些错误。如使用者忘记释放资源。可以先发制人，直接返回一个`smart pointer`
* **综上，“促进正确使用”：接口一致性，与内置类型兼容。“阻止误用”：建立新类型限制类型上的操作，限制对象的取值，消除客户的资源管理责任**。
* `tr1:shared_ptr`支持定制删除器`custom deleter`。可以防范DLL问题，可被用来自动解除互斥锁。



**条款19：Treat class design as type design**

* 如何被构造和析构，涉及到资源分配和资源释放的设计。
* 初始化和赋值行为有什么区别。
* 对象如果被`passed by value`意味着什么？ copy构造函数用于定义一个type的*pass by value*该如何实现？
* 考虑合法值的取值。并限制非法值。
* 是否需要配合继承体系？
* 需要什么样的类型转换？
* 什么样的操作符和函数对此*type*是合理的？
* 什么样的标准函数应该被驳回？那些必须声明为*private*
* 哪个成员是*public*,*protected*,*private*,*friend*.
* **什么是新*type*的未声明接口？**
* 是否要定义为*class* *template*
* 真的需要一个新*type*吗， 还是只是新的*derived* *class*?



**条款20： Prefer pass-by-reference-to-const to pass-by-value**

* 无需拷贝，更为高效，且可以避免切割问题（子类传递后拷贝为父类）。
* 但内置类型以及STL的迭代器和函数对象，通常使用 *pass-by-value*更为高效。



**条款21：Don't try to return a reference when you must return an object**

* 绝不要返回*pointer*或*reference*指向一个*local stack*对象。
* 考虑返回*reference*指向一个*heap-allocated*对象.
* 考虑拷贝返回。



**条款22：Declare data members private**

* 尽可能将成员变量声明为*private*.
* *protected*并不比*public*更有封装性。



**条款23：Prefer non-member non-friend function to member functions**



**条款24：Declare not-member functions when type conversions should apply to all parameters.**

```cpp
Rational oneHalf(1, 2);
Rational result = 2 * oneHalf;
Rational result = oneHalf * 2;
```

重载`operator*`并让隐式转换成两个`Rational`对象相乘。对接上一个*non-member function*.



**条款25：Consider support for a non-throwing swap**

* 当`std::swap`对你的类型效率不高时，提供一个`swap`成员函数，并确定它不抛出异常。
* 如果你提供一个*member swap*，也该提供一个*non-member swap*来调用前者
* 调用*swap*应该针对`std::swap`使用`using`声明式，然后调用`swap`并且不带任何“命名空间修饰”。
* 为“用户自定义类型”进行*std templates*全特化是好的，但不要在*std*内加入。



## 实现 Implement

**条款26：Postpone variable definitions as long as possible**

* 可以的话，直接初始化。而不是默认初始后再赋值。
* 考虑循环里头直接构造，还是在外头构造，循环内赋值的成本。



**条款27：Minimize casting**

`const_cast<T>(expression)` 移除`const`。

`dynamic_cast<T>(expression)` 

`reinterpret_cast<T>(expression)`

`static_cast<T>(expression)` 强迫转型，如`int`转成`double`。*pointer-to-base*转成*pointer-to-derived*.



**条款28：Avoid returning "handles" to object internals.**

* 可能会降低封装性。
* 可能会导致使用已经释放的资源。



**条款29：Strive for exception-safe code**

* 基本保证：不发生资源泄露和数据败坏。
* 强烈保证：要么执行成功，要么直接回滚到不执行的状态。
* 不抛出保证。



**条款30：Understand the ins and outs of inlining**

* *inline*函数在编译器直接将函数本体到替换到调用处。
* 因此，对于非inline函数来说，修改后重新编译链接这一个文件即可。而inline函数是需要重新编译所有调用文件的。
* 构造函数和析构函数含有不少代码，不建议*inline*



**条款31：Minimize compilation dependencies between files**

* 支持”minimize compilation dependencies “的一般构想是：相依与声明式，不要相依与定义式。
* 程序库头文件应该以”完全且仅有声明式子“（full and declaration-only forms）的形式存在。这种做法不论是否涉及*templates*都适用。



## 继承与面向对象设计 Inheritance and Object-Oriented Design

**条款32：Make sure public inheritance models "is-a"**

* "public继承"意味着*is-a*;



**条款33：Avoid hiding inherited names.**

* *derived classes*内的名词会掩盖*base  classes*的名称，即**名字查找先于类型检查。**
* 要用到被掩盖的名称，可以使用 *using*声明（全部显形）或者转交函数（*forwarding function*）（指定显形）。



**条款34：Differentiate between inheritance of interface and inheritance of implementation**

* *pure virtual*函数在基类可以有定义，但子类仍需要重写，必要时可以显示调用基类的定义。



**条款35：Consider alternatives to virtual functions**

* NVI: non-virtual interface  基类中实现一个函数，调用相应*virtual*。
* *Strategy*  使用可调用的方式来替代*virtual*函数如 使用函数指针（但只能访问类中*public*的内容），或使用`tr1:function`调用（可调用物：可以是某个函数，函数对象，成员函数）。



**条款36：Nevenr redefine an inherited non-virtual function**

* 如果*override*一个 *non-virtual function*， 使用基类的指针或引用指向子类时候，调用的是基类（即声明类型）的版本（静态绑定）。
* 从 *is-a*的角度，基类的 *non-virtual function*应该具有不变性。不可被改变。



**条款37：Never redefine a functions' inherited default parameter value**

* 默认参数在编译期已经确定，是静态的。
* 由于 *default parameter value*是静态的，取决于声明类型，而*virtual function*则是动态的，重定义默认参数会导致使用动态的*function*却使用了声明类型的默认参数。
* 解决办法是 条款35中的替代*virtual*的方案。



**条款38： Model "has-a" or "is-implemented-in-terms-of" through composition**

* *has-a* 如一个人有名字，身份证号码，这些是 *has-a*的关系。
* *is-implemented-in-terms-of* 如一个*set*可以由list来实现。



**条款39：*Use private inheritance judiciously***

* *private inheritance*意味着 *is-implemented-in-terms-of* 。它通常比 *composition*的级别低。但是当 *derived class*需要访问 *protected base class*的成员，或重新定义 *virtual*函数的时候，这样做是合理的。
* 较于 *composition*，*private*继承可以使得 *empty base*最优化，占用更少空间。



**条款40：*Use multiple inheritance judiciously***

* 多重继承比单一继承复杂。可能导致新的歧义性，以及对*virtual*继承的需要。
* *virtual*继承会增加大小、速度、初始化（及赋值）复杂度等等成本。如果*virtual base classes*不带任何数据，将是最具有使用价值的情况。
* 多重继承的确有正当用途。如“涉及*public*继承某个*interface  class*”和“*private*继承某个协助实现的*class*”



## 模板与泛型编程 *Template and Generic Programming*

**条款41：Understand implicit interfaces and compile-time polymorphism**

* *classes* 和 *templates* 都支持接口（*interface*）和多态（*polymorphism*）。
* 对*classes*而言接口是显式的（*explicit*），以函数签名为中心。而多态则是通过 *virtual* 函数发生与运行期。
* 对*template*参数而言，接口是隐式的（*implicit*），基于有效表达式。多态则是通过*template*具现化和函数重载解析发生于编译期。



**条款42：Understand the two meanings of `typename`**

* 声明*template*参数时，前缀关键字`class`和`typename`可互换。
* 请用`typename`标识嵌套从属类型（如`typename std::interator...`）。但不得在基类列表或者成员初值列内作为 *base class*修饰符。



**条款43：Know how to access names in templatized base classes**

* 由于 *template class*可能被特例化而缺少共同函数，因此 *derived template class*会默认不寻找基类的函数。
* 解决办法3种：
  * 使用`this->`修饰该函数。
  * 使用`using`告诉编译器去基类寻找。
  * 使用`::`明确寻找哪个函数。



**条款44： Factor parameter-independent code out of templates**

* 这块我的代码量不够，没咋看懂。



**条款45：Use member function templates to accept all compatible types** 

* 请使用 *member function templates*生成“可接受所有兼容类型”的函数。
* 如果你声明 *member templates*用于“泛化*copy*构造”或“泛化*assignment*”。你还需要声明 *non-template*版本，否则编译器会帮你生成。



**条款46：Define non-member functions inside templates when type conversions are desired**

* *template*实参推导从不将隐式类型转换考虑在内。如条款24的例子在*template*的情况下行不通。
* `friend`的其他用法。



**条款47：Use traits classes for information about types**

* *Trait classes* 使得“类型相关信息”在编译器可用。它们以 *templates* 和 ”*template*特化“实现。
* *overloading*使得 ， *trains classes*可能在编译期对类型执行 *if...else*测试。



**条款48： Be aware of template metaprogramming**

* *Template metaprogramming*(TMP 模板元编程)可将工作由运行期移往编译期。得以实现早期错我侦测和更高的执行效率。
* TMP可被用来生成”基于政策选择组合“（*based on combinations of policy choices*)的客户定制代码，也可用于避免生成对某些特殊类型并不适合的代码。



## 定制`new`和`delete`

**条款49：Understand the behavior of the new-handler**



**条款50：Understand when it makes sense to replace new and delete**

* 有许多理由需要写一个自定的`new`和`delete`，包括改善效能、对*heap*运用错误进行调试、收集*heap*使用信息。



**条款51：Adhere to conversion when writing new and delete**

* *operator new*应该内含一个无穷循环，并在其中尝试分配内存。如果无法满足内存需求，就该调用 *new-handler*。它也应该有能力处理*0 bytes*申请。*Classes*专属版本则还应该处理”比正确大小更大的（错误）申请“。
* *operator delete*应该在收到*null*指针时不做任何事。*Classes*专属版本则还应该处理”比正确大小更大的（错误）申请“。



**条款52：Write placement delete if you write placement new**

* 写了*placement operator new*，要写对应版本的*delete*。
* 当你写了*placement new* 和 *placement delete*，请确定不要无意识地遮掩他们正常的版本。



## 杂谈：Miscellany

**条款53：Pay attention to compiler warnings.**



**条款54：Familiar yourself with the standard library，including TR1**



**条款55：Familiarize yourself with Boost**

* [Boost](http://boost.org)



## 读者总结

前面几部分是容易接受的，直到模板这里头涉及文件的依赖性、元编程。以及`new`和`delete`部分。

模板这块代码量不够，不能彻底理解。`new`和`delete`也是。



