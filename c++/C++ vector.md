[TOC]

## C++ vector

### 概念

vector是一个封装了动态大小数组的顺序容器，可以认为是一个能够存放任意类型的动态数组

**顺序序列** ->可以根据下标访问

**动态数组** ->可以在序列末尾相对快速地添加、删除元素，不用手动扩容

**Allocator-aware** 能够感知内存分配器 -> 使用一个内存分配器对象来动态处理它地存储需求

### 函数实现

**include <vector>**

#### 构造

vector() ; //e.g.  vector<int> obj;

vector(int nSize); 元素个数nSize 没有给出初值的话值是不确定的

vector(int nSize, const T& t); nSize个值为t的元素

vector<int> a(b)拷贝构造

vector<int> a(b.begin(), b.begin()+3) 这里b是vector

vector<int> a(b,b+3) 这里b是数组

#### 插入

void push_back(const T& x); 尾部增加一个元素x

iterator insert(iterator it, const T& x) 迭代器指向元素前增加一个元素x

iterator insert(iterator it, int n, const T& x) 迭代器指向元素前增加n个元素x

#### 删除

void clear(); 清空

void pop_back(); 删除最后一个元素，注意，不能得到返回值

iterator erase(iterator it); 删除迭代器指向的元素

iterator erase(iterator first, iterator last); 删除[first, last)中的元素

#### 遍历

reference at(int pos) 返回pos位置元素的引用

reference front() reference back() 返回首尾元素的引用

iterator begin() 返回头指针，指向第一个元素

iterator end() 尾指针，指向最后一个元素的下一个位置

reverse_iterator rbegin() 反向迭代器，指向最后一个元素

reverse_iterator rend() 反向迭代器，指向第一个元素之前的位置

##### 三种访问方式

数组访问： vec[i]

函数访问：vec.at(i)

迭代器访问： 

vector<int>:: iterator it; 迭代器是一个指向元素的指针 

for(it=vec.begin();it!=vec.end();it++) { cout<<*it; }

#### 判断是否为空

bool empty() const 

#### 容器大小

int size() const; 当前含有的元素

int capacity() const; 当前最大容量

int max_size() const 这个vector最大可以是多大

resize(int n) 将现有元素调为n个，多则删，少则补，其值随机

resize(int n, const T& x) 将现有元素调为n个，多则删，少则补，其值为x

reserve(int n) 将a的capacity扩容到n个，用于即将给a添加大量数据的时候，避免多次自动扩容带来的性能降低

##### 定义二维数组的三种方法

1.int n=5, m=6;

vector< vector<int> > obj(n)  外层<>要有空格否则在旧编译器中无法通过

for(int i=0;i<obj.size();i++){ obj[i].resize(m); }

2.vecor< vector<int> > obj(n, vector<int>(m))

3.int out[3][2 ={ 1,2,3,4,5,6 };

vector<int*> obj; obj.push_back(out[0])

#### 算法include< algorithm >

##### 排序

sort(vec.begin(), vec.end()) 从小到大排序

重写sort的compare函数来实现降序：

bool compare(int a, int b) { return a>b; } a>b为降序 a<b为升序

sort(vec.begin(), vec.end(), compare)

##### 查找

find(a.begin(), a.end(), 10)

在a的begin到end(不包括)的元素中查找10，如果存在则返回一个迭代器iter指向其在vector中的位置，如果查找失败返回end迭代器 该元素的值为*iter 如果iter==a.end()则说明没找到

p.s. find也可以用于数组查找 返回int*

find_end(a.begin(), a.end(), b.begin(), b.end(), func)

查找b序列在a的范围内的最后一次出现，返回一个正向迭代器指向查找到的a的子序列的第一个元素，查找失败返回a.end() b也可以用int*等指针代替

func是一个附加条件，可选可不选

find_if(a.begin(), a.end(), func()) 容器元素类型为自定义类的时候不能用find，只能用find_if  遍历之后返回第一个使函数func为true的元素的迭代器，如果查找失败返回end迭代器

find_if_not(a.begin(), a.end(), func())

查找第一个不符合func的元素，和find_if一样适用于所有容器，包括序列式、关联式。返回输入迭代器。

###### p.s.STL分类

序列式：array vector heap list queue deque 默认未排序

关联式：set map hashtable multiset RB-tree 元素为键值对，默认按键值大小升序排列

###### 迭代器

（*迭代器名）就表示它指向的元素

正向迭代器++指向它后一个元素，反向迭代器++指向前一个 

##### 拷贝

copy(a.begin(), a.end(), b.begin()+1) 

把a从begin到end(不包括)的元素复制到b中，从b.begin()+1(包括)的位置开始

##### 逆序

reverse(vec.begin(), vec.end())  从begin()到end()(不包括)的元素倒置 如1 3 2 4 -> 4 2 3 1 

注意这里不包括第二个指针指向的位置，但end本来就指向最后一个元素的下一个位置

#### 其他

1.void swap(vector&) 交换数据 a.swap(b)

2.void assign(int n, const T& x) 设置前n个元素的值为x

3.注意，如果要保存很多元素，vector长度过大容易导致内存泄漏，而且效率会很低

4.vector作为函数参数或返回值时，&一定不能少

e.g. double Distance(vector<int>&a, vector<int>&b)

5.vector<int> a; for(int i=0;i<10;i++) a[i]=i

这种方法是错误的，下标只能用于获取已存在的元素，而现在的a[i]还是未初始化的空对象