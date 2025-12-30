# Queue

> **Category:** Data Structures  
> **Subcategory:** Linear Data Structures (FIFO)  
> **Implementation:** [Queue.java](../../src/main/java/com/thealgorithms/datastructures/queues/Queue.java)

---

## 📚 Overview

A **Queue** is an abstract data type that follows the **First-In-First-Out (FIFO)** principle. Elements are added at the rear (enqueue) and removed from the front (dequeue). This structure is essential for managing ordered processing in operating systems, networking, and breadth-first algorithms.

---

## 🔢 Mathematical Foundation

### Definition

A queue Q is a sequence of elements with operations at opposite ends:
- **enqueue(x):** Add element x to the rear
- **dequeue():** Remove and return the front element
- **front()/peek():** Return front element without removing

### FIFO Property

If elements are enqueued in order $\langle a_1, a_2, ..., a_n \rangle$, then dequeues return elements in the same order $\langle a_1, a_2, ..., a_n \rangle$.

### Key Invariants

- First element added is first element removed
- Size after k enqueues and m dequeues: $|Q| = k - m$

---

## 📊 Complexity Analysis

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| **enqueue(x)** | $O(1)$ | Add to rear |
| **dequeue()** | $O(1)$ | Remove from front |
| **front()/peek()** | $O(1)$ | View front element |
| **isEmpty()** | $O(1)$ | Check if empty |
| **size()** | $O(1)$ | Return count |

### Space Complexity

- **Array-based (circular):** $O(n)$ fixed allocation
- **Linked list-based:** $O(n)$ with pointer overhead
- **Dynamic array:** $O(n)$ with amortized resize

---

## 🔄 Operations (Pseudocode)

### Queue Interface
```
INTERFACE Queue<T>
    enqueue(item: T): void
    dequeue(): T
    front(): T
    isEmpty(): boolean
    size(): int
```

### Circular Array Implementation
```
CLASS CircularArrayQueue<T>
    array: T[]
    front: int ← 0
    rear: int ← -1
    size: int ← 0
    capacity: int
    
    ALGORITHM enqueue(item)
        IF size = capacity THEN
            THROW QueueFullException
        rear ← (rear + 1) MOD capacity
        array[rear] ← item
        size ← size + 1
    
    ALGORITHM dequeue()
        IF isEmpty() THEN
            THROW QueueEmptyException
        item ← array[front]
        front ← (front + 1) MOD capacity
        size ← size - 1
        RETURN item
    
    ALGORITHM front()
        IF isEmpty() THEN
            THROW QueueEmptyException
        RETURN array[front]
    
    ALGORITHM isEmpty()
        RETURN size = 0
```

### Linked List Implementation
```
CLASS LinkedQueue<T>
    head: Node<T>      // front
    tail: Node<T>      // rear
    size: int
    
    ALGORITHM enqueue(item)
        newNode ← new Node(item)
        IF tail = null THEN
            head ← tail ← newNode
        ELSE
            tail.next ← newNode
            tail ← newNode
        size ← size + 1
    
    ALGORITHM dequeue()
        IF head = null THEN
            THROW QueueEmptyException
        item ← head.data
        head ← head.next
        IF head = null THEN
            tail ← null
        size ← size - 1
        RETURN item
```

### Circular Queue Visual

```
Initial: front=0, rear=-1, capacity=5
         [_, _, _, _, _]
          ↑
          front

After enqueue(A,B,C):
         [A, B, C, _, _]
          ↑        ↑
        front    rear

After dequeue():
         [_, B, C, _, _]
             ↑     ↑
           front  rear

After enqueue(D,E,F): (wraps around)
         [F, B, C, D, E]
          ↑  ↑
        rear front
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

The repository provides multiple queue implementations:
- **Queue<T>:** Generic array-based circular queue
- **LinkedQueue<T>:** Linked list implementation
- **CircularQueue<T>:** Explicit circular buffer
- **PriorityQueues:** Priority-based ordering

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/queues/Queue.java`

