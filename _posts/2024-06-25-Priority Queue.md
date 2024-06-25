---
layout: post
title: "Priority Queue"
date: 2024-06-25
categories: [Algorithms]
tags: [Algorithms, Priority Queue]
---

A Priority Queue is an abstract data type similar to a regular queue or stack data structure, but with an additional property: each element has a priority associated with it. In a priority queue, elements are served based on their priority rather than their order in the queue. Elements with higher priorities are dequeued before elements with lower priorities. If two elements have the same priority, they are served according to their order in the queue.

### Key Concepts

1. **Elements and Priorities**:
    - Each element in a priority queue is associated with a priority value.
    - Elements with higher priority are processed before elements with lower priority.
2. **Operations**:
    - **Insert (enqueue)**: Add an element to the priority queue with an associated priority.
    - **Remove (dequeue)**: Remove and return the element with the highest priority.
    - **Peek**: Return the element with the highest priority without removing it.

### Implementations

Priority queues can be implemented using various data structures, including:

1. **Binary Heap**:
    - A common and efficient implementation of a priority queue.
    - Binary heaps can be either min-heaps or max-heaps.
    - In a min-heap, the element with the smallest priority is always at the root.
    - In a max-heap, the element with the largest priority is always at the root.
    - Insertions and deletions take O(log n) time.
2. **Binary Search Tree (BST)**:
    - Can be used to implement a priority queue.
    - Allows for fast insertions, deletions, and finding the element with the highest priority.
    - However, BSTs are not as efficient as heaps for priority queue operations.
3. **Fibonacci Heap**:
    - A more advanced data structure that provides better amortized time complexity for some operations.
    - Insertions take O(1) amortized time.
    - Deletions and finding the minimum element take O(log n) amortized time.

### Example

Consider a scenario where you have a set of tasks with different priorities, and you want to process the tasks in order of their priority:

```python
import heapq

# Create an empty priority queue
priority_queue = []

# Insert elements into the priority queue with associated priorities
heapq.heappush(priority_queue, (3, "Task 1"))
heapq.heappush(priority_queue, (1, "Task 2"))
heapq.heappush(priority_queue, (2, "Task 3"))

# Remove elements from the priority queue based on priority
while priority_queue:
    priority, task = heapq.heappop(priority_queue)
    print(f"Processing {task} with priority {priority}")

```

Output:

```arduino
Processing Task 2 with priority 1
Processing Task 3 with priority 2
Processing Task 1 with priority 3

```

### Applications

- **Scheduling**: Priority queues are widely used in operating systems for task scheduling, where tasks with higher priorities are executed first.
- **Graph Algorithms**: Used in algorithms like Dijkstra's shortest path and Prim's minimum spanning tree.
- **Event Simulation**: Priority queues are used to manage events in discrete event simulations, where events are processed in the order of their scheduled times.
- **Huffman Coding**: Used in the creation of Huffman trees for data compression.