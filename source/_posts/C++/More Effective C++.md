# More Effecctive C++

## 基本议题：*Basics*

**条款1： 仔细区别*pointers*和*references***


由于*reference*必须代表某个对象，因此必须被初始化。由此，*reference*不会成为`null`。

但指针有这种可能性。

* pointers可以不被赋初值
* pointers可以被重新赋值



**条款2：最好使用C++转型操作符**

四种

* `static_cast<>()`: 常用于隐式类型转换
* `const_cast<>()`：去除或添加常量性
* `dynamic_cast<>() `用于多态的类型中，基类与子类的互转。
* `reintepret_cast<>()`：最常用于函数指针转换。不具有移植性。



**条款3：绝对不要以多态（polymorphically）方式处理数组**

数组是根据当前位置，数组存放对象的大小，来计算下一个对象的位置。

继承体系中，子类很有可能会比基类占用更大内存。

因此，当使用多态方式处理数组时，内存偏移量很可能是错误的，无法找到下一个下标的对象的位置。



**条款4： 非不要不提供*default constructor***

在一定的场景下，没有指定的信息，构造出来的对象是无意义的。





## 操作符：*Operators*

**条款5： 对定制的“类型转换”函数保持警觉**

类型转换函数有转换构造和操作符转换等。尽可能声明为`explicit`来避免非预期的隐式转换。（可以主动使用`static_cast`转换）

```cpp
class obj{
public:
    explicit obj(int val): val(val) {}
    explicit operator double() {
        return val * 10.1;
    }
private:
    int val;
}; 
```



**条款6：区分*increment*和*decrement*的前置和后置形式**

前置和后置形式都应该以前置式为基础，只需维护前置形式即可。



**条款7：千万不要重载`&&`,`||`和`，`操作符**

就算可以被重载，也不应该无理由的进行重载。

因为无法令其行为像他们内置的行为一样（执行顺序，短路性质）。



**条款8：了解各种不同意义的 *new*和 *delete***

*operator new*和*new operator*是不一样的。我们经常使用到的`new`是*new opeartor*。它实际包括了，分配内存和构造对象两个过程。这个是不能重载的。

但可被显示分为两个过程，一个是分配内存（*operator new*），一个是构造对象（*placement new*）。

* 我们能重载的是`operator new`,这个函数仅仅负责分配内存，接收`size_t`类型，返回指向内存的`void* `指针。
* *placement new*无法被重载。

由于*new operator*会调用上面两个过程，因此我们可以重载`operator new`来自定义*new operator*部分行为。

**同时，由于可以将分配内存和构造对象分开。在需要多次使用一个临时对象的场景下，可以开辟一个该空间重复构造。从而节省大量分配内存，释放内存的时间**

同理，*delete operator*也分为析构对象和释放内存两个过程。对于不同分配方式的内存，需要不同地释放（*operator new* 和*operator delete*是对应的， *placement new*出来的对象需要手动析构）。

**析构对象**

* 当使用了*placement new*的时候，不能对那块内存使用*delete operator*，因为*delete opertor*会调用*operator delete*来释放内存。
* 但该内存的对象并非由*operator new*分配而来。因此对于*placement new*，我们只需要调用其对象的析构函数即可。

**释放内存**

* *heap*内存就`operator delete(ptr)`;
* *stack*内存就无需操作。



数组的new和delete遵循即可。

* `new`对应`delete`
* `new[]`  对应 `delete[]`



## 异常：Exception

**条款9：利用*destructors*避免泄露资源**

异常发生时，普通指针的delete操作可能不会被正常执行。

可以将其delete操作置于一个局部变量的destructor中。局部变量在离开作用域时会被析构。

即，*smart pointer*。 用智能指针的*destructots*释放其所指的*heap*内存。



**条款10：在*constructors*内阻止资源泄露（*rescource leak*）**

当内部需要*heap*资源时，需要注意，当构造函数中发生异常，可能会对已经分配好的heap资源无法析构释放。

理论上，该类的资源应该由该类的析构函数释放。但由于该对象未成功构造，因此无法被析构（C++规定不那么做）。

可以使用智能指针代替内置指针管理该类的成员，自动释放内存。



**条款11：禁止异常流出*destructors*之外**

有两点好处

* 避免`terminate`函数在`exception`传播过程的栈展开（*stack-unwinding*）机制中被调用。
* 保证*destructor*的所有步骤被执行。



**条款12：了解”抛出一个*exception*与”传递一个参数“或调用一个”虚函数“之间的差异**

* *exception objects*总是会被复制（第一次抛出时一定被复制）。如果是以*pass by value*，会被复制更多次。
* 总共有三种传递方式*catch by   value\ by reference\ by pointer*.(*pass by value*时可以是*non-const*，但函数传参不行)。
* 抛出的*exception*对象被允许的转型动作，比”被传递到函数去“的对象少。
* 抛出的exception对象*pass by value*时，总是根据其静态类型来*copy*。也有例外，见条款25。
* *exception objects*匹配顺序由*catch*顺序决定。



**条款13：以*by reference*方式捕捉*exception***

