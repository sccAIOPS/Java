# Stack

> **Category:** Data Structures  
> **Subcategory:** Linear Data Structures (LIFO)  
> **Implementation:** [Stack.java](../../src/main/java/com/thealgorithms/datastructures/stacks/Stack.java)

---

## 📚 Overview

A **Stack** is an abstract data type that follows the **Last-In-First-Out (LIFO)** principle. Elements are added and removed from the same end, called the "top." This simple but powerful structure is fundamental to computer science, supporting recursion, expression evaluation, and backtracking algorithms.

---

## 🔢 Mathematical Foundation

### Definition

A stack S is a sequence of elements with operations restricted to one end:
- **push(x):** Add element x to the top
- **pop():** Remove and return the top element
- **peek()/top():** Return top element without removing

### LIFO Property

If elements are pushed in order $\langle a_1, a_2, ..., a_n \rangle$, then pops return elements in reverse order $\langle a_n, a_{n-1}, ..., a_1 \rangle$.

### Key Invariants

- Size after k pushes and m pops: $|S| = k - m$ (assuming valid operations)
- Pop on empty stack is undefined (throws exception)

---

## 📊 Complexity Analysis

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| **push(x)** | $O(1)$ | Add to top |
| **pop()** | $O(1)$ | Remove from top |
| **peek()/top()** | $O(1)$ | View top without removal |
| **isEmpty()** | $O(1)$ | Check if empty |
| **size()** | $O(1)$ | Return count |
| **search(x)** | $O(n)$ | Find element (non-standard) |

### Space Complexity

- **Array-based:** $O(n)$ with potential waste if under-utilized
- **Linked list-based:** $O(n)$ exact with pointer overhead
- **Both implementations:** Amortized O(1) per operation

---

## 🔄 Operations (Pseudocode)

### Stack Interface
```
INTERFACE Stack<T>
    push(item: T): void
    pop(): T
    peek(): T
    isEmpty(): boolean
    size(): int
```

### Array-Based Implementation
```
CLASS ArrayStack<T>
    array: T[]
    top: int ← -1
    capacity: int
    
    CONSTRUCTOR(capacity)
        this.capacity ← capacity
        array ← new T[capacity]
    
    ALGORITHM push(item)
        IF top = capacity - 1 THEN
            resize()      // Double capacity
        top ← top + 1
        array[top] ← item
    
    ALGORITHM pop()
        IF isEmpty() THEN
            THROW StackUnderflowException
        item ← array[top]
        top ← top - 1
        RETURN item
    
    ALGORITHM peek()
        IF isEmpty() THEN
            THROW StackUnderflowException
        RETURN array[top]
    
    ALGORITHM isEmpty()
        RETURN top = -1
```

### Linked List Implementation
```
CLASS LinkedStack<T>
    head: Node<T>
    size: int
    
    ALGORITHM push(item)
        newNode ← new Node(item)
        newNode.next ← head
        head ← newNode
        size ← size + 1
    
    ALGORITHM pop()
        IF head = null THEN
            THROW StackUnderflowException
        item ← head.data
        head ← head.next
        size ← size - 1
        RETURN item
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

The repository provides multiple stack implementations:
- **Stack<T> Interface:** Defines standard operations
- **StackArray<T>:** Fixed-size array implementation
- **StackArrayList<T>:** Dynamic ArrayList-backed implementation
- **NodeStack<T>:** Linked node implementation

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/stacks/Stack.java`

```java
public interface Stack<T> {
    void push(T item);
    T pop();
    T peek();
    boolean isEmpty();
    int size();
}
```

📁 **File:** `src/main/java/com/thealgorithms/datastructures/stacks/StackArray.java`

```java
public class StackArray<T> implements Stack<T> {
    private static final int DEFAULT_CAPACITY = 10;
    private T[] array;
    private int top;
    
    @SuppressWarnings("unchecked")
    public StackArray() {
        array = (T[]) new Object[DEFAULT_CAPACITY];
        top = -1;
    }
    
    @Override
    public void push(T item) {
        if (top == array.length - 1) {
            resize();
        }
        array[++top] = item;
    }
    
    @Override
    public T pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        T item = array[top];
        array[top--] = null; // Avoid memory leak
        return item;
    }
    
    @Override
    public T peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return array[top];
    }
    
    @Override
    public boolean isEmpty() {
        return top == -1;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Function Call Stack
**Use Case:** Managing function calls and returns  
**Example:** Every programming language uses a call stack for recursion and function invocations

### 2. Expression Evaluation
**Use Case:** Infix to postfix conversion, calculator applications  
**Example:** Compilers use stacks for parsing and evaluating expressions

### 3. Undo/Redo Functionality
**Use Case:** Document editors, graphic design tools  
**Example:** Photoshop's undo history, VSCode's edit history

### 4. Browser Navigation
**Use Case:** Back button functionality  
**Example:** Browser maintains visited pages stack for backward navigation

### 5. Syntax Parsing
**Use Case:** Bracket matching, XML/HTML validation  
**Example:** IDEs validating balanced parentheses and brackets

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **JVM** | Method call stack, operand stack |
| **Chrome V8** | JavaScript execution stack |
| **Visual Studio** | Syntax highlighting, brace matching |
| **Git** | Stash operations (git stash push/pop) |
| **Photoshop** | History/undo functionality |

---

## ⚖️ Comparison with Related Data Structures

| Feature | Stack | Queue | Deque |
|---------|-------|-------|-------|
| **Order** | LIFO | FIFO | Both ends |
| **Add** | push (top) | enqueue (rear) | Both ends |
| **Remove** | pop (top) | dequeue (front) | Both ends |
| **Use Case** | Backtracking | Scheduling | Sliding window |

### Implementation Trade-offs

| Implementation | Pros | Cons |
|----------------|------|------|
| **Array (fixed)** | Simple, cache-friendly | Fixed size, waste space |
| **Array (dynamic)** | Auto-resize | Occasional O(n) resize |
| **Linked List** | No size limit | Pointer overhead |
| **Java's Stack** | Built-in | Synchronization overhead |
| **ArrayDeque** | Fast, no sync | No random access |

### Java Standard Library

```java
// Legacy (synchronized, extends Vector)
Stack<Integer> stack = new Stack<>();

// Recommended (faster, implements Deque)
Deque<Integer> stack = new ArrayDeque<>();
```

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Stack Underflow:** Popping from empty stack
   - Solution: Always check `isEmpty()` before pop

2. **Stack Overflow:** Exceeding maximum capacity (recursion depth)
   - Solution: Use iterative solutions or increase stack size

3. **Memory Leak (Array):** Not nulling removed elements
   - Solution: Set `array[top] = null` after pop

4. **Using java.util.Stack:** Performance issues due to synchronization
   - Solution: Use `ArrayDeque` instead

### Edge Cases to Handle

- [x] Pop/peek on empty stack
- [x] Push on full stack (fixed-size array)
- [x] Single element stack operations
- [x] Null elements (if allowed by implementation)

---

## 📖 References

1. Knuth, D.E. "The Art of Computer Programming, Vol. 1" (1997)
2. Sedgewick, R. "Algorithms" (4th ed.)
3. [Wikipedia: Stack (abstract data type)](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))

---

## 🔗 Related Data Structures & Algorithms

- [Queue](./queue.md) - FIFO counterpart
- [Deque](./deque.md) - Double-ended queue
- [Call Stack](./call-stack.md) - System-level implementation
- [Balanced Brackets](../../stacks/balanced-brackets.md) - Stack application
- [Infix to Postfix](../../stacks/infix-to-postfix.md) - Expression conversion
