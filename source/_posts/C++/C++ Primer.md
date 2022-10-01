
# C++ Primer
## 第二章: Variable

1. 头文件不应包含`using`声明，因为头文件会被`copy`到所有引用其的文件中。

### decltype(expresssion)

* 类型推导
* 如果expression含括号，则一定是引用类型。

```cpp
int i = 0;
int& j = i;
decltype(i) a = 0; //int
decltype(j) b = i; //b是引用
decltype((i)) c = a; //c是引用类型
```

## 第三章：String Vector Array

### String

> 列表初始化和拷贝初始化、直接初始化的区别

`string::size_type`和`size_t`有助于代码的可移植性。 根据不同平台，编译成不同大小。

### Vector

1. `vector`对象能高速增长，除了所有元素都一样的情况，没必要定义时指定容量。

### Array

1. 定义（有括号优先括号，其次右向左读）

   ```Cpp
   int *ptrs[10]; //是一个数组， 里头10个指针
   int &refs[10] = ; // 错误，不存在引用数组
   int (*Parray)[10] = &arr; // 指针，指向一个数组，数组包含10个元素
   int (&arrRef)[10] = arr; //引用，引用一个数组，数组包含10个元素
   int *(&array)[10] = ptrs //引用，引用一个数组，数组包含10个元素，数组元素是指针。
   ```

2. 编译器编译,`auto`,为指针，`decltype()`为原类型。

   ```cpp
   int arr[] = {0,1,2,3};
   auto arr2(arr); //指针
   decltype(arr) arr3() = {0,1,2,3}; //int型数组
   ```

3. 指针也是迭代器，可获取尾后迭代器遍历数组。但建议使用`begin()`和`end()`获取迭代器。

   ```cpp
   int arr[] = {0,1,2,3,4};
   int *first = begin(arr);
   int *last = &arr[5] //不建议
   int *last = end(arr); //建议
   ```

4. 指针相减得到数据类型`ptrdiff_t`, 为有符号类型
5. 静态多维数组需要使用引用。

	```cpp
	int arr[][2] = {0,1,2,3};
	for (auto& row: arr) { //如果不是引用，编译器认为是 int*, col无法遍历。
	    for (auto col: row) {
	        cout << col <<endl;
	    }
	}
	```



## 第四章：Expression

1. 左值和右值

  ```cpp
  int a = 0;
  int *p = &a;
  decltype(*p) //*p, 解引用。为int&
  decltype(&p) //&p取地址为右值。为int**
  ```

2. 求值顺序（CPP没有明确规定求值顺序，方便编译器优化）

   ```cpp
   int i = f1() * f2(); //不确定先算f2还是f1，未定义行为。
   //确定求值顺序运算符有四个：&& || , ? :
   ```

3. 位运算符

   ```cpp
   int a = 1111;
   a &= ~(a << 1);
   ```

4. `sizeof`

   ```cpp
   int *p;
   sizeof (*p); //没错，实际不对p解引用。返回p指向的大小。在这为int类型大小  
   sizeof (p); //返回p指针本身大小
   int arr[10] = {1,2,3};
   sizeof arr; //返回10，数组所占大小。
   string s{"Hello"};
   sizeof s; // 返回其固有所占空间大小，不随s长度改变。vector同理
   ```

5. 类型转换

      1. 隐式转换

      2. 显示转换`static_cast<>()`、`const_cast<>()`、`dynamic_cast<>()`、`reinterrpret_cast<>()`

            ```cpp
            float a = 1.0;
            double res = static_cast<double>(a); //
            const char *pc;
            char *p = const_cast<char*> (pc)//去除底层const，但是通过p写值是未定义的行为。
            ```



## 第六章：Function

1. `initializer_list<>`初始化列表

   ```cpp
   initializer_list<int> lst{1,2,3,4};
   vector<int> vec(lst);//{}实际上是一个初始化列表。
   //作为函数返回值，用于初始化被调用处的变量。
   vector<int> fun1() {
   	return {1,2,3,4};//实际上是一个初始化列表。
   }
   ```

2. 返回数组指针

   ```cpp
   //方法1
   int (*func(int i))[10]; //逐层理解
   func(int i); //函数需要一个int参数
   (*func(int i)); //函数返回值可以解引用
   (*func(int i))[10]; //函数解引用得到一个大小为10的数组
   int (*func(int i))[10]; //数组中的元素大小为10;
   //方法2：使用尾置返回类型
   auto func(int i) -> int (*)[10];
   ```

