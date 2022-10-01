
# 计数排序：Counting Sort

时间复杂度：$O(n)$

空间复杂度：$O(maxValue - minValue)$

适用于数据范围比较小。



## 简单计数排序

如果只是普通的值、直接排序即可。代码如下

```cpp
vector<int> countingSort(const vector<int>& arr) {
    int max = *max_element(arr.begin(), arr.end());
    int min = *min_element(arr.begin(), arr.end());
    int n = max - min + 1;
    vector<int> count(n, 0);
    for (const auto& e: arr) {
        ++count[e - min];
    }
    vector<int> res; res.reserve(arr.size());
    for (int i = 0; i < n; ++i) {
        int val = i + min;
        int cnt = count[i];
        for (int j = 0; j < cnt; ++j) {
            res.emplace_back(val);
        }
    }
    return res;
}
```



## 计数排序（稳定）：实际应用

考虑这样一种情况，我们有一个app，有用户类，需要对用户地年龄进行排序。上述代码还可行吗？

对于用户，不仅含有一个值、还有其他很多信息。

拿年龄排序举例，尽管每个人的年龄相同，但他们不是同一个人。就不能简简单单地直接进行对值进行计数，然后赋值。

那计数排序行不通了吗？答案是可以的。先上代码。

```cpp
#include <bits/stdc++.h>

using namespace std;

class User{
public:
    int age;
    string name;
    User(string name, int age) : name(name), age(age) {}
};
using UserPtr = shared_ptr<User>;

auto comp1 = [] (const auto& prev, const auto& curr) {
    return prev->age < curr->age;
};
auto comp2 = [] (const auto& prev, const auto& curr) {
    return prev->age > curr->age;
};

vector<UserPtr> countingSort(const vector<UserPtr>& arr) {
    int maxAge = (*max_element(arr.begin(), arr.end(), comp1))->age;
    int minAge = (*max_element(arr.begin(), arr.end(), comp2))->age;
    int n = maxAge - minAge + 1;
    vector<int> count(n, 0);
    for (const auto& ptr: arr) {
        ++count[ptr->age - minAge];
    }
    partial_sum(count.begin(), count.end(), count.begin());
    vector<UserPtr> res(arr.size());
    for (int i = arr.size() - 1; i >= 0; --i) {
        int index = count[arr[i]->age - minAge] - 1;
        res[i] = arr[index];
        --count[arr[i]->age - minAge];
    }
    return res;
}


int main() {
    UserPtr user1 (new User("zhangsan", 1));
    UserPtr user2 (new User("lisi", 2));
    UserPtr user3 (new User("liu5", 1));
    vector<UserPtr> arr{user1, user2, user3};
    arr = countingSort(arr);
    for (const auto& ptr: arr) {
        printf("%s %d\n", ptr->name.c_str(), ptr->age);
    }

}
```

我们使用用户指针来排序，避免用户数据的冗余构造和析构。



## 核心代码

精简下来，精简下无关代码如下。

```cpp
vector<int> countingSort(vector<int> nums) {
    int max = *max_element(nums.begin(), nums.end());
    int min = *min_element(nums.begin(), nums.end());
    int n = max - min + 1;
    vector<int> count(n, 0);
    for (const auto& e: nums) {
        ++count[e - min];
    }
    partial_sum(count.begin(), count.end(), count.begin()); 
    //count: 前缀和，表示该值(包括该值)对应的坐标前面有多少个元素.
    vector<int> res(nums.size());
    for (int i = nums.size() - 1; i >= 0; --i) {
        int index = count[nums[i] - min] - 1;
        res[index] = nums[i];
        --count[nums[i] - min];
    }
    return res;
}
```

步骤如下

1. 计算出容量大小`n = max - min + 1`
2. 对于每个坐标进行次数统计
3. 前缀和运算.
4. 从后向前(从后向前主要是为了**稳定性**)进行放置