```java
public class Queue<T> {
    private static final int DEFAULT_CAPACITY = 10;
    private T[] array;
    private int front;
    private int rear;
    private int size;
    
    @SuppressWarnings("unchecked")
    public Queue() {
        array = (T[]) new Object[DEFAULT_CAPACITY];
        front = 0;
        rear = -1;
        size = 0;
    }
    
    public void enqueue(T item) {
        if (size == array.length) {
            resize();
        }
        rear = (rear + 1) % array.length;
        array[rear] = item;
        size++;
    }
    
    public T dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        T item = array[front];
        array[front] = null; // Help GC
        front = (front + 1) % array.length;
        size--;
        return item;
    }
    
    public T peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return array[front];
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Task Scheduling
**Use Case:** CPU process scheduling, print job spooling  
**Example:** Operating systems use ready queues for process management

### 2. Message Queues
**Use Case:** Asynchronous communication between services  
**Example:** RabbitMQ, Apache Kafka, AWS SQS for microservices

### 3. Breadth-First Search
**Use Case:** Shortest path, level-order traversal  
**Example:** Social network friend suggestions, web crawlers

### 4. Request Handling
**Use Case:** Web server request processing  
**Example:** HTTP servers queue incoming requests for handling

### 5. Buffering
**Use Case:** Data streaming, I/O operations  
**Example:** Video buffering, keyboard input buffer

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **AWS SQS** | Message queuing service |
| **RabbitMQ** | Enterprise message broker |
| **Node.js** | Event loop task queue |
| **Print Servers** | Print job spooling |
| **Call Centers** | Customer service queue |

---

## ⚖️ Comparison with Related Data Structures

| Feature | Queue | Stack | Deque | Priority Queue |
|---------|-------|-------|-------|----------------|
| **Order** | FIFO | LIFO | Both ends | By priority |
| **Add** | rear | top | Both | Any |
| **Remove** | front | top | Both | Min/Max |
| **Use Case** | Scheduling | Backtracking | Sliding window | Dijkstra's |

### Queue Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| **Circular Queue** | Fixed array with wrap-around | Bounded buffers |
| **Priority Queue** | Elements ordered by priority | Scheduling, heap |
| **Double-ended Queue** | Add/remove both ends | Palindrome check |
| **Blocking Queue** | Thread-safe with blocking | Producer-consumer |

### Java Standard Library

```java
// Interface-based usage
Queue<Integer> queue = new LinkedList<>();
Queue<Integer> queue = new ArrayDeque<>();  // Recommended

// Blocking (concurrent)
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(100);

// Priority
Queue<Integer> pq = new PriorityQueue<>();
```

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Queue Underflow:** Dequeue from empty queue
   - Solution: Check `isEmpty()` before dequeue

2. **Queue Overflow:** Fixed-size queue filled
   - Solution: Use dynamic resize or throw exception

3. **Circular Index Calculation:** Off-by-one errors
   - Solution: Use modulo: `(index + 1) % capacity`

4. **Memory Leak:** Not nulling dequeued positions
   - Solution: Set `array[front] = null` after dequeue

### Edge Cases to Handle

- [x] Dequeue/peek from empty queue
- [x] Enqueue to full queue (fixed-size)
- [x] Single element queue operations
- [x] Queue wrapping in circular implementation

---

## 📖 References

1. Knuth, D.E. "The Art of Computer Programming, Vol. 1" (1997)
2. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS)
3. [Wikipedia: Queue (abstract data type)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))

---

## 🔗 Related Data Structures & Algorithms

- [Stack](./stack.md) - LIFO counterpart
- [Deque](./deque.md) - Double-ended queue
- [Priority Queue](../trees/priority-queue.md) - Ordered by priority
- [Circular Buffer](./circular-buffer.md) - Ring buffer implementation
- [BFS](../../05-graph-algorithms/bfs.md) - Queue-based graph traversal
