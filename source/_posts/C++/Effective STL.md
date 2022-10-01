# Effective STL

## Containers

**Item 1: Choose Your containers with care**



**Item 2: Beware the illusion of container-independent code**

use `typedef` to facilitate changes of container's kind by encapsulating



**Item 3: Make copying cheap and correct for objects  in containers**

create containers of *pointers* instead of containers of objects.

use smart pointers to avoid resource leaking.



**Item 4: Call `empty()` instead of checking `size()` against zero.**

It's always constant-time operation about `empty()` for all containers.

But `size()` is linear time operation for some container, such as list.



**Item 5: Prefer range member  to their single-element counterparts.**

If you want to do a operation a range of elements, using range member function to accelerate.



**Item 6: Be alert for C++'s most vexing parse.** 

It's confusing to recognize.

```cpp
ifstream dataFile("ints.dat");
list<int> data(istream_iterator<int>(dataFile), istream_iterator<int>()); //this doesn't do what you think it does.

//three kinds of same declaration are as follow.
int f(double d);
int f(double (d));
int f(double);

//g receive a function pointer.
int g(double (*pf)());
int g(double pf());
int g(double ());
```

for `list<int> data(para1, para2)`  idiom as above.

* `para1`'s type is `istream_iterator<int>`
* `para2`'s type is function pointer.

```cpp
list<int> data((istream_iterator<int>(dataFile)), istream_iterator<int>()); //proper way to involve range constructor.

//solution2
ifstream dataFile("ints.dat");
istream_iterator<int> dataBegin(dataFile);
istream_iterator<int> dataEnd;
list<int> data(dataBegin, dataEnd);
```



**Item 7: When using containers of newed pointers, remember to `delete`the pointers before the contains is destroyed. **

Containers have responsibility for the element they contain, it's for pointer when contains pointer, but not for the object pointers point to.



**Item 8: Never create containers of `auto_ptr`s.**

 *copying* leads to `nullptr`.



**Item 9: Choosing carefully among erasing options.**

using different erasing way for different contains.

* For sequence container, use erase-remove idiom.
* For `list`, use `remove`&`remove_if`.
* For associative container, use its erase member function and update the iterator before erasing.



**Item 10: Be aware of allocator conventions and restrictions.**

wait updating.



**Item 11: Understand the legitimate uses of custom allocators.**

wait updating



**Item 12: Have realistic expectations about the thread safety of STL contains.**

use mutex lock to make thread safe.

use local object to control lock.



## vector and string

**Item 13: Prefer `vector` and `string` to dynamically allocated arrays.**

Advantages as follow.

* Resource manage.
* proper delete behavour.
* growing capacity.



**Item 14: Use reserve to avoid unnecessary reallocations.**



**Item 15: Be aware of variations in `string` implementations.**

* Based on referenced-counting or not.
* support per-object allocators or not.
* require zero, one or two dynamic allocations ?
* varing memory layout.



**Item 16: Know how to pass `vector` and `string` data to legacy APIs.**

use `string::c_str()`&`&arr[0]` to pass.



**Item 17: Use "the swap trick" to trim excess capacity.**

```cpp
string s("11....1");
//use s
s.resize(s.size() / 2); //minimize s's size, not capacity.
string(s).swap(s);//trick to implement "shrink-to-fit"
s.shrink_to_fit(); //library function.
```



**Item 18: Avoid using `vector<bool>`**

It doesn't hold bools. 

It's not an STL container, implemented by Proxy technique.



## Associative Containers

**Item 19: Understand the difference between equality and equivalence.**

equality are defined by `bool operator==();`.

equivalence depends on  compare function, like typically `bool oeprator<();`  This example,  equivalence only when `!(o1 < o2) && !(o2 < o1)`.

`bool operator<()`return true when o1 should precede the o2 in order.

 

**Item 20: Specify comparison types for associative container of pointers.**

custom comparison if necessary, like container of pointer.



**Item 21: Always have comparison functions return false for equal values.**

As it mentioned in Item19, `bool operator<()`return true when o1 should precede the o2 in order.

when o1 equal o2(namely `o1 == o2`), o1 shouldn't precede o2 and o2 shouldn't precede o2.

Also, in associative container, if comparison function return true for equal value, it's not way that both can be in the range identified by `equal_range`.

Because they are not equivalent.

**set **

```cpp
set<int, less_equal<int>> set{10, 10};
cout << set.size() << endl; //size = 2.
```

**multiset**

```cpp
multiset<int, less_equal<int>> mset{10, 10};
auto it = mset.lower_bound(10);
cout << (it == mset.end()) << endl; // 1 -> true
```



**Item 22: Avoid in-place key modification in set and multiset.**

set contains \<K>, while map contains pair<const K, V>;

Don't change the key part that order depends on, using erase and insert new technique to replace modification.



**Item 23： Consider replacing associative container with sorted vectors.**

Consider relevant consumption.

* Setup.
* Lookup.
* Reorganize.

vectors commonly use less memory than associative container.



