---
title: Welcome to my blog
---

Hello. This is a learning process for how to use github page.

## STL

> 参考
>
> - MOOC郭炜C++算法入门
> - https://wyqz.top/p/870124582.html#toc-heading-2

STL提供了六大组件，彼此之间可以组合套用，这六大组件分别是：容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器。

- 容器：各种数据结构，如vector、list、queue、set、map等，用来存放数据，从实现角度看，STL容器是一种class template。

- 算法：各种常用的算法，如sort、find、copy、for_each。从实现角度来看，STL算法是一种 function template。
- 迭代器：扮演了容器与算法之间的胶合剂，共有五种类型，从实现角度来看，迭代器是一种将operator* , operator-> , operator++,operator–等指针相关操作予以重载的class template. 所有STL容器都附带有自己专属的迭代器，只有容器的设计者才知道如何遍历自己的元素。原生指针(native pointer)也是一种迭代器。
- 仿函数：行为类似函数，可作为算法的某种策略。从实现角度来看，仿函数是一种重载了operator()的class 或者class template。
- 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西。
- 空间配置器：负责空间的配置与管理。从实现角度看，配置器是一个实现了动态空间配置、空间管理、空间释放的class tempalte。

STL六大组件的交互关系：

​	容器通过空间配置器取得数据存储空间，

​	算法通过迭代器存储容器中的内容，

​	仿函数可以协助算法完成不同的策略的变化，

​	适配器可以修饰仿函数。



**常用容器概述**

vector, list, queue, dqueue, priority_queue, set, multiset, unordered_set, map, multimap, unordered_map等

### Vector

#### vector常用API操作

**vector构造函数**

```C++
vector<T> v;	// 采用模板实现类实现，默认构造函数
vector<T> (v.begin(), v.end());		// 将[v.begin(), v.end())区间中的元素拷贝给本身。
vector<T> (n, elem);				// 构造函数将n个elem拷贝给本身
vector<T> (const vector &vec);		// 拷贝构造函数
```

**vector常用赋值函数**

```C++
assign(beg, end);		// 将[begin, end)区间内的数据拷贝给本身
assign(n, elem);		// 将n个elem赋值给本身
vector& operator=(const vector &vec);	// 重载等号操作符
swap(vec);				// 将vec与本身的元素互换。
```

**vector大小操作**

```C++
size();					// 返回容器内元素的个数
empty();				// 判断容器是否为空
resize(int num);		// 重新指定容器长度为num, 变长填充默认值， 变小删除旧元素
resize(int num, elem);	// 重新指定容器长度为num, 变长填充elem， 变小删除旧元素
capacity();				// 容器的容量
reserve(int len);		// 容器预留len个元素长度，预留位置不初始化，元素不可访问
```





### 二分查找

#### binary_search

`bool binary_search(数组名+n1, 数组名+n2, 值);`

- 查找下标范围为[n1, n2)的元素, **下标为n2的元素不在查找区间内** . 在该区间内查找"等于"值”的元素，返回值为true(找到）或false(没找到）

自定义比较函数的重载形式

`bool binary_search(数组名+n1, 数组名+n2, 值, 重载函数);`

```C++
// 比较函数实例
struct Rule
{
	bool operator() (const int &a1, const int & a2) const {
        return a1%10 < a2%10;
    }
};

// 运用实例
void test()
{
    int a[] = { 12,45,3,98,21,7};
    sort(a,a+6,Rule()); //按个位数从小到大排
    cout <<"result:"<< binary_search(a,a+6,7) << endl;
    cout <<"result:"<< binary_search(a,a+6,8,Rule()) << endl;
}
```





#### lower_bound二分查找下界

> 在对元素类型为T的从小到大排好序的基本类型的数组中进行查找

`T * lower_bound(数组名+n1,数组名+n2,值);`

- 返回一个指针 T * p;

- *p是查找区间里下标最小的, **大于等于** `值`的元素. 如果找不到, p = 数组名+n2. 

  [更友好的说明]：插入`值`，而不影响有序性的最小位置（下界）

- 同样有自定义比较函数的重载.



#### upper_bound二分查找上界

```c++
T * upper_bound(数组名+n1, 数组名+n2, 值, 排序规则结构名());
```

- *p 是查找区间里下标最小的，**大于**`值`的元素。如果找不到，p指向下标为n2的元素

  [更友好的说明]：插入`值`，而不影响有序性的最大位置（上界）







### 二叉平衡树

#### multiset

- 声明方式：

  ```c++
  multiset<T> st;
  ```

- 排序规则：若 a < b，则a排在b前面

- 使用`st.insert`插入元素，`st.find`查找元素， `st.erase`删除元素，复杂度都是`log(n)`

- 使用multiset和set，需要头文件 `<set>`

  



**multiset上的迭代器**

- 声明方式：

  ```
  multiset<T>::iterator p;
  ```

- **基本用法：**p 是迭代器，相当于指针，可用于指向multiset中的元素。访问multiset中的元素要通 过迭代器。

- 与指针的区别：multiset上的迭代器可 ++ ，--， 用 != 和 == 比较，**不可比大小，不可加减整数，不可相减**

- `st.begin()` 返回值类型为 multiset::iterator,  是指向st中的头一个元素的迭代器

- `st.end()` 返回值类型为 multiset::iterator,  是指向st中的最后一个元素后面的迭代器

- 对迭代器 ++ ,其就指向容器中下一个元素，-- 则令其指向上一个元素



**自定义排序规则的multiset的用法**

- 自定义规则示例：

  ```c++
  struct Rule1 {
      bool operator()( const int & a,const int & b) const { 
          return (a%10) < (b%10); 
      }//返回值为true则说明a必须排在b前面
  };
  ```

- 用法示例：

  ```c++
  void multiset_modify_sort_test(){
  	multiset<int, Rule1> st;	// 自定义排序规则
      
      int a[10]={1,14,12,13,7,13,21,19,8,8 };
      for(int i = 0; i < 10; i++)
          st.insert(a[i]);
      multiset<int,greater<int> >::iterator i; 
      for(i = st.begin(); i != st.end(); ++i) 
          cout << * i << ",";
      cout << endl; 
  }
  ```

  





#### set

- 简要说明：api和`multiset`基本一致

**不同之处：**

- `set`里不能有重复元素，`multiset`可以有

- `set`插入元素可能不成功

  ```c++
  // 检查插入是否成功的示例
  // 核心代码
  set<int> st;
  /*
  ...
  */
  pair<set<int>::iterator, bool> result = st.insert(2);
  if(result.second == false) // 条件成立说明插入不成功
  	cout << *result.first << "already exists." << endl;
  else
      cout << *result.first << "inserted." << endl;
  ```

  **完整示例**

  ```c++
  #include <iostream>
  #include <cstring>
  #include <set>
  using namespace std;
  int main()
  {
      set<int> st;
      int a[10] ={ 1,2,3,8,7,7,5,6,8,12 };
      for(int i = 0;i < 10; ++i)
          st.insert(a[i]);
      cout << st.size() << endl; //输出：8
      set<int>::iterator i;
      for(i = st.begin(); i != st.end(); ++i)
              cout << * i << //输出：1,2,3,5,6,7,8,12,
      cout << endl;
      pair<set<int>::iterator, bool> result = st.insert(2);
      if( ! result.second ) //条件成立说明插入不成功
          cout << * result.first <<" already exists." << endl;
      else
          cout << * result.first << " inserted." << endl;
      return 0;
  } 
  ```

- **Ex. pair模板说明**

  ```c++
  pair<T1,T2>类型等价于：
  struct {
      T1 first;
      T2 second;
  };
  例如：pair<int, double> a; 
  等价于：
  struct {
      int first;
      double second;
  } a;
  a.first = 1;
  a.second = 93.93; 
  ```

  

#### multimap

- 声明方式：

  ```c++
  #include <map>
  multimap<T1, T2> mp;
  ```

- multimap里的元素都是pair形式的，即

  ```c++
  struct {
  	T1 first;		// 关键字
      T2 second;		// 值
  };
  ```

- multimap中的元素按照`first`排序，并可以按`first`进行查找 

- 缺省的排序规则是 `a.first < b.first` 为true,则a排在b前面

- 插入 (mp.insert) 时使用`make_pair(T1变量,T2变量)`更加方便







#### map

**和multimap区别在于：**

- 不能有关键字重复的元素
- 可以使用 [] ，下标为关键字，返回值为first和关键字相同的元素的second。而multimap要使用迭代器

- 插入元素可能失败

- 不存在的键使用[]，返回0
