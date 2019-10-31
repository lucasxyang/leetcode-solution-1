# 2, 445. Add Two Numbers \[Series\]

> You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
>
> Input: \(2 -&gt; 4 -&gt; 3\) + \(5 -&gt; 6 -&gt; 4\)
>
> Output: 7 -&gt; 0 -&gt; 8

两个链表相加的问题，需要处理好进位就成了，比较简单，直接上代码：

```cpp
class Solution {
public:
    ListNode *addTwoNumbers(ListNode *l1, ListNode *l2) {
        ListNode dummy(0);
        ListNode* p = &dummy;

        int cn = 0;

        while(l1 || l2) {
            int val = cn + (l1 ? l1->val : 0) + (l2 ? l2->val : 0);
            cn = val / 10;
            val = val % 10;
            p->next = new ListNode(val);
            p = p->next;
            if(l1) {
                l1 = l1->next;
            }
            if(l2) {
                l2 = l2->next;
            }
        }

        if(cn != 0) {
            p->next = new ListNode(cn);
            p = p->next;
        }

        return dummy.next;
    }
};
```

