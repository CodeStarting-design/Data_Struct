# LeetCode-92-反转链表 II

给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表 。

## 确定首尾后翻转

分类讨论：

```C++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* pre=nullptr;
        if(left!=1){
            // 寻找前缀
            int cur=1;
            ListNode* tmp=head;
            while(cur!=left-1){ 
                tmp=tmp->next;
                cur++;
            }
            pre=tmp;
        }
        ListNode* lNode=pre?pre->next:head,*rNode=head;
        int cur=1;
        while(cur!=right) {
            rNode=rNode->next;
            cur++;
        }
        ListNode* vhead=new ListNode();
        vhead->next=rNode->next;
        rNode->next=nullptr;
        while(lNode!=nullptr){
            ListNode* tmp=lNode->next;
            lNode->next=vhead->next;
            vhead->next=lNode;
            lNode=tmp;
        }
        if(pre){
            pre->next=vhead->next;
            return head;
        }else{
            return vhead->next;
        }
    }
};
```

使用虚拟头节点统一处理：

```C++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* vHead=new ListNode(-1,head);
        int cur=0;
        ListNode* pre=vHead,*rNode=vHead;
        while(cur!=left-1){
            pre=pre->next;
            cur++;
        }
        rNode=pre;
        while(cur!=right){
            rNode=rNode->next;
            cur++;
        }
        ListNode *nHead=new ListNode(-1,rNode->next);
        rNode->next=nullptr;
        rNode=pre->next;
        while(rNode){
            ListNode* tmp=rNode->next;
            rNode->next=nHead->next;
            nHead->next=rNode;
            rNode=tmp;
        }
        pre->next=nHead->next;
        return vHead->next;
    }
};
```
