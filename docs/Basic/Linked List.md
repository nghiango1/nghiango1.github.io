---
share: true
catagory: Basic
tags:
  - computer_sience
---

# 1. Problem

A `array` like structure, but dynamic with actual Insert and Delete operation. Implement Linked List

# 2. Solve

## Linked list

```python
class Node:
    def __init__(self, value, nextNode = None):
        self.value = value
        self.nextNode = nextNode

class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def insert(self, v):
        if self.head == None:
            self.head = Node(v)
            self.tail = self.head
            return
        self.tail.nextNode = Node(v)
        self.tail = self.tail.nextNode

    def delete(self, v):
        if self.head == None:
            return
        # Head delete
        if self.head.value == v:
            tmp = self.head
            self.head = self.head.nextNode
            self.head.prevNode = None
            # del tmp
        # Middle, Tail delete
        # TODO: Traversal to v and delete
```

## Double Linked list

```python
class Node:
    def __init__(self, value, nextNode = None, prevNode = None):
        self.value = value
        self.nextNode = nextNode
        self.prevNode = prevNode

class DoubleLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def insert(self, v):
        if self.head == None:
            self.head = Node(v)
            self.tail = self.head
            return
        self.tail.nextNode = Node(v, prevNode = self.tail)
        self.tail = self.tail.nextNode

    def delete(self, v):
        if self.head == None:
            return
        # Head delete
        if self.head.value == v:
            tmp = self.head
            self.head = self.head.nextNode
            self.head.nextNode
            # del tmp
        # Tail delete
        
        # Middle, 
        # TODO: Traversal to v and delete
```