
### Linked List Theory Basics

A linked list is a linear data structure connected by pointers. Each node consists of **two parts**: a data field and a pointer field (which points to the next node). The last node points to `null`.

The entry node is called the **head**.

#### Types of Linked Lists

**Singly Linked List**  
Each node points only to its next node.

**Doubly Linked List**  
Each node has two pointers: one to the next node and one to the previous node. Supports traversal in both directions.

**Circular Linked List**  
The last node points back to the head, forming a circle. Used in problems like Josephus problem.

#### Storage in Memory

Unlike arrays (which are contiguous), linked list nodes are scattered throughout memory. Nodes are connected via their pointers; actual memory addresses are managed by the OS.

#### Node Definition

**C++ example:**
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};
```
If no constructor is defined, a default one is provided but does not initialize members.

Example with custom constructor:
```cpp
ListNode* head = new ListNode(5);
```
Example with default constructor:
```cpp
ListNode* head = new ListNode();
head->val = 5;
```

#### Operations

**Delete a node**  
Point the previous node's `next` to the node after the one being deleted. In C++, manually free the node.

**Add a node**  
Adjust pointers to insert without affecting other nodes.

Note: To delete or add, you must first locate the previous node (O(n)), but actual pointer adjustment is O(1).

#### Performance Comparison

| Feature              | Array                      | Linked List                  |
|-----------------------|----------------------------|-----------------------------|
| Memory layout         | Contiguous                | Non-contiguous              |
| Fixed length          | Yes                       | No                          |
| Insert/Delete         | Expensive (O(n))         | Fast (O(1) after locating)  |
| Search                | Fast (O(1) by index)    | Slow (O(n))                |

---

### LeetCode 203. Remove Linked List Elements

Remove all nodes of a linked list that have a specific value.

**C++ Code**
```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyhead = new ListNode(0);
        dummyhead->next = head;
        ListNode* cur = dummyhead;

        while (cur->next != NULL) {
            if (cur->next->val == val) {
                cur->next = cur->next->next;
            } else {
                cur = cur->next;
            }
        }
        return dummyhead->next;
    }
};
```

Key: Use a dummy head to simplify edge cases.

---

### LeetCode 707. Design Linked List

Implement get, addAtHead, addAtTail, addAtIndex, and deleteAtIndex operations.

**C++ Code**
```cpp
class MyLinkedList {
public:
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val) : val(val), next(nullptr) {}
    };
    
    MyLinkedList() {
        _dummyhead = new LinkedNode(0);
        _size = 0;
    }

    int get(int index) {
        if (index > _size - 1 || index < 0) return -1;
        LinkedNode* cur = _dummyhead->next;
        while (index--) {
            cur = cur->next;
        }
        return cur->val;
    }
    
    void addAtHead(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        newNode->next = _dummyhead->next;
        _dummyhead->next = newNode;
        _size++;
    }
    
    void addAtTail(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyhead;
        while (cur->next != nullptr) {
            cur = cur->next;
        }
        cur->next = newNode;
        _size++;
    }
    
    void addAtIndex(int index, int val) {
        if (index > _size) return;
        if (index < 0) index = 0;
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyhead;
        while (index--) {
            cur = cur->next;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= _size || index < 0) return;
        LinkedNode* cur = _dummyhead;
        while (index--) {
            cur = cur->next;
        }
        LinkedNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        tmp = nullptr;
        _size--;
    }

private:
    int _size;
    LinkedNode* _dummyhead;
};
```

---

### LeetCode 206. Reverse Linked List

Reverse a singly linked list.

**Two Pointers Approach**
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* cur = head;
        while (cur != nullptr) {
            ListNode* tmp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = tmp;
        }
        return prev;
    }
};
```

**Recursive Approach with Helper Function**
```cpp
class Solution {
public:
    ListNode* reverse(ListNode* pre, ListNode* cur) {
        if (cur == NULL) return pre;
        ListNode* temp = cur->next;
        cur->next = pre;
        return reverse(cur, temp);
    }
    ListNode* reverseList(ListNode* head) {
        return reverse(NULL, head);
    }
};
```

---

### LeetCode 24. Swap Nodes in Pairs

Swap every two adjacent nodes.

**C++ Code**
```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead;

        while (cur->next != NULL && cur->next->next != NULL) {
            ListNode* tmp = cur->next;
            ListNode* tmp1 = cur->next->next->next;
            cur->next = cur->next->next;
            cur->next->next = tmp;
            cur->next->next->next = tmp1;
            cur = cur->next->next;
        }
        return dummyHead->next;
    }
};
```

---

### LeetCode 19. Remove N-th Node From End of List

Remove the n-th node from the end of the list.

**C++ Code**
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* fast = dummyHead;
        ListNode* slow = dummyHead;

        while (n-- && fast != NULL) {
            fast = fast->next;
        }

        fast = fast->next;
        while (fast != NULL) {
            fast = fast->next;
            slow = slow->next;
        }

        slow->next = slow->next->next;
        return dummyHead->next;
    }
};
```

---

### LeetCode 142. Linked List Cycle II

Return the node where the cycle begins. If there is no cycle, return NULL.

**C++ Code**
```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;

        while (fast != NULL && fast->next != NULL) {        
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) {
                ListNode* index1 = head;
                ListNode* index2 = fast;
                while (index1 != index2) {
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index1;
            }
        }
        return NULL;
    }
};
```

---