**Item 24: Choose carefully between `map::operator[]` and `map::insert`when efficiency is important.**

```cpp
map<obj, obj> map; 
map[1] = 1; 
//1. map[1] means creating default object by its default construtor. 
//2. right hand side is created by relevant constructor.
//3. replace the defualt one by "=" copy assignment.
```

while `insert` skip the default object(constructor and destructor.)



**Item 25: Familiarize yourself with the nonstandard hashed containers. ** 

While there are standard hashed container, `unordered_set, unordered_map, unorderd_multiset, unordered_multimap.`



## Iterators

**Item 26：Prefer `iterator` to `const_iterator`,  `reverse_iterator`, and `const_reverse_iterator`.**

**deprecated**: Iterator is more acceptable in many STL function.



**Item 27: Use distance and advance to convert a container's `const_iterators` to `iterators`**.

For most containrs, `iterator` and `const_iterator` are not same type, except `vector` and  `string`(using *raw pointer* and `char*`).

They can't be implicitly convert.

```cpp
vector<int> arr{1, 2, 3, 4, 5};
vector<int>::iterator it = arr.begin();
vector<int>::const_iterator cit = arr.cbegin() + 3;
advance(it, distance<vector<int>::const_iterator>(it, cit));
cout << *it;
```





**Item 28: Understand how to use a `reverse_itertor`'s base iterator.**

Use `reverse_iterator::base()`to get  relevant `iterator`.



**Item 29: Consider `istreambuf_iterator`s for character-by-character input.**

To avoid some unnecessary check behavior consumption for character-by-character input.

```cpp
ifstream data("test.txt");
string s((istreambuf_iterator<char>(data)), istreambuf_iterator<char>());
cout << s;
```



## Algorithms

**Item 30: Make sure destination ranges are big enough.**

Be careful something like insert element to the end.



**Item 31: Know your sorting options.**

sort algorithm

* `sort`
* `partial_sort`
* `nth_element`
* `partition`
* `stable_sort`
* `stable_partition`



**Item 32: Follow remove-like algorithm by erase if your remove something.**

`remove` implemented by double pointer algorithm, remove the element remain in the front of contain, but don't change the size.

while erase eliminate the element.



**Item 33:  Be wary of remove-like algorithm on container of pointers.**

Before erasing the pointer, free the resource it point to.

Or, use smart pointer.



**Item 34: Note which algorithms expect sorted ranges.**

Relevant algorithm

* `binary_seach`
* `lower_bound`
* `upper_bound`
* `equal_range`
* `set_union`
* `set_intersection`
* `set_symmetric_difference`
* `set_difference`
* `includes`
* `unique`
* `unique_copy`



**Item 35：Implement simple case -intensive string comparisons via mismatch or lexicographical_compare. **

custom you own comparison function or function object.



**Item 36: Understand the proper implementation of copy_if.**

Waiting Updating.



**Item 37: Use accumulate or for_each to summarize ranges.**

custom your function object to keep more information and state.



## Functors, Functor Classes, Functions, etc.

**Item 38: Design functor classes for pass-by-value.**

for BPFC(*Big Polymorphic Functor Class*), use container of pointer to keep small and monomorphic(not polymorphic).



**Item 39: Make predicates pure functions.**

pure functions keeps

* return bool.
* only depends on its parameters.

Meanwhile, it's important for functor to declare const for its `operator() const`. to keep pure.



**Item 40: Make functor classes adaptable.**

`not1`, `not2`, `bind2nd`

Waiting Updating





**Item 41: Understand the reason for `ptr_fun`, `mem_fun`, and `mem_fun_ref`.**

Waiting Updating.

```cpp
class obj {
public:
    obj(char c):c(c) {}
    void say() {
        cout << c << ' ';
    }
private:
    char c;
};
```

For container of object, use 

```cpp
vector<obj> vec;
vec.emplace_back('c');
for_each(vec.begin(), vec.end(), std::mem_fun_ref(&obj::say));
```

For container of pointer, use

```cpp
vector<obj*> vec;
vec.emplace_back(new obj('c'));
for_each(vec.begin(), vec.end(), std::mem_fun(&obj::say));
```





**Item 42: Make sure less\<T> means operator<.**

less\<T> will invoke `operator<()`.



## Programing with STL

**Item 43：Prefer algorithm calls to hand-written loops.**



**Item 44: Prefer member functions to algorithms with the same name.**



**Item 45: Distinguish among count, find, binary_search, lower_bound, upper_bound, and equal_range.**



**Item 46：Consider function objects instead of functions as algorithm parameters.**



**Item 47: Avoid producing write-only code.**



**Item 48: Always #include the proper headers.**



**Item 49: Learn to decipher STL-related compiler diagnostics.**



**Item 50: Familiarize yourself with STL-related websites.**

* SGI STL
* STLport
* Boost



## Summary

Frankly speaking, most of items are useful.

Some of them, I need more practice to understand relevant usage.(which I marked waiting updating.)