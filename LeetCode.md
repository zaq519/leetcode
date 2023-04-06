# 单链表

[单链表解题技巧]: https://labuladong.github.io/algo/di-yi-zhan-da78c/shou-ba-sh-8f30d/shuang-zhi-0f7cc/

## 1 合并两个有序链表

[Leetcode:21]: https://leetcode.cn/problems/merge-two-sorted-lists/

在需要创造新链表的时候，可以设置虚拟头节点简化边界情况的处理

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* head = dummy;
        while (list1 && list2)
        {
            if (list1->val <= list2->val)
            {
                head->next = list1;
                list1 = list1->next;
            }
            else if (list1->val > list2->val)
            {
                head->next = list2;
                list2 = list2->next;
            }
            head = head->next;
        }
        if (list1 != nullptr)
        {
            head->next = list1;
        }
        if (list2 != nullptr)
        {
            head->next = list2;
        }
        return dummy->next; 
    }
};
```

## 2 分隔链表

[Leetcode:86]: https://leetcode.cn/problems/partition-list/

解题思路：题目要求分隔链表，可以将链表先拆分再合并

```cpp
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode* dummy1 = new ListNode(-1);
        ListNode* dummy2 = new ListNode(-1);
        ListNode* head1 = dummy1;
        ListNode* head2 = dummy2;
        while(head) {
            if(head->val >= x) {
                dummy2->next = head;
                dummy2 = dummy2->next;

            }
            else{
                dummy1->next = head;
                dummy1 = dummy1->next;
            }
            ListNode* node = head->next;
            head->next = nullptr;
            head = node;

        }
        dummy1->next = head2->next;
        return head1->next;

    }
};
```

## 3 合并K个升序链表

[LeetCode:23]: https://leetcode.cn/problems/merge-k-sorted-lists/

解题思路：题目要求将多个链表按照升序规则，合并成一个升序链表。使用**优先队列**，记录每个子链表中的头节点，根据**优先队列（最小堆）**的特性，快速得到k个节点中的最小节点。

- cpp优先队列使用方法

```java
// priority_queue 可以解决一些贪心问题，也可以对 Dijkstra 算法进行优化。
#include<queue>
using namespace std;




//自定义排序
//1. 自定义类型比较关系

//lamda表达式
priority_queue<int, vector<int>, function<bool(int, int)>> pq(
            [] (int a, int b) {return a->val > b->val;}
        );
```

# 附录 C++知识

## priority_queue 优先队列

[priority_queue]: https://blog.csdn.net/Strengthennn/article/details/119078911

priority_queue的定义

```cpp
//T：元素类型。
//Container：低层存储容器类型，默认为vector< T >类型。
//Compare：两个参数且返回值为bool类型的双参判断式，默认为less< T >判断式。
template <class T, class Container = vector<T>,
  class Compare = less<typename Container::value_type> > class priority_queue;


//定义
priority_queue<int, vector<int> > pq; //默认：大根堆 队首为最大值
priority_queue<int, vector<int>,less<int> > pq1; //大根堆 队首为最大值
priority_queue<int, vector<int>, greater<int> > pq2; //小根堆 队首为最小值
```

priority自定义排序

- 自定义类型比较关系

通过重载"<"和">"运算符，使得自定义类型可以使用< , > 符号比较

```cpp
struct Node {
    int size;
    int price;
    // 重载<运算符
    bool operator<(const Node &b) const {
        return this->size == b.size ? this->price > b.price : this->size < b.size;
    }
};
```

