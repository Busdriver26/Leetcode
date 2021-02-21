#  02.01. Remove Duplicate Node LCCI

Write code to remove duplicates from an unsorted linked list.

Example1:

 Input: [1, 2, 3, 3, 2, 1]
 Output: [1, 2, 3]
Example2:

 Input: [1, 1, 1, 1, 2]
 Output: [1, 2]
Note:

The length of the list is within the range[0, 20000].
The values of the list elements are within the range [0, 20000].
Follow Up:

How would you solve this problem if a temporary buffer is not allowed?

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeDuplicateNodes(ListNode* head) {
        //temporary buffer not allowed
        if(head == NULL||head->next==NULL)return head;
        ListNode* pa=head;
        ListNode* pb=head->next;
        ListNode* last = head;
        while(pa!=NULL){
            pb = pa->next;
            while(pb!=NULL){
                if(pb->val==pa->val){
                    pb = pb->next;
                    last->next = pb;
                }
                else{
                    pb = pb->next;
                    last = last->next;
                }
            }
            pa=pa->next;
            last = pa;
        }
        return head;        
    }
};
```

The very first idea is to use hash table to track elements. It takes O(n) time and space.

Following up : No buffer allowed

My way is to search the list every time when p = p->next.

#  02.02. Kth Node From End of List LCCI

Implement an algorithm to find the kth to last element of a singly linked list. Return the value of the element.

Note: This problem is slightly different from the original one in the book.

Example:

Input:  1->2->3->4->5 和 k = 2
Output:  4
Note:

k is always valid.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int kthToLast(ListNode* head, int k) {
        ListNode *p1,*p2;
        p1 = head;
        p2 = head;
        for(int i=0;i<k;i++){
            p1=p1->next;
        }
        while(p1!=NULL){
            p1=p1->next;
            p2=p2->next;
        }
        return p2->val;
    }
};
```

