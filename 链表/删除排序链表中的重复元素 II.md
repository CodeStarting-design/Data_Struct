# LeetCode-82-删除排序链表中的重复元素 II

给定一个已排序的链表的头 head ， 删除原始链表中所有重复数字的节点，只留下不同的数字 。返回 已排序的链表 。

## 虚拟头节点

pre指向虚拟头节点，当向后找到了不重复的节点时则 `pre->next=q;pre=q;pre->next=nullptr`

```C++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head) return head;
        ListNode* vHead=new ListNode(-1,head),*pre=vHead;
        ListNode* q=head;
        pre->next=nullptr;
        while(q){
            ListNode* tmp=q->next;
            while(tmp&&tmp->val==q->val){
                tmp=tmp->next;
            }
            if(tmp==q->next){
                pre->next=q;
                pre=q;
                pre->next=nullptr;
            }
            q=tmp;
        }
        return vHead->next;
    }
};
```

简化版：对于重复的元素保留一个

```C++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* vHead=new ListNode(-1,head),*pre=vHead;
        ListNode* q=head;
        while(q){
            ListNode* tmp=q->next;
            while(tmp&&tmp->val==q->val){
                tmp=tmp->next;
            }
            pre->next=q;
            pre=q;
            pre->next=nullptr;
            q=tmp;
        }
        
        return vHead->next;
    }
};
```