三种方式比较

***catch by pointer***

如果指向了局部变量，则导致*undefined behavior*，如果指向*heap*变量，便要纠结是否要删除该变量回收内存。

因此，不推荐这种方式。

***catch by value***

可以消除上述是否需要删除和局部变量的问题。但是，也会导致切割（*slicing*）。如

子类被基类捕获，则切割了子类特有部分。因此，调用的函数还是其基类的。

***catch by reference***

解决了catch by pointer的变量生存期和catch by value的slicing问题。

可以通过虚函数调用正确版本。



**条款14：明智运用exception specifications**

懂了大概意思，有些抽象。难以总结。



**条款15：了解异常处理（exception handing）的成本**



## 效率：*Efficiency*

**条款16：谨记80-20法则**

针对20的代码做优化，那才是效率瓶颈。

选用更好的分析器并尽可能以最多的数据分析你的软件。



**条款17：考虑使用*lazy evaluation***

需要用到时才进行相应操作。需要搭配其他技术如代理（*proxy*）



**条款18：分期摊还预期成的计算成本**

*Over-eager evaluation*：

* *Caching*：使用Cache缓存
* *Prefetching*：一次取出磁盘的大部分内容（可能他们都会被用到），而不是一块一块地读（改随机读为顺序读）；`vector`扩张时直接扩1.5倍。



**条款19： 了解临时对象的来源**

局部对象（如用于swap交换的temp变量）不是临时变量。临时变量来源有

* 隐式类型转换
* 返回值

注意*reference-to-const*参数，常常用于临时变量。但*reference-non-const*则不会（编译器认为改变临时变量达不到程序员的预期，因此拒绝）。



**条款20：协助完成”返回值优化“（RVO）** *return value optimization*

编译器为节pass by value的开支，优化了临时变量作为返回值（相当于直接构造）。



**条款21：利用重载（*overlaod*）避免隐式类型转换（*implicit type conversions*）**

注意，重载运算符时，必须至少有一个是非内置类型。

比如不能定义`Complex operator+(int lhs, int rhs);`。改变了int加法的本来含义。



**条款22：考虑以操作符复合形式（op=）取代其独身形式（op）**

通常独身形式一般基于符合形式实现，继续维护复合形式。

因此，复合形式具有更高效率。且，返回时，优先选择匿名对象。



**条款23：考虑使用其他程序库**

如stdio和iostream库的对比



**条款24：了解*virtual function*、*multiple inheritance*、*virtual base classes*、*runtime type identification*的成本**

对于多态，大部分编译器使用虚表（*virtual tables*）和虚表指针（*virtual table pointers*）实现。

具体实现见《深度探索C++对象模型》（*Inside the C++ Object Model* byStanley B.Lippman）



## 技术： *Techniques, Idioms, Patterns*

**条款25：将*constructor*虚化和*non-member functions*虚化**

并非是把*constructor*声明为*virtual*，而是使用虚函数来实现*virtual constructor*的功能。

考虑这样一个场景。Base为消息，Derive有视频消息，文字消息等。使用基类指针来拷贝实际类型，就要根据实际类型来拷贝，即虚函数。

**copy constructor**

```cpp
class Base{
public:
    virtual Base* clone() const = 0;
};

class Derive1: public Base{
public:
    virtual Derive1* clone() const {
        return new Derive1(*this);
    }
};

class Derive2: public Base{
public:
    virtual Derive2* clone() const {
        return new Derive2(*this);
    }
};
```

**non-member function**

```cpp
class Base{
public:
    virtual ostream& print(ostream& s) const = 0;
};

class Derive1: public Base{
public:
    virtual ostream& print(ostream& s) const {
        s << "Derived1" << endl;
    }
};

class Derive2: public Base{
public:
    virtual ostream& print(ostream& s) const {
        s << "Derived2" << endl;
    }
};

inline ostream& operator<<(ostream& s, const Base& o) {
    return o.print(s);
}
```



**条款26：限制某个class所能产生的对象**

实现单例模式的一种简单想法是，将构造函数声明为private或 `= delete`,使用一个`static`函数（以下称为工厂函数）来创建该类的`static`对象，用于返回该类对象的引用。

工厂函数不能声明为`inline`，因为`inline`会在多处展开，生成多个副本，就破坏了单例。（注，后续已经修复。无需考虑这点问题）



另外一种限制数量的方式是，使用一个计数器。超过即抛出异常。仅仅从自身角度来说，这是可行的。

但要考虑其他两种情况。**派生出子类**和**被其他类包含**。这样，也会产生计数器的计数（调用了构造函数），就算他理论上不应该被计数（不是同一类东西）。

因此，需要将构造函数声明为*private*，结合工厂函数和智能指针管理。



另一种实现方式是，使用**一个用于计算对象个数的*Base Class***，采用*private inheritance*，即*has-a*的形式。



**条款27：要求（或禁止）对象产生于*heap*之中**

太抽象了，且用法都比较Trick不具有移植性。



**条款28：智能指针（*Smart Pointers*）**

