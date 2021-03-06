## leetcode

### hard

#### 295 数据流的中位数

2021.9.2

中位数是有序列表中间的数或中间两个数的平均值，设计一个数据结构MedianFinder，包括构造函数，addNum(int num)添加一个整数，findMedian()返回当前所有元素的中位数

进阶：如果所有数都在0-100内如何优化算法，如果有99%的数在0-100内如何优化算法

### medium

#### 165 比较版本号

2021.9.1

给定两个字符串版本号，如2.5.33 1.0.1，修订号之间由.连接，每个修订好由多位数字组成，包含前导零。按从左到右的顺序比较，忽略前导零，如1.0==1.0.0

##### 暴力解法/字符串分割

可以用isdigit()判断是数字还是.，但是这题只有这一种符号所以=='.'也能判断

字符转int用的是stoi()，这样就可以完全忽略前导零的影响

c_str()把c++字符串转化为c字符串从而使用c的一些函数，调用方法为str.c_str()

emplace_back() c++提出的对STL容器的插入方法，有emplace和emplace_back，比insert和push_back更高效

原理：

push_back先向容器尾部添加一个右值元素（临时对象），然后调用构造函数构造出这个临时对象，最后用移动构造函数将其放入容器，然后释放这个临时对象

emplace_back在容器尾部添加一个元素，调用构造函数原地构造，不需要触发拷贝和移动，所以更高效

用stringstream实现字符串分割，版本号1为string version1，初始化的方法有两种：stringstream input1(version1); 和stringstream input1;  input1<<version1;

再初始化一个空string temp

while(getline(input1, temp, '.')){ vec1.push_back(temp); }

为了节省空间，1处理完之后可以input1.clear() 然后input1<<version2接着做

因为两个vector很可能size不一样，为了方便比较，先末尾补0

while(v1.size()<v2.size()){ v1.push_back(0)} //这样比较简练，避免作差

p.s. java对应的函数是String[] s1=v1.split("\\.") 记得转义 a=Integer.parseInt(s1[i])

##### 双指针

为了优化空间复杂度，不存储分割后的修订号，直接在分割的同时解析并比较

双指针并不一定是指针，这里是广义的代指和表达数据的变量

字符串-'0'也表示转换成正数

有两个遍历的量的时候可以选择while循环

while( i<version1.length || j<version2.length )

里面再用for求单个修订号 for(; i<version1.length&&version1[i]!='.';++i) { x=x*10+version[i]-'0'}

然后++i跳过点号，现在x就是一个修订号了

因为修订号不一定是个位数，所以要写循环

至于时间复杂度，两个方法都是O(n+m)，也就是O(max(m,n))

#### 528 按权重随机选择

2021.8.30

给定一个正整数数组w，其中w[i]表示i的权重，写一个随机获取下标，选取下标i的概率与w[i]成正比的函数

给的是Solution类，没填的构造方法Solution(vector< int>& w)，还有要填的函数，需要自己补一些private成员变量，类初始化的方法为Solution* obj=new Solution(w) 调用为int param=obj->pickIndex()

##### 前缀和+二分查找

数组w的权重之和为total，将[1,total]范围内所有的整数分成n(w.size())个部分，第i个部分正好包含w[i]个整数

在[1,total]范围内随机取一个整数x，x在第i个部分内，故返回i

每个区间的左边界是它之前出现的元素权重和再加1，右边界是到它为止的权重和，前缀和数组pre，pre[0]=w[0] pre[i]=pre[i-1]+w[i] 则第i个区间左边界为pre[i]-w[i]+1 右边界就是pre[i] 注意这里左边界也用pre[i]而不是pre[i-1]是为了后面列式子查找方便

取到随机数x之后，找满足pre[i]-w[i]+1<=x<=pre[i]的i

因为pre是单调递增，所以使用二分查找，在最理想的O(logn)时间复杂度下找到i

![image-20210831150838740](C:/Users/rickyrqzhao/AppData/Roaming/Typora/typora-user-images/image-20210831150838740.png)

mt19937是伪随机数产生器，使用须include< random>

uniform_int_distribution< int> ，头文件< random>， 随机数分布类，<>参数里是生成随机数的类型（int只支持整数），还可以指定区间

random_device  头文件< random> 非确定性随机数生成器，生成均匀分布的无符号数，可能会依靠非确定源（如硬件设备）生成真随机数，在环境受限时生成伪随机数

accumulate 头文件< numeric> 求特定范围内所有元素的和

partial_sum(first, last, result ) 头文件< numeric> 对从first到last的元素逐个累积求和，放入result容器内

back_inserter 头文件< iterator> 用于在末尾插入元素

lower_bound 头文件< algorithm> 用于找出范围内不大于num的第一个元素

##### 轮询

构造一个最小轮询序列，权重精度保留到小数点后一位，用(i, cnt)的形式进行存储，代表下标i在最小轮询

TODO

#### 1109 航班预定统计 

