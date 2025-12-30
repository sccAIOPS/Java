# Singly Linked List

> **Category:** Data Structures  
> **Subcategory:** Linear Data Structures  
> **Implementation:** [SinglyLinkedList.java](../../src/main/java/com/thealgorithms/datastructures/lists/SinglyLinkedList.java)

---

## 📚 Overview

A **Singly Linked List** is a linear data structure consisting of nodes where each node contains data and a reference (pointer) to the next node in the sequence. Unlike arrays, linked lists don't require contiguous memory allocation, allowing efficient insertions and deletions but sacrificing random access capability.

---

## 🔢 Mathematical Foundation

### Structure Definition

A singly linked list L consists of:
- **head:** Pointer to the first node (null if empty)
- **Nodes:** Each containing data and a next pointer

Formally: $L = \{n_1 \rightarrow n_2 \rightarrow ... \rightarrow n_k \rightarrow null\}$

### Key Properties

- **Sequential Access:** Must traverse from head to reach any node
- **Dynamic Size:** Can grow or shrink during execution
- **Memory Overhead:** Each node requires extra space for pointer
- **No Index Access:** Cannot directly access by index like arrays

---

## 📊 Complexity Analysis

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| **Access by Index** | $O(n)$ | Must traverse from head |
| **Search** | $O(n)$ | Linear traversal required |
| **Insert at Head** | $O(1)$ | Direct pointer update |
| **Insert at Tail** | $O(n)$ or $O(1)$* | *O(1) if tail pointer maintained |
| **Insert at Position** | $O(n)$ | Find position first |
| **Delete at Head** | $O(1)$ | Direct pointer update |
| **Delete at Tail** | $O(n)$ | Must find second-to-last |
| **Delete by Value** | $O(n)$ | Search + delete |
| **Get Size** | $O(n)$ or $O(1)$* | *O(1) if size counter maintained |

### Space Complexity

- **Per Node:** $O(1)$ for data + $O(1)$ for next pointer
- **Total:** $O(n)$ for n elements
- **Overhead vs Array:** Extra pointer per element (~4-8 bytes per node)

---

## 🔄 Operations (Pseudocode)

### Node Structure
```
CLASS Node
    data: T
    next: Node
    
    CONSTRUCTOR(data)
        this.data ← data
        this.next ← null
```

### Insert at Head
```
ALGORITHM InsertAtHead(head, data)
    1. newNode ← new Node(data)
    2. newNode.next ← head
    3. head ← newNode
    4. RETURN head
```

### Insert at Tail
```
ALGORITHM InsertAtTail(head, data)
    1. newNode ← new Node(data)
    2. IF head = null THEN
    3.     RETURN newNode
    4. current ← head
    5. WHILE current.next ≠ null DO
    6.     current ← current.next
    7. current.next ← newNode
    8. RETURN head
```

### Delete by Value
```
ALGORITHM Delete(head, value)
    1. IF head = null THEN RETURN null
    2. IF head.data = value THEN RETURN head.next
    3. current ← head
    4. WHILE current.next ≠ null AND current.next.data ≠ value DO
    5.     current ← current.next
    6. IF current.next ≠ null THEN
    7.     current.next ← current.next.next
    8. RETURN head
```

### Search
```
ALGORITHM Search(head, value)
    1. current ← head
    2. WHILE current ≠ null DO
    3.     IF current.data = value THEN RETURN current
    4.     current ← current.next
    5. RETURN null
```

### Reverse List
```
ALGORITHM Reverse(head)
    1. prev ← null
    2. current ← head
    3. WHILE current ≠ null DO
    4.     next ← current.next
    5.     current.next ← prev
    6.     prev ← current
    7.     current ← next
    8. RETURN prev
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

The repository implementation includes:
- **Generic Type:** `SinglyLinkedList<E>` supports any element type
- **Inner Node Class:** Encapsulated `Node` class
- **Sentinel Head:** Option for dummy head node to simplify operations
- **Size Tracking:** Maintains count for O(1) size queries
- **Utility Methods:** `reverse()`, `detectLoop()`, `getNth()`, `middle()`

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/lists/SinglyLinkedList.java`