**测试*Smart Pointer*是否为*nullptr***

一种做法是提供一个`isNull`函数，但为了尽可能模仿原指针的行为，另一种做法是，进行隐式转换。

但转换为其他类型（如`bool`，`void*`等）都不能避免不同类型的互相比较问题（由于隐式转换可能会转成相同的类型）。

C++标准库中，隐式转换为`void*`已经被`bool`取代，而`operator bool`总是返回`operator!`的反相。

源码中与nullptr的比较。

```cpp
typedef decltype(nullptr) nullptr_t;
template<typename _Tp>
  inline bool
  operator==(const shared_ptr<_Tp>& __a, nullptr_t) noexcept
  { return !__a; }

template<typename _Tp>
  inline bool
  operator==(nullptr_t, const shared_ptr<_Tp>& __a) noexcept
  { return !__a; }
```

**将*Smart Pointer*转换为*Dumb Pointers***

一种形式是，提供隐式类型转换。但容易导致意料之外的转换。

标准库提供`get()`来返回原指针。



**Smart Pointers  和 ”与继承有关的“ 类型转换**

指向基类的智能指针和指向子类的智能指针之间没有继承关系，无法隐式转换。

但我们可以使用*template*来实现。

```cpp
template<class newType>
operator SmartPtr<newType>() { //template function, 用于隐式类型转换操作符。
    return SmartPtr<newType> (rawPtr);
}
```

实际上，任何raw1指针可以转换成raw2指针的行为，对应的smart pointer也可以通过转成raw指针再实现转换。



**Smart Pointer 与 Const**

raw指针所指之物可以被const修饰，但智能指针不行。取而代之的是`SmartPtr<const Obj>`;

如同上面template实现转换，const的转换也可以同理实现。

或者使用继承和C Part of C++中的Union实现。



**条款29：Reference counting**

实现一个Reference Counting基类，方便子类实现*copy-on-write*功能。

将计数的操作全部封装在基类之中，子类只需判断是否可以共享即可。

注：RCPtr可使用库Smart Pointer实现。

```cpp
class RCObject{
public:
    void addReference();
    void removeReference();
    void makeUnsharable();
    bool isShareable();
    bool isShared();

protected:
    RCObject();
    RCObject(const RCObject& rhs);
    RCObject& operator=(const RCObject& rhs);
    virtual ~RCObject() = 0;

private:
    int refCount;
    bool shareable;
};


class String{
public:
    String(const char *value= "");
    const char& operator[](int index) const;
    char& operator[](int index);

private:
    struct StringValue: public RCObject{
      char *data;
      StringValue(const char *initValue);
      StringValue(const StringValue& rhs);
      void init(const char *initValue);
      ~StringValue();
    };

    shared_ptr<StringValue> value;
};
```

同理，为了让已经实现好的类在不改变源代码的情况下，实现Reference Counting功能，只需再提供一层封装性。

需要实现一个间接智能指针，智能指针内含一个原生指针，该原生指针指向一个结构体，该结构体继承自`RCObject`，且内含有目的类的原生指针。



**条款30：Proxy classes**

使用*Proxy*可以用于区分`operator []`的读和写，使得*copy-on-write*技术更完善。但是，为了让*Proxy Class*尽可能地像原*Class*，需要重载大量操作符使得其像原*Class*（如`&`，`+=`，`+`，`-=`等）。

同时，由于可能多一层隐式转换，使得原来能够实现的转换，在多一层的情况下，无法进行。

此外，*Proxy Class*需要承担一定的构造和析构成本。



**条款31：让函数根据一个以上的对象类型来决定如何虚化**

这条东西很多，主要关于”动态确定多个类型“，给出了一些解决方案。

* 虚函数+RTTI
* 只使用虚函数
* 自行仿真虚函数表
* ”继承“ + ”自行仿真的虚函数表格“





## 杂谈：Miscellany

**条款32：在未来时态下发展程序**

考虑程序后续的扩展性，如被派生可能导致的问题。



**条款33：将非尾端类（*non-leaf-classes*）设计为抽象类（*abstract classes*）**

此条款里头讨论可能导致的问题，如

* 子类之间的部分赋值。
* 不同的子类之间的异型赋值。

挺抽象的，也没给出完美解决方案。



**条款34：如何在同一个程序里头结合C++和C**

C无重载，编译器不会对函数名字进行修改。

但C++有重载，会有*name Mangling*现象。使用 `extern ”c“`来避免。

简单守则如下

* C++与C的兼容需要编译器产出兼容的目标文件（Object file）；
* 双方函数都声明为`extern ”C“`；
* 如果可能，尽量在C++中撰写`main`
* `delete`和`new`，`free`和`malloc`需要配套。
* C++ `struct`仅在无虚函数的情况下兼容C。



**条款35：让自己习惯于C++语言**



## 读者总结：Summary

通篇读下来，C++给我的感觉是，一个很硬核的语言，可以自己定制各种功能，事实上，C++也是由各类开发者发展完善的（如Boost社区）。

实现一个具有优良性质的设计，需要很深厚的经验和长远考虑等。
