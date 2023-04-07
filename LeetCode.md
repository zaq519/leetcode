# LeetCode题目

## 1 单链表

[单链表解题技巧]: https://labuladong.github.io/algo/di-yi-zhan-da78c/shou-ba-sh-8f30d/shuang-zhi-0f7cc/

### 1.1 合并两个有序链表

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

### 1.2 分隔链表

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

### 1.3 合并K个升序链表

[LeetCode:23]: https://leetcode.cn/problems/merge-k-sorted-lists/

解题思路：题目要求将多个链表按照升序规则，合并成一个升序链表。使用**优先队列**，记录每个子链表中的头节点，根据**优先队列（最小堆）**的特性，快速得到k个节点中的最小节点。

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
		
        ListNode* dummy = new ListNode(-1);
        ListNode* p = dummy;
        priority_queue<ListNode*, vector<ListNode*>, function<bool const(ListNode*, ListNode*)>> pq(
        [] (ListNode* a, ListNode* b) {return a->val > b->val;}
        );
        for(auto list : lists) {
            if (list != nullptr)
                pq.push(list);
        }
        while(!pq.empty()){
            ListNode* node = pq.top();
            pq.pop();
            p->next = node;
            node = node->next;
            if(node != nullptr)
                pq.push(node);
            p = p->next;
        }
        return dummy->next;
    }
};
```

堆排序时间复杂度O(logk)，k为队列中元素个数

所有链表中的元素被加入和弹出队列，O(n)

该题时间复杂度O(nlogk)

### 1.4 删除链表的倒数第N个节点

[LeetCode:19]: https://leetcode.cn/problems/remove-nth-node-from-end-of-list/

寻找**第k个节点**：一遍for循环

寻找**倒数第k个节点**：

- 倒数第k个节点，相当于正数第n-k+1个节点
- 双指针，p1指针先走k次，p2指针从头节点再走，直到p1到达队尾，p2指针为倒数第k个

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(n == 0) return head;
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode* p1 = dummy;
        for(int i=0;i<n+1;i++)
            p1 = p1->next;
        ListNode* p2 = dummy;
        while(p1) {
            p2 = p2->next;
            p1 = p1->next;
        }
        p2 ->next = p2->next->next;
        return dummy->next;
    }
};
```



# 附录 C++知识

## 1 priority_queue 优先队列

[priority_queue]: https://blog.csdn.net/Strengthennn/article/details/119078911

### 1.1 priority_queue的定义

```cpp
#include<queue>
using namespace std;
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

### 1.2 priority自定义排序

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
// less 重载< 运算符，greater 重载> 运算符
```

- 仿函数

使用仿函数作为Compare双参判断式。

```cpp
class cmp{
public:
    bool operator()(const Node &a, const Node &b) {
        return a.size == b.size ? a.price > b.price : a.size < b.size;
    }
};
```

- lamda表达式

```cpp
auto cmp = [](const Node &a, const Node &b) { return a.size == b.size ? a.price > b.price : a.size < b.size; };
priority_queue<Node, vector<Node>, decltype(cmp)> priorityQueue(cmp);


priority_queue<Node, vector<Node>, function<bool(const Node&, const Node&)>> priorityQueue(cmp);

```

### 1.3常用函数

 ```cpp
 priority_queue<int, vector<int>,less<int> > pq;
 pq.push(1);//压入队列
 pq.top();//获取队首元素
 pq.pop();//弹出队首元素
 pq.empty();//判断队列是否为空
 ```

