# 算法笔记

**算法是完成任务的指令**

> log10^n = 100     ;   n=2    /也就是10的2次方等于100, 求指数, 对数

**应选择效率最高的算法，以最大限度地减少运行时间或占用空间。**

最多需要猜测的次数与列表长度相同，这被称为 **线性时间(linear time)** , O(n)

**算法运行时间是从其增速的角度度量的**

**算法运行时间用大O表示法表示**



- **链表的优势在插入元素方面**
- **数组的优势在于随机读取**
    - **元素的位置称为索引**





- [大O表示法](#大O表示法)
- [二分查找法](#二分查找法)
- [选择排序](#选择排序)
- [快速排序](#快速排序)
- [散列表](#散列表)
- [广度优先搜索](#广度优先搜索)
- [狄克斯特拉算法](#狄克斯特拉算法)
- 





## 大O表示法

**大O这指出了算法需要执行的操作数**

**大O表示法 让你能够比较操作数，它指出了算法运行时间的增速** 完成这个算法用了多少步操作

**大O表示法指出了最糟情况下的运行时间**

- **一些常见的大O运行时间,从快到慢: **(log指的都是log2)
    - O(log n)，也叫对数时间，这样的算法包括二分查找
    - O(n)，也叫线性时间，这样的算法包括简单查找
    - O(n * log n)，快速排序,一种速度较快的排序算法,  (n* [2为底的多少次幂])
    - O(n2)，选择排序,一种速度较慢的排序算法
    - O(n!)，一种非常慢的算法, n的阶乘



## 二分查找法

**二分查找的运行时间为对数时间(或log时间)** ,O(logn)

**只能用于有序元素列表**

```c++
int binary(const int* const  arr, const int arrLen, const int item){
    int low = 0;
    int tempArrLen = arrLen-1;
    if(arrLen == 0)
        return -2;
    // 优化: 极速查找. 0和末尾是最后一个被查找的, 先进行探索
    if(arr[0] == item)
        return 0;
    if(arr[tempArrLen] == item)
        return tempArrLen;
    
    while(low <= tempArrLen){
        int min = (low+tempArrLen)/2;  //1,0
        int guess = arr[min];
        if( guess == item)
            return min;
        if( guess > item)
            tempArrLen = min-1;
        else
            low = min +1;
    }
    return -1;    //没找到
}
```





## 选择排序

**时间复杂度在 O(n^2), 太慢了**

```c++
#include <iostream>
#include <vector>
/* 查找最小值 , 返回下标*/
std::vector<int>::iterator
findSmallest( std::vector<int>& arr){
    int n = arr.at(0);
    std::vector<int>::iterator  it = arr.begin();
    if( 1 >= arr.size()){
        return it;
    }
    for (std::vector<int>::iterator item = it; item != arr.end(); item++ ) {
        if(n >  *item){
            n =  *item;
            it =  item;
        }
    }
    return it;
}

/* 从小到大进行排序 */
int
selectionSort(std::vector<int>& arr){
    std::vector<int> ret;
    for(std::vector<int>::iterator item = arr.begin() ; item != arr.end(); ) {
        auto it = findSmallest(arr);
        ret.push_back(*it);
        arr.erase(it);
    }
    arr.swap(ret);     // 交换两个容器的内容
    return 0;
}

int main(void){
    srand48(100000);
    std::vector<int>arr;
    for(int a=0; a < 20000; a++){
        arr.push_back(rand() %10000000 +1);
        //printf("%d  ",arr.at(a));
    }
    printf("on\n");
    
    selectionSort(arr);

    printf("ok\n");
    return 0;
}
```





## 快速排序

- **递归条件   指的是函数调用自己**
- **基线条件  递归停止条件**
- 递归式问题解决的方法思路 **分而治之** (D&C)
    - **D&C的定义，每次递归调用都必须 缩小问题的规模**
    - **D&C工作原理:**
        - **找出简单的基线条件;**
        - **确定如何缩小问题的规模，使其符合 基线条件(递归停止条件)**
- **实现快速排序时，请随机地选择用作基准值的元素。快速排序的平均运行时间为O(n log n)**

```c++
/* 寻找数组中, 最大的那个元素的值,并返回 */
int factorial(const int* const arr, const int arrLen ){
    if(arrLen <= 1)
        return arr[0];
    int temp = factorial(arr+1, arrLen -1);
    return arr[0] > temp ? arr[0] : temp;
}
```

```c++
/* 填坑式 快排 */
void qSort(int arr[], int start , int end){
    int small = start;
    int big = end;
    int pivot = arr[small];   /* 基准,也把 start位置的值挖走  */
    if(small < big){  /* 基线条件 */
        while (small < big) {
            while ( small < big && arr[big] >= pivot)  {
                big--;    /* 这是大于基准的值 */
            } /* 一旦退出, 就是小于或等于基准值了 */
            if( small < big){
                arr[small] = arr[big];    /* 填坑 */
                small++;
            }
            
            while (small < big && arr[small] < pivot){
                small++;    /* 小于等于 基准的值 */
            }/* 一旦退出, 就是大于基准值了 */
            if (small < big){
                arr[big] = arr[small];   /* 填坑 */
                big--;
            }
        }
        arr[small]  = pivot;  /* 填补中间残留的坑*/
        qSort(arr, start  , small -1);
        qSort(arr, big+1, end);
    }
}
```

```python
def quicksort(array):
    if len(array) < 2:
        return array    # 基线条件:为空或只包含一个元素的数组是“有序”的
    else:
        pivot = array[0]     #递归条件
        less = [i for i in array[1:] if i <= pivot] #由所有小于基准值的元素组成的子数组
        greater = [i for i in array[1:] if i > pivot] #由所有大于基准值的元素组成的子数组
    return quicksort(less) + [pivot] + quicksort(greater)
   
print(quicksort([10, 5, 2, 3])) #测试
```







## 散列表

**散列表是数据结构(数组), 散列函数查找散列表速度为 O(1), 最糟糕的情况是 O(n)**

- **散列表由键和值组成, 与 map结构相同,但不是树,而是数组**
- **散列表是包含额外逻辑的数据结构**
- 散列函数“将输入映射到数字”
    - 它必须是一致的。例如，假设你输入apple时得到的是4，那么每次输入apple时，得到的都 必须为4。如果不是这样，散列表将毫无用处。
    - 它应将不同的输入映射到不同的数字。例如，如果一个散列函数不管输入是什么都返回1， 它就不是好的散列函数。最理想的情况是，将不同的输入映射到不同的数字。
- **散列函数准确地指出了存储位置，你根本不用查找**
- **散列表使用散列函数来确定元素的存储位置**
    - **散列函数总是将同样的输入映射到相同的索引**
    - **散列函数将不同的输入映射到不同的索引**
    - **散列函数知道数组有多大，只返回有效的索引**
- **使用散列表来检查是否重复，速度非常快**
- **缓存是一种常用的加速方式，所有大型网站都使用缓存，而缓存的数据则存储在散列表中**



- **散列表适用于:**
    - **模拟映射关系**
    - **防止重复**
    - **缓存/记住数据，以免服务器再通过处理来生成它们**



- 避免冲突的散列表, 应该在数组内存储链表, 而不是单独的数据了
    - **较低的填装因子**
        - 计算方式:  `散列表包含的元素数 / 位置总数`
            - 就是 `已用/总容量`
            - **一旦填装因子大于 0.7 ,就需要将数组增大一倍**
    - **良好的散列函数**
        - 散列函数很重要,好的散列函数很少导致冲突,还拥有更好的性能

```python
# 检查某个键值 是否存在于散列表中
voted = {}    #保存在全局的散列表
def check_voter(name): 
    if voted.get(name):
        print ("kick them out!")
    else:
        voted[name] = True
        print ("let them vote!")

```

```c++
#include <iostream>
#include <vector>
#include <string>

/*
 * 散列函数, 接受字符串, 以第一个字母为单位进行搜寻和放置,重复就替换
 */
class Str{
public:
    Str() {
        for(int i=0; i < 26; i++)
            vec[i]= -1;
    }
    ~Str(){}
    
    
    /* 搜寻 */
    int  findStr( const char*& str){
        if (str!= nullptr && *str == '\0')
            return -1;    /* 空字符串, 强制返回 -1*/
        return  vec[selectSL(str)];
    }
    
    /* 添加 , 键值str 数据n*/
    void addStr(const char* str, const int& n){
        if (str!= nullptr && *str == '\0')
            return;    /* 空字符串, 强制返回 */
        vec[selectSL(str)] = n;
    }
    
private:
    /* 得到下标 */
    int selectSL(const char*& v){
        int n = static_cast<int>(v[0]) - 97;
        n = n > 26 ? 26 : n;
        return n < 0 ? 0 : n;
    }
private:
    int  vec[26];
};



int main(void){
    Str str;
    str.addStr("a", 10);
    str.addStr("b", 20);
    str.addStr("c", 30);
    str.addStr("e", 40);
    str.addStr("p", 50);
    const char* p = "a";
    std::cout <<  str.findStr(p) << std::endl;
}

```





## 广度优先搜索

- **图: 由节点(node)和边(edge)组成**

    - **一个节点可能与众多节点直接相连，这些节点被称为邻居 (距离为1的)**
    - **`图用于模拟不同的东西是如何相连的`**
        - **有向图中的边为箭头，箭头的方向指定了关系的方向，例如，rama→adit表示rama欠adit钱**
        - **无向图中的边不带箭头，其中的关系是双向的，例如，ross - rachel表示“ross与rachel约会，而rachel也与ross约会”**

- **图算法    广度优先**

- **广度优先搜索让你能够找出两样东西之间的最短距离**

    - **`你需要按加入顺序检查搜索列表中的人，否则找到的就不是最短路径，因此搜索列表必须是队列`**

- **解决最短 路径问题的算法被称为 `广度优先搜索`**

- **使用广度优先搜索可以:**

    - 编写国际跳棋AI，计算最少走多少步就可获胜

    - 编写拼写检查器，计算最少编辑多少个地方就可将错拼的单词改成正确的单词，如将

        READED改为READER需要编辑一个地方

    - 根据你的人际关系网络找到关系最近的医生

- **对于检查过的人，务必不要再去检查，否则可能导致无限循环**

- **广度优先搜索的运行时间为 O(节点数+ 边数)，这通常写作O(V + E)，其中V为顶点(vertice)数，E为边数**



```python
from collections import deque   #当成一个是库, 一个是头文件
# 图的节点
graph={}    #字典类型
graph['you']= ['alice', 'bob','claire']    #散列表, 一个 键值, 对应多个实质
graph['bob']= ["anuj", "peggy"]
graph["alice"] = ["peggy"]
graph["claire"] = ["thom", "jonny"]
graph["anuj"] = []
graph["peggy"] = []
graph["thom"] = []     #结尾为 m的 就是那个需要找到的元素
graph["jonny"] = []



def person_is_seller(name):
    return name[-1] == 'm'

def search(name):
    search_queue = deque()   #队列, 先进先出 FIFO (栈是 LIFO 后进先出)
    search_queue+=graph[name]
    searched = []    #list  列表类型
    while search_queue :
        person = search_queue.popleft()   #取出并删除第一个人,也就是第一个元素的值 'alice'
        if   not person in searched:
            if person_is_seller(person):
                print (person + " is a mango seller!")
                return True
            else:
                search_queue += graph[person] #没有找到, 就把 该元素的临近元素都加入到队列
                searched.append(person)  #标记为已检查过
    return False


search('you')   #执行
```





## 狄克斯特拉算法