3. 调试帮助

   ```cpp
   #define NDEBUG //在源文件的第一句写,用于取消以下模式 
   //方法一
   assert(expr);  //expr为0则输出信息并终止程序.
   //方法二
   #ifndef NDEBUG
   	some opreation
   #endif
   ```

4. 函数指针

   ```cpp
   bool (*pf)(const string& a, const string& b);
   //(*pf)代表是一个指针,右边的参数列表表示指向一个函数.
   ```

5. 函数指针形参

   ```cpp
   void useBigger(bool (*pf)(const string& a, const string& b));
   useBigger(func)//调用时直接传函数名即可.
   decltype(func) * funcPtr; //通过decltype得到函数类型,加上*变成函数指针.
   //可以如下重新声明useBigger函数
   void useBigger(funcPtr);
   ```

6. 返回指向函数的指针

   ```cpp
   //使用别名
   using F = int(int*, int); //F是函数类型,不是指针.
   using PF = int(*)(int*, int); //PF是函数指针
   PF f1 (int);
   F *f2(int); //F 是一个函数,加上*变成函数指针.
   //直接声明
   int (*f1(int))(int*, int);
   //尾置返回
   auto f3(int) -> int (*)(int*, int);
   ```

   

## 第七章：Class

1. 对于公共代码使用私有功能函数

   1. 避免在多处使用同样的代码
   2. 随着规模发展, 函数的功能可能变得更复杂, 修改时只需修改一处即可.
   3. 可能在开发时有调试信息, 发行版去除函数处的即可.
   4. 不带来额外开销, __类的函数定义在内部会隐式声明成inline类型__.

2. 函数的参数列表可以告诉编译器目前处于哪个作用域

   ```cpp
   void Window_mgr:: addScreen(const Screen &s) {}
   // 但是返回值在这个作用域参数之前,因此需要指明.
   Window_mgr::ScreenIndex
   Window_mgr::addScreen(cosnt Screen &s) {return ...}
   ```

3. 编译器处理完所有声明才会处理成员函数的定义.

   1. 对于成员函数中的使用的名字, 可以使用类中所有的成员变量.
   2. 但对于声明, 返回值, 参数列表中使用的名字, 都必须保证使用前可见.

   ```cpp
   typedef double Money;
   string bar;
   class Account{
   public:
       //返回值类型是double, bar是下方的bar1.0, 而非上方的string.
       Money balance() {return bar};
   private:
       Money bar = 1.0;
   };
   ```

   

4. 成员初始化的顺序

   1. 实际顺序与其定义的顺序一致.

   2. 编码顺序可以不一致, 需要考虑如下问题

      ```cpp
      class X{
          int i;
          int j;  
          X(int val): j(val), x(j) {} //未定义行为, 因为实际顺序中, i先被初始化.
      };
      ```

5. 委托构造（构造函数调用其他构造函数）

   ```cpp
   class SaleData{
       string id;
       double price;
       SaleData(string id, double price): id(id), price(price) {}
       SaleData(): SaleData(0, 0) {}
       saleData(string id): SaleData(id, 0) {}
   };
   ```

6. 默认构造函数

   ```cpp
   SaleData obj(); //函数声明
   SaleData obj; //对象声明, 调用默认构造函数.
   ```

7. 转换构造函数

   1. 例子

   ```cpp
   string null_book = "9-999-9999-9"; //此处存在一次隐式转换 即const char *到string
   SaleData temp = null_book; 
   SaleData temp = "9-999-9999-9";// 不行, 只支持一步转换. 
   ```

   2. explicit关键字 阻止隐式转换

8. 聚合类

   1. 符合如下条件

      * 所有成员都是`public`的
      * 没有定义任何构造函数
      * 没有类内初始值
      * 没有基类, 也没有`virtual`函数

   2. 如

      ```cpp
      class Data {
      public:
          int val;
          string s;
      };
      ```

   3. 可以如下初始化

      ```cpp
      Data data1 = {0, "Anna"}; //赋值顺序与定义顺序一致
      ```

9. 字面值常量类(成员都是字面值的聚合类或者符合以下条件)

   * 数据成员必须是字面值类型
   * 类至少含有一个`constexpr`构造函数
   * 类内初始值为常量表达式, 或者初始值使用自己的`constexpr`构造函数初始化.
   * 析构函数必须默认