```java
public class SinglyLinkedList<E> implements Iterable<E> {
    private Node<E> head;
    private int size;
    
    private static class Node<E> {
        E data;
        Node<E> next;
        
        Node(E data) {
            this.data = data;
        }
    }
    
    public void insertHead(E data) {
        Node<E> newNode = new Node<>(data);
        newNode.next = head;
        head = newNode;
        size++;
    }
    
    public void insertTail(E data) {
        Node<E> newNode = new Node<>(data);
        if (head == null) {
            head = newNode;
        } else {
            Node<E> current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
        size++;
    }
    
    public void reverse() {
        Node<E> prev = null;
        Node<E> current = head;
        while (current != null) {
            Node<E> next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }
        head = prev;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Undo Functionality
**Use Case:** Browser back button, editor undo  
**Example:** Each state stored as a node, allowing sequential undo operations

### 2. Music/Media Playlists
**Use Case:** Sequential playback with next track navigation  
**Example:** Spotify's play queue implementation

### 3. Memory Management
**Use Case:** Free memory block tracking  
**Example:** Operating systems maintain free memory as linked list

### 4. Polynomial Representation
**Use Case:** Mathematical computation  
**Example:** Each term as a node with coefficient and exponent

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **Browsers** | History navigation (back/forward) |
| **Photo Apps** | Image carousel/gallery |
| **Linux Kernel** | Process scheduling queues |
| **Database Systems** | Overflow chains in hash tables |

---

## ⚖️ Comparison with Other Data Structures

| Feature | Singly LL | Doubly LL | Array | ArrayList |
|---------|-----------|-----------|-------|-----------|
| **Memory per element** | data + 1 ptr | data + 2 ptr | data only | data + overhead |
| **Random Access** | O(n) | O(n) | O(1) | O(1) |
| **Insert at front** | O(1) | O(1) | O(n) | O(n) |
| **Insert at end** | O(n)* | O(1) | N/A | O(1)† |
| **Delete at front** | O(1) | O(1) | O(n) | O(n) |
| **Delete at end** | O(n) | O(1) | N/A | O(1) |
| **Memory allocation** | Dynamic | Dynamic | Static | Dynamic |

*O(1) if tail pointer is maintained  
†Amortized O(1)

### When to Use Singly Linked List

✅ **Use when:**
- Frequent insertions/deletions at the beginning
- Unknown size that changes frequently
- Memory is fragmented
- Only forward traversal needed

❌ **Avoid when:**
- Random access required frequently
- Backward traversal needed
- Memory efficiency is critical (array better)
- Cache locality matters (arrays better)

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Null Pointer:** Forgetting to check for empty list
   - Solution: Always check `head == null` before operations

2. **Lost Reference:** Not maintaining previous node during deletion
   - Solution: Use two-pointer technique

3. **Memory Leak:** Not properly handling node removal (in non-GC languages)
   - Solution: Set removed node's next to null

4. **Loop Detection:** List with cycle causes infinite traversal
   - Solution: Use Floyd's cycle detection (tortoise and hare)

### Edge Cases to Handle

- [x] Empty list (head = null)
- [x] Single element list
- [x] Inserting into empty list
- [x] Deleting head node
- [x] Deleting non-existent element
- [x] Lists with cycles (for detection algorithms)

---

## 📖 References

1. Knuth, D.E. "The Art of Computer Programming, Vol. 1" (1997)
2. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS)
3. [Wikipedia: Linked List](https://en.wikipedia.org/wiki/Linked_list)

---

## 🔗 Related Data Structures

- [Doubly Linked List](./doubly-linked-list.md) - Bidirectional traversal
- [Circular Linked List](./circular-linked-list.md) - Last points to first
- [Stack](./stack.md) - Often implemented with linked list
- [Queue](./queue.md) - FIFO using linked list
- [Skip List](./skip-list.md) - Multi-level linked list for fast search
