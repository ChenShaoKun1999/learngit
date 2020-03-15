# 线性表(Linear List)

## 定义

数据对象：$D = \{a_i \, | \, i = 1, 2, ..., n, n \ge 0\}$

数据关系：$R = \{ \langle a_{i-1}, \, a_i \rangle \, | \, i = 2, 3, ..., n\}$

基本操作：初始化，销毁，清空，判断是否空表，求表长，读指定位置表元，求前驱，求后继，插入，删除，合并，复制，etc.

## 实现

```c++
typedef int Status;
const Status OK = 1, ERROR = 0, INFEASIBLE = -1, OVERFLOW = -2;

//顺序表实现
//用一段连续地址存放，读表元复杂度O(1)，插入删除复杂度都是O(n)
class SqList
{
    ElemType* elem;
    int length;
    int size;
public:
    SqList();
    Status init();
    Status getElem(int i, ElemType* e);
    Status insert(int i, ElemType e);
    Status delete_(int i);
    Status destroy();
}

//单链表实现
//



```