10. 类的静态成员

    * 定义在任何函数之外, 定义后存在于整个程序的生命周期.
    * __建议把静态数据成员的定义与其他非`inline`函数的定义在同一个文件中.__
    * 一般来说,不应该在类内初始化, 但可以提供为`const`静态成员提供类内初始值(必须是字面值或`constexpr`)

    ```cpp
    class Data {
    public:
        constexpr static int cnt = 30; // 正确
        static int count = 30; //报错, 非const的static变量的定义和初始化必须分开.
    };
    //正确
    class Data {
    public:
        static int count; //类内定义
    };
    int Data::count = 30; //类外初始化
    ```

    * 即使一个常量静态数据成员在类内部被初始化, 通常情况下也应该在类外定义一下.

    ```cpp
    class Data {
    public:
        constexpr static int cnt = 30;
    };
    constexpr int Data::cnt;
    ```

    * 静态数据成员类型可以是不完全类型. 

    * 可以作为默认实参

      ```cpp
      class Screen {
      public:
          Screen& clear(char = bkground);
      private:
          static const char bkground;
      };
      ```

* `mutable`可以突破函数`const`的限制, 任何时候都可改;

## 第八章: IO Library

1. 宽字符版本输入输出流 `wcin`, `wcout`,`wcerr`,与原版本在同一个头文件.

2. 流状态管理

3. 管理输出缓冲: __如果程序崩溃,输出缓冲区不会被刷新__

   ```cpp
   cout << unitbuf; //设置模式:任何输出都会立即刷新缓冲区
   cout << nounitbuf //恢复默认
   ```

   1. 关联输入和输出流

      > 关联后, 任何输入流读取操作都会先刷新关联的输出流.

      ```cpp
      cin >> ival; //标准库将cout和cin关联, 因此这会使得cout刷新.
      cin.tie(&cerr); // 关联cerr流.
      ```

4. 文件输入输出流

   1. `open`和`close`, 文件流对象被销毁时.自动调用`close`
   2. string流 :`istringstream`, `ostringstream`

## 第九章: Sequential Container

1. 顺序容器类型

   ```cpp
   vector; 
   deque; 
   list; //双向链表 
   forward_list; //单向链表
   array;
   string;
   ```

2. `capacity`和`size`

   1. `capacity`表示当前分配的空间最大容纳元素个数
   2. `size`是当前容器存储元素个数.

   ```cpp
   vector<int> vec;
   vec.reverse(n); //指定vec分配n个元素的内存空间. 此时capacity = n; 
   ```



## 第十章: Generic Algorithm

1. 只要元素支持相关操作,例如`+`, `==`即可调用泛型算法.

   ```cpp
   string sum = accumulate(v.cbegin(), v.end(), string("")); //支持+即可
   equal(v.cbegin(). v.end(), v2.cbegin()); //支持==即可
   ```

2. 算法不检查写操作

   ```cpp
   finll_n(vec.begin(), 10, 0); //若vec大小小于10, 则此操作未定义.
   ```

3. `lambda`

   ```cpp
   [capture list] (parameter list) -> returnType {
       function body
   }
   //capture list只用于局部非static变量, lambda可以直接使用局部static变量和在它所在函数之外声明的名字
   ```

   1. 值捕获和引用捕获

      ```cpp
      int a = 42;
      //显式捕获
      auto f = [a] { return a;} //value
      auto f2 = [&a] { return a;} //reference
      //隐式捕获
      auto f = [=] { return a;} //value
      auto f2 = [&] { return a;}// reference
      //capture list
      []
      [name1, names2...]
      [&]
      [=]
      [&, identifier_list]
      [=, identifier_lsit]
      ```

4. `bind`

   1. 使用`bind`取代lambda

5. 插入迭代器

   ```cpp
   back_inserter;
   front_inserter;
   inserter;
   ```

6. 算法命名规范

   ```cpp
   find(beg, end);
   find(beg, end, comp);
   //参数都是三个,避免重载歧义, 提供不同名字版本
   find_if(beg, end, pred);
   
   //拷贝和不拷贝
   reverse(beg, end);
   reverse_copy(beg, end, dest);
   
   remove_if(beg, end, [] (int i) {return i % 2 == 0;});
   remove_copy_if(beg, end, v2.beg(), [](int i) {return i % 2 ==0;} );
   ```

7. 特定容器算法

   对于`list`和`forward_list`优先使用成员算法.

   ```cpp
   sort;
   merge;
   remove;
   reverse;
   unique;
   ```

   链表的特有操作会改变容器

## 第十一章: Associative Container

1. 8种

   ```cpp
   map;
   set;
   multimap;
   multiset;
   unordered_map;
   unordered_set;
   unordered_multimap;
   unoreered_multiset;
   ```