2021.8.31

从1到n编号的n个航班，航班预定表vector< vector < int>> bookings，第i条记录bookings[i]=[firsti, lasti, seatsi]，意为从firsti到lasti（闭区间）的每个航班预定了seatsi个座位

返回一个长度为n的数组，第i项为航班i预定的座位总数

![image-20210831112448197](C:\Users\rickyrqzhao\AppData\Roaming\Typora\typora-user-images\image-20210831112448197.png)

##### 暴力解法

注意初始化一个长度为n的vector可以使用vector< int> ans(n,0)

注意题上说的是从first到last而不是只有这两个

注意航班编号从1开始

二维vector的行数是vec.size() 列数是vec[0].size()

在这道题中暴力解法会超时

##### 差分

差分数组的第i个数表示原数组第i-1个元素与第 i个元素的差值，对应的概念是前缀和数组，也就是nums[i]+=nums[i-1] 这两个数组的0项都和原数组一样

对差分数组求前缀和就可以得到原数组

差分数组的性质是，当我们希望对原数组的区间[first, last]增加一个增量inc时，在差分数组nums中对应的体现是，nums[first]+=inc nums[last+1]-=inc，这样对区间的修改就变成了对两个位置的修改

所以，遍历给出的预定记录数组，每次O(1)地完成对差分数组地修改，最后在求点缀和得到答案

注意，当last为最后一个元素时，无需修改nums[last+1]，会导致溢出，就算没有语法报错，在对这个位置求前缀和的时候值一定是0

因为，假设原数组最后一个元素a[n]为x，则差分数组b[n+1]为-x，再求前缀和，求到a[n+1]=a[n]+b[n+1]=x+(-x)=0

还有，要求的返回值是数组，而返回值不计入空间复杂度，所以空间复杂度为O(1)

auto关键字

C++11引入的auto关键字是一个类型说明符，通过变量的初始值或表达式中参与运算的数据类型来自己推断变量的类型，所以auto变量定义时必须初始化，如auto x=5.2f auto a=b+c

也支持在一条语句中声明多个变量，但这些变量的类型需要时一样的，如auto i=0, *p=&i ；这里i是整数，p是整形指针

它并不是一个独立的类型，所以不能使用类型转换和sizeof typeid等操作

函数和模板参数不能被声明为auto

##### 线段树

TODO

##### 区间求和问题

数组不变，区间查询：前缀和，树状数组，线段树

数组单点修改，区间查询：树状数组，线段树

数组区间修改，单点查询：差分，线段树

数组区间修改，区间查询：线段树

1109涉及的是区间修改和单点查询（求结果数组即为对每个下标做单点查询），所以是差分的模板题

#### M17.14 最小k个数

2021.9.3

以任意顺序返回给出的数组中最小的k个数

##### sort函数排序

sort(arr.begin(), arr.end()) 对[first,last)内的元素做默认的升序排序，参数是随机访问迭代器，还可以定义一个具有两个参数并返回bool的函数作为排序规则，写作第三个参数

记得include< algorithm> 

排序的时间复杂度为O(nlogn) 空间复杂度为O(logn)

##### 大根堆

用一个大根堆实时维护数组的前k小的值：首先将前k个数插入大根堆，然后从第k+1个数开始遍历，如果比大根堆堆顶的数要小，就把堆顶弹出换成当前的，最后把堆里所有数存起来返回就行

c++中的堆（即优先队列）priority_queue为大根堆，而python中的堆是小根堆，解决方法是对数组中所有数取相反数

注意先处理k=0的情况，直接返回空就行了

![image-20210903171511976](C:/Users/rickyrqzhao/AppData/Roaming/Typora/typora-user-images/image-20210903171511976.png)

时间复杂度O(nlogk) 因为大根堆是是维护前k小的值所以插入删除都是O(logk)的复杂度，最坏情况下数组里n个数都需要插入（也就是一开始数组是个倒序），所以是nlogk

空间复杂度O(k)，因为大根堆里最多k个数

##### 快排/递归

快速排序的划分函数每次执行完之后都能将数组分成两个部分，小于等于分界值pivot的元素放在数组左边，大于的放右边，然后返回分界值所在的下标。不同的是，快排根据分界值下标递归处理两侧，而此题只需要处理一边

TODO

### easy

#### J22 链表中倒数第k个节点

2021.9.2

给定一个链表1->2->3->4->5，k=2，则返回链表4->5

注意4->5其实就是第4个节点ListNode*

##### 暴力解法/顺序查找

假设当前链表长度为n，则倒数第k个节点是正数第n-k个节点（想不明白的话写个例子数一下，注意计数和下标的区别，for循环条件的判断）

c++的空指针使用nullptr

##### 双指针

快慢指针，第一个fast指向链表的第k+1个节点，第二个slow指向头节点，此时二者之间刚好间隔k个节点，然后同步向后走，当fast走到尾部的下一个空节点时，slow刚好指向倒数第k个

