c++ 11 随机值获取 random

c中的随机函数只有rand() 

生成均匀分布的伪随机数，每个数的范围在0和一个系统相关的最大值（至少32767）之间

c++的random库中的组件分为两类：随机数引擎类和随机数分布类

随机数引擎类是可以独立运行的随机数发生器，无法指定随机数的范围和概率，

随机数分布类是在随机数引擎的支持上，根据用户需求，生成符合条件的随机数，如某一区间，某一概率分布的随机数



所有随机数引擎类都支持的操作

| 名称            | 功能                    |
| --------------- | ----------------------- |
| Engine e        | 创建一个引擎            |
| Engine e(s)     | 创建一个以s为种子的引擎 |
| e.seed(s)       | 使用种子s充值e的状态    |
| e.min() e.max() | e能生成的最值           |
| e.discard(u)    | 将e推进u步              |

所有随机数分布类共有的操作

| 名称            | 功能                                     |
| --------------- | ---------------------------------------- |
| U u             | 创建一个分布类u                          |
| u(e)            | 用随机数引擎e生成随机数                  |
| u.min() u.max() | u能生成的最值                            |
| u.reset()       | 重置u的状态使随后u生成的值不受之前的影响 |

随机非负数 default_random_engine

返回一个随机的unsigned类型值

default_random_engine e;

for(int i=0;i<10;i++){ cout<<e(); }

伪随机数 如果再运行一次程序得到的还是这几个数

如果想每次运行结果不同，应为其设置较为随机的种子，如当前系统的时间



指定范围的非负数 uniform_int_distribution

模板类，模板参数为生成随机数的类型（需要是整数），构造函数接收一个分布范围（闭区间）

default_random_engine e;

uniform_int_distribution<unsigned> u(0, 9);

for(int i=0;i<10;i++){ cout<<u(e); }



随机浮点数 uniform_real_distribution

模板类，参数是浮点类型(float, double, long double) 构造函数接收一个分布范围（闭区间）

default_random_engine e;

uniform_real_distribution<double> u(0.0, 1.0);



随机布尔值 bernoulli_distribution

分布类 非模板 构造函数只有一个参数，表示该类返回true的概率，默认0.5

default_random_engine e;

bernoulli_distribution u;

cout<<u(e);