2. 有序容器的元素需要提供`<`运算, 无序容器需要提供`hash function`和`==`

   ```cpp
   class Sale_data{
   public:
       string isbn;
   };
   
   size_t hashFunc(const Sale_data &t) {
       return hash<string> () (t.isbn);
   }
   
   bool eqOp(const Sale_data &t1, const Sale_data &t2) {
       return hashFunc(t1) == hashFunc(t2);
   }
   int main() {
       unordered_set<Sale_data, decltype(hashFunc)*, decltype(eqOp)*> test;
       cout << test.size() << endl;
       return 0;
   }
   ```

3. 无序容器桶管理

## 第十二章: Dynamic Memory

1. 动态内存与智能指针

   ```cpp
   shared_ptr;
   unique_ptr;
   
   shared_ptr<int> p = make_shared<int>(42);
   auto p1 = make_shared<int>(42); 
   //分配动态内存与删除
   int *p = new int[10];
   delete [] p; //数组要加[]
   ```

2. 使用动态内存的原因是允许多个对象共享相同的状态.

3. 相关`shared_ptr`的定义和修改

   ```cpp
   shared_ptr<T> p(q); //q是new出来的内存, 且能转成T*
   shared_ptr<T> p(u); //u是unique_ptr, 从u处接管对象所有权. u置空.
   shared_ptr<T> p(q, d); //q是内置指针,p接管其内存, 同时用d来代替delete来释放内存.
   shared_ptr<T> p(p2, d);//接管shared_ptr p2, 并用d释放内存.
   
   p.reset();
   p.reset(q);
   p.reset(q, d);
   ```

4. 传递删除器

   ```cpp
   unique_ptr<objT, decT*> p (new ObjT, fcn);
   ```

5. `weark_ptr`

6. 智能指针与动态数组

   ```cpp
   unique_ptr<int[]> up(new int[10]);
   up.release();//自动调用delete[]销毁其指针
   //shared_ptr 与 unique_ptr不同, 不支持管理动态数组, 需要自定义删除器.
   shared_ptr<int> sp(new int[10], [](int *p) {delete[] p});
   ```

7. `allocator`

   1. 分配未构造的内存, 将内存分配与对象构造分隔开.
   2. 分配与回收, 构造与析构
   3. 拷贝和填充分配但未构造的内存


## 第十三章: Copy Control

1. 拷贝构造函数和拷贝赋值运算符

   ```cpp
   class Foo {
   public:
       Foo() {}
       Foo(const Foo& o) {} //拷贝构造函数, 形参必须是引用, 不然会无限拷贝
       Foo& operator=(const Foo& o) {}//拷贝赋值
       
       //或默认生成, synthesized copy constructor
       //或显示赋值default, 如下
       Foo() = default;
       Foo(const Foo&) = default;
       Foo& operator=(const Foo& o); //在类外
       ~Foo() = default;
       
       //或删除
       Foo() = default;
       Foo(const Foo&) = delete;
       Foo& operator=(const Foo& o) = delete;
       ~Foo() = default;
   };
   Foo& Foo::operator=(const Foo& o) = default; //防止inline.
   ```

2. 需要析构函数的类也需要拷贝和赋值操作（显式生成拷贝构造函数、重载=运算符）,需要拷贝的类也需要赋值操作，反之亦然。

3. 类值拷贝赋值运算符

   ```cpp
   //需要注意当左右两侧是同一个对象时, 保证正确性和异常安全.
   HasPtr&
   HasPtr::operator=(const HasPtr& rhs) {
       delete ps; // 释放ps指向的动态内存
       //如果rhs和*this是同一对象, 行为未定义
       ps = new string (*(rhs.ps));
       return *this;
   }
   //copy and swap
   HasPtr::operator=(const HasPtr rhs) {
       swap(*this, rhs);
       return *this;
   }
   ```

4. 自定义`swap`函数而非`std::swap`交换指针, 防止冗余拷贝.

5. 右值引用 `&&`

6. 对象移动

   1. 容器保存的类不必可以拷贝, 但一定要可以移动.

   2. **移动构造函数**(需要dynamic memory除外, 可能分配失败)通常不会抛出异常, 需要声明成`noexcept`, 原因如下:

      ```cpp
      public:
      	StrVec(StrVec&&) noexcept {}
      };// 类头文件声明, 和定义都指定noexcept
      StrVec::StrVec(StrVec &&o) noexcept: {...};
      ```

      **原因:** 移动构造时, 在当前对象已经移动部分, 但未完全移动时发生异常, 容器就会发生改变.因此 容器就不会选用会抛出异常的移动构造函数, 而选用拷贝构造, 因为拷贝构造构造发生异常时, 旧元素仍然存在, 仅需释放新元素内存即可保持容器不变.

   3. **移动赋值运算符**

      1. 类似拷贝赋值, 需要确保自己给自己赋值的正确性.