注意，非空尾节点是倒数第一个，所以fast要走到空节点，也就是

while(fast) { fast=fast->next;  slow=slow->next; }

虽然时空复杂度都没变但是听起来高级一些

##### 栈/队列

比较直观但需要额外的空间复杂度，就当多个思路了

将所有节点压入栈/队列中，然后从栈顶/队列头弹出k个元素，第k个就是答案

#### 1588 所有奇数长度子数组的和

2021.9.2

给定一个正整数数组计算所有可能的连续长度子数组的和，这里的子数组必须是连续子序列

注意，题目要求子数组连续，而且求和是每个数组求和再求和->由此可以想到计算每个数字被加了几倍

##### 暴力解法

甚至压根没有往这个方向去想，但逻辑是很顺的

遍历数组中每个长度为奇数的子数组，维护一个变量存储求和。令数组arr的长度为n，子数组的开始下标为start，长度为length，结束下标为end，则需要满足的条件是0<=start<end<=n, length=end-start+1为奇数

具体实现是，start从0到n一层for循环，里面有length从1到n-start每次加2的for循环，根据定好的start和length求出end，然后从start到end第三层for循环求个和

总计时间复杂度是O(n3)

##### 前缀和

对于暴力的三层for循环，只要想办法优化一层，就能把复杂度降下来

求子数组的和可以采用前缀和，建立前缀和数组需要O(n)的时间，建立好了之后只需要O(1)就能知道单个子数组的和。前缀和数组sums长度为n+1,sums[0]=0, sums[i]表示从下标0到i-1的元素和，[start, end]子数组的元素和即为sums[end+1]-sums[start]

因为c++不能直接建立大小为n+1的数组，所以可以使用vector< int>反正下标访问操作没有影响

这样复杂度就是O(n2)

##### 数学方法

对于下标为i的元素，它左边有i个数字，右边有arr.size()-i-1个数字，左右分别考虑，计算包含这个元素的、长度为奇数的数组有多少个，就给总和加上这个数字的多少倍

情况一：左边奇数右边奇数，加上它这个1，刚好是奇数

int oddL=(left+1)>>1 左边可以选多少种奇数 >>1也就是/2，即右移1位

同理oddR=(right+1)>>1

这样总情况就是oddL*oddR

情况二：左边偶数右边偶数 （注意0也是偶数，满足题意的）

evenL=(left>>1)+1 evenL*evenR

最后是ans+=arr[i] * [(oddL *oddR)+(evenL *evenR)]

这四个式子的思路是，假设是1 2 3 4 5 6 7 x，从x左边取单数，一定有7，然后就是567，34567，1234567，可以理解为这可取的4个数组分别是7，56，34，12，所以是总数加一个数之后/2

而如果取偶数，67，45，23，就是总数/2，多的舍掉，再加一个0

#### 1480 一维数组的动态和

2021.9.2

数组nums，求其动态和（其实就是前缀和）数组runningSum[i]=sum(nums[0]...nums[i])

究极无敌模板题，居然都能写出报错，你也真是丢人

报错一：定义一个vector< int> result; 然后直接写result[0]=nums[0] 但此时result[0]是未初始化的空对象，下标只能用于获取已存在的元素，所以报错，参见c++ vector-其他-5

报错二：写个for循环，直接result[i]=result[i-1]+nums[i]，错误原因同上，所以在这之前先处理一边，全push_back(0)，反正前缀和数组大小已知，和原数组一样

注意，简单题比较究细节，所以最好的解法是原地修改，nums[i]+=nums[i-1]，刚好也省了新定义vector所带来的如上问题

#### 704 二分查找

2021.9.6

给定一个升序数组和一个目标值，搜索目标值在数组中的下标，没有返回-1

经典的二分

模板：int left=0,right=nums.size()-1;

while(left<=right){

int middle=(left+right)/2; if(nums[middle]<target){ left=middle+1} else if..{ right=middle-1} else..return

}return -1;

注意while循环的判断条件是小于等于

注意循环内的改变条件是left=middle+1和right=middle-1

第一次求middle的时候可以采用int middle=left+(right-left)>>>1 这样可以防止极其微小的可能性和极其刁钻的条件下left+right整型溢出

位操作右移>>1比/2更好，这里的>>>是无符号右移

#### 1221 分割平衡字符串

2021.9.7

L和R字符相同的字符串平衡，给定一个平衡字符串返回她最多能分隔出多少平衡字符串

可以从数学上证明每一次都从最小分割点进行分割（也就是分得尽可能短）就能得到最优解

##### 遍历计数/贪心

if(s[i]=='L') balance++ else balance-- if(balance==0) ans++

注意字符串长度是s.length() 取单个字符可以用s[i]也可以用s.charAt(i)

常见的遍历信息统计可以选择记录前缀信息的数据结构如栈，但对于成对的元素，更好的方式是转为-1，1，0的数学判定