7. 五个拷贝控制成员应该看成一个整体.

   1. 构造析构移动: 
   2. 赋值, 拷贝: 



## 第十四章: Overload Operator & Type Conversion

1. `function`类型

   1. 可通过函数, 函数指针 ,lambda, 函数对象构造.

2. 类型转换运算符

   ```cpp
   class SmallInt{
   public:
       SmallInt(int i = 0): val() {}
       operator int() const {} //转换成int, 通常设置成const
   private:
       int val;
   };	
   ```

3. 不要设计两个可以转换的类, 一个转换构造一个定义类型转换运算符

4. 不要定义多种内置算术类型的类型转换.

5. 转换构造和类型转换可能产生二义性，因此推荐转换构造

## 第十五章: Object Oriented Programming

1. `virtual` 

2. 动态绑定

3. `final` 防止被该类继承

4. 多态: `polymorphism`

5. 纯虚函数`pure virtual`, 有该类函数的类称为抽象基类`abstract base class`, 无法被直接定义.

   ```cpp
   class Disc_quete: public Quote {
   public:
       Disc_quote() = default;
       double net_price(std::size_t) const = 0; //纯虚函数只需声明, = 0即可
   }
   ```

6. 派生类的成员或友元只能通过派生类对象来访问基类的受保护成员. 基类对象不行.

   ```cpp
   class Base {
   protected:
      	int prot_mem;
   }
   class Sneaky : public Base {
       friend void clobber (Sneaky&); //能访问
       friend void clobber (Base&);//不能通过基类对象访问
   };
   ```

7. 继承方式:`public`,`proteced`&`private`;

   * 不影响基类的访问权限.

   * 仅仅控制派生类对象对基类的访问.

   * 通过`using`改变个别成员的可访问性. 前提是该成员可在派生类内访问.

     ```cpp
     class Derived: private Base {
     public:
         using  base::var;//改变base基类的var变量访问权限为public
     }
     ```

8. `friend`不能继承给派生类

9. 名字查找先于类型检查, 因此内层作用域的函数会隐藏外层的函数, 即使他们**参数列表不一致**, 

   1. 因此虚函数一定要有相同的参数列表.
   2. 内层同名函数会隐藏所有外层同名函数(可能是重载函数,因此可能有多个),可通过基类`::`访问. 

10. 虚析构函数

    1. 一个基类总是需要虚析构函数
    2. 会阻止合成移动操作.因此使用移动操作时, 实际使用的是拷贝操作.

11. 移动操作与继承

    1. 基类操作

        ```cpp
        class Base{
        public:
            virtual ~Base() = default; //基类通常需要定义虚析构函数
            //定义析构函数后, 不会默认合成移动操作. 需要显式定义,如下.
            Base() = default;
            Base(const Base& t) = default;
            Base(Base&& t) = default;
            Base& operator=(const Base& t) = default;
            Base& operator=(Base&& t) = default;
        };
        ```

    2. 派生类操作

        ```cpp
        class D: public: Base {
        public:
            //D(const D& t) = default;
         	//=default会使用默认初始化, 与我们的期望相违背.
            D(const D& t) = default;
            D(D&& t) noexcept : Base(std::move(t)) {};
            D& operator=(const D& t);
            D& operator=(const D&& t);
        
        }
        //拷贝和赋值
        D& D::operator=(const D& t) {
            Base::operator=(t);
            //...
            return *this;
        }
        D& D::operator=(D&& t) {
            Base::operator=std::move(t);
            //...
            return *this;
        }
        ```

12. "继承"构造函数, 继承除了

    1. 默认, 拷贝, 移动构造函数

    2. 派生类中与基类参数列表相同的构造函数.

       ```cpp
       class D: public Base {
       public:
           //复制基类的构造函数到派生类, 派生类的成员执行默认初始化.
           //且不影响派生类默认构造函数的生成.
           using Base::Base;
           //之前的using声明改变了访问权限, 此处不改变, 编译器仅生成代码.
       };
       ```

13. 容器与继承

    1. 在容器中放置(智能)指针而非对象.

    2. 模拟虚拷贝: `new Base(obj)` `obj`可能是派生类, 因此可能会仅拷贝`Base`的部分

       ```cpp
       virtual Base* clone() const & {
           return new Quote(*this);
       }
       virtual Base* clone() && {
           return new Quote(std::move(*this))
       } 
       ```

       

       



