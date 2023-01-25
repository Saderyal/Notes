# Data Structures and Algorithms

Mastering Data Structures and Algorithms is a really difficult but yet interesting process of being a Developer. Here I will try to explain the main Data structures and algorithms.
But, before going directly into that, let's take a step back and talk about **Big O**.

# Big O

The Big O is a time complexity function for calculating the time a function needs to execute.

## Types

There are actually 6 formulas, or types of Big O notations (ordered from the less to the most expensive):

* **CONSTANT TIME** -> `O(1)`. It's the less expensive. You do only one operation.
* **LOGARITMIC TIME** -> `O(log n)` It's the complexity time for perfect binary trees. It's extremely efficient, because you don't have to look each element.
* **LINEAR TIME** -> `O(n)`. For a for loop. You look every element.
* **N LOG N TIME** -> `O(n log n)`. It's a common time complexity for sorting algorithms.
* **QUADRATIC TIME** -> `O(n^2)`, `O(a*b)`. For nested for loops.
* **BINARY TIME** -> `O(2^n)`.
* **FACTORIAL TIME**, or "oh no!" -> `O(n!)` It's the most expensive. A loop for each item.


## Rules for calculating complexity

1. Worst case: **Keep just the worst case**
2. **Remove constants**: in an interview, it's unsignificant to have `O(1 + n + 100)` or `O(n + 50000000)` or even `O(2n)`. Just keep `O(n)`. It doesn't matter how much the function grows, but what matters is HOW the function moves in the graphic (in case of O(n), it moves linearly).
3. For different parameters, **use different terms**. For example, O(a + b).
4. **Drop non dominant terms**. For example, in a case like `O(n + n^2)`, you would drop the `n`, because `n^2` is the dominant term.


----


# Good code

What is a good code?
A good code is a code that is:
1. Readable
2. Scalable

"Which code is best?" can be answered with these three pillars:
* Readable
* Fast (Speed. Time complexity)
* Memory (Space complexity)

The speed of a program can be calculated with the **Big O**, as we've seen above. But, what about memory?
What causes space complecity, are:

* Variables
* Data structures
* Function call
* Allocations




# How to solve problems

What a company looks for?

1. Analytic Skills
2. Coding Skills
3. Technical Skills
4. Communication Skills

Companies wants **smart people**. People who know how to look for answers. They wanna know that you know data structures and their existance, how, and when, to use them.
You don't have to memorize everything.

#### Data Structures

* Arrays
* Trees
* Stacks
* Tries
* Queues
* Graphs
* Linked Lists
* Hash Tables

#### Algorithms

* Sorting
* Dynamic Programming
* BFS + DFS (searching)
* Recursion

---

# Data structures

Data structures are objects that contains values. Each data structure solve a specific problem and organize data in a certain way.
There are a tone of data structures.
What an engineer must know, is:

1. How to build one
2. How to use it

You have to understand data structures so you can **pick** the perfect data structure.

## How they works

Before getting specific, we must know how a computer store data.

Data are stored in the RAM. On top of that we have the storage (Hard disk, SSD, ecc.).
Persistent storage is slow, so the CPU access the RAM to be faster.
Each data structure is allocated in the RAM in a specific way, and that's why, in a certrain problem, a data structure can be better than another one.

## List of data structures

The main data structures are the following:

* Arrays
* Trees
* Stacks
* Tries
* Queues
* Graphs
* Linked Lists
* Hash Tables

## Languages

Each programming language has it's own data types and data structures. If the language you're using doesn't have a specific data structure, you can always build it on your own.





# Arrays

Arrays, or lists, organize items sequentially (one after another in memory, RAM).
It's the most common used data structure.

## Operations

* lookup -> `O(1)`
* push -> `O(1)`
* insert -> `O(n)`
* delete -> `O(n)`

## Types of arrays

There are actually 2 types of arryas:

* Static
* Dynamic

The **static** array has a fixed size (e.g. only 7, fixed, items).
The **dyanmic** array, can copy and reallocate itself to create a new one with a new size.
JavaScript and other languages (such python) use dynamic arrays that automatically reallocate themselves.
This means that you can't really know if an operation like `push` has a complexity of O(1), beacuse it can also be O(n).


## Implementation

```javascript
class MyArray {

  constructor(){
    this.length = 0
    this.data = {};
  }

  get(index){
    return this.data[index]
  }

  push(item){
    this.data[this.length] = item
    this.length++
    return this.length
  }

  pop(){
    const lastItem = this.data[this.length-1]
    delete this.data[this.length-1]
    this.length--
    return lastItem
  }

  delete(index){
    const item = this.data[index]
    this.shiftItems(index)
  }

  shiftItems(index){
    for(let i=index; i<this.length-1; i++){
      this.data[i] = this.data[i+1]
    }
    delete this.data[this.length-1]
    this.length--
  }

}

const newArray = new MyArray()
newArray.push('hi')
newArray.push('you')
newArray.push('!')
newArray.delete(0)
newArray.push('are')
newArray.push('nice')
console.log(newArray)
```




# Hash Tables

Hash Tables, or Hash maps, Maps, dictionaries, or, again, objects are an absolute MUST.
In an hash tables, we have a key and a corresponding value. The key is used as an index to find a certain value.

## Hash function

A hash function is a function that generate a Hash given an input. It's one-way function (so, given the output, you can't tell what the input was) and for the same input, the output remain the same (This is what we call **indempotent** function).
The generated hash, in hash tables, is used to immediately retriev a value.
Of course, the hash function must be as fast as possible.
So:
```
Key -> Hash function -> RAM
```

## Operations

* Insert -> `O(1)`
* Lookup -> `O(1)`
* Delete -> `O(1)`
* Search -> `O(1)`

## The collision problem

Hash tables have a problem called **collision**. This happens when a key is allocated in the same address space of another one.
Note that the data is not lost, but it slow down the procedure to obtain that value.
```
[address] -> value1 -> value2 -> value3
```
In the above scheme, the same address has multiple values. This gets resolved as a `LINKED LIST`.
When there is a collision, the time complexity function is `O(n/k)`, where k is the size of the hash table. Simplifing things, `O(n)`.

## Hash tables in different languages

Normally, hash tables doesn't know the insertion order, but objects like `Map()`, in JavaScript, do.

## Why hash tables

Hash tables are **perfect** for a quick access of a certain value (that's why they are used in Databases). But, if you want to loop over the keys of the tables, it cost more time then looping through an array. 

PROS:
* Fast lookups (with NO collision)
* Fast inserts
* Felixible keys

CONS:
* Unordered
* Slow key iteration


## Implementation

```javascript
class HashTable {

  constructor(size){
    this.data = new Array(size)
    // structure -> [[key, value], [key, value]]
  }

  // O(1)
  set(key, value){
    const address = this._hash(key)
    if(!this.data[address]){
      this.data[address] = []
    }
    this.data[address].push([key, value])
  }

  // O(1). O(n) if there are collisions
  get(key){
    const address = this._hash(key)
    const currentBucket = this.data[address];
    if(currentBucket){
      for(let i=0; i<currentBucket.length; i++){
        if(currentBucket[i][0] === key)
          return currentBucket[i][1]
      }
    }
    return undefined
  }

  keys(){
    const keysArray = []
    for (let i = 0; i < this.data.length; i++) {
      if (this.data[i] && this.data[i].length) {
        // but also loop through all the potential collisions
        if (this.data.length > 1) {
          for (let j = 0; j < this.data[i].length; j++) {
            keysArray.push(this.data[i][j][0])
          }
        } else {
          keysArray.push(this.data[i][0])
        } 
      }
    }
    return keysArray;
  }

  // O(1) cause it's very very fast
  _hash(key){
    let hash = 0;
    for(let i=0; i<key.length; i++){
      hash = (hash + key.charCodeAt(i) * i) % this.data.length
    }
    return hash;
  }

}

const myHashTable = new HashTable(50) // Allocate 50 slots
myHashTable.set('grapes', 10000)
myHashTable.set('bottlesOfWater', 3)
console.log(myHashTable.get('grapes'))
console.log(myHashTable.get('bottlesOfWater'))
console.log(myHashTable.keys())
```




# Linked Lists

The problem with arrays (at least static arrays), is that we have only a given space where we can allocate data. Linked lists solve this problem using hashed tables, but in an ordered way.

A linked list is a a set of nodes linked to each other. A set has a value of the data, and a **pointer** for the next node. The first node is called *head*, the last one, *tail*. The tail node has a pointer equals to null.

Linked list has a "loose" nature, so that you can add/delete/modify a node without much time complexity (actually, `O(1)`).
In an array, adding an element in the head or in the middle of itself, would cost `O(n)`. So, the linked list solve this kind of problem, and, unlike hash tables, you can have ordered data.

## Operations

* prepend -> `O(1)`
* append -> `O(1)`
* lookup -> `O(n)`
* insert -> `O(n)`
* delete -> `O(n)`

## Pointers

A pointer is simply a reference to a place in memory. 

## Doubly linked list

A doubly linked list is a linked list where each node doesn't point only to the next one, but also to the previous one.
This allows the linked list to be traversed backards, and permits operations like lookup, insert, and delete, to be more efficient (because, based on the index, you can decide to traverse the linked list forewards or backwards). Howevere, complexity time of `O(n/2)` will be anyway `O(n)`.

## Single vs Double

Single linked list use less memory, so are appropriate to use when you have memory problems and want to have fast addition and deletion of elements.
A doubly linked list requires more memory and it's more complex, but it's a little faster and flexible.

## Pros and cons

PROS:
* Fast insertion
* Fast deletion
* ordered
* Flexible size

CONS:
* Slow Lookup
* More memory

## Implementation

```javascript
class Node {

  constructor(value){
    this.value = value
    this.next = null
  }

}

class LinkedList {

  constructor(value){
    this.head = {
      value: value,
      next: null
    }
    this.tail = this.head
    this.length = 1
  }

  append(value){
    const newNode = new Node(value)
    this.tail.next = newNode // this modify the pointer at this.head
    this.tail = newNode
    this.length++
    return this
  }

  prepend(value){
    const newNode = new Node(value)
    newNode.next = this.head
    this.head = newNode
    this.length++;
    return this
  }

  printList(){
    const array = []
    let currentNode = this.head
    while(currentNode !== null){
      array.push(currentNode.value)
      currentNode = currentNode.next
    }
    return array.toString()
  }

  insert(index, value){
    if(index === 0){
      this.prepend(value)
      return this
    }
    if(index >= this.length){
      this.append(value)
      return this;
    }
    const newNode = new Node(value)
    const prevNode = this.traverseToIndex(index-1)
    const nextNode = prevNode.next
    prevNode.next = newNode
    newNode.next = nextNode
    this.length++
    return this
  }

  traverseToIndex(index){
    let counter = 0
    let currentNode = this.head
    while(counter !== index){
      currentNode = currentNode.next
      counter++;
    }
    return currentNode
  }

  remove(index){
    if(index == 0){
      this.head = this.head.next
      return this
    }
    if(index >= this.length)
      return this;
    const prevNode = this.traverseToIndex(index-1)
    const toDelete = prevNode.next
    prevNode.next = toDelete.next
    this.length--
    return this
  }

  reverse(){
    if(!this.head.next)
      return this.head
    let first = this.head
    this.tail = this.head
    let second = first.next
    while(second){
      const temp = second.next
      second.next = first
      first = second
      second = temp
    }
    this.head.next = null // tail
    this.head = first
  }

}

// 16 -> 5 -> 10 -> 99 -> 1

// f=99
// s=10

const myLinkedList = new LinkedList(10)
myLinkedList.append(5)
myLinkedList.append(16)
myLinkedList.prepend(1)
myLinkedList.insert(1, 99)
console.log(myLinkedList.printList())
myLinkedList.remove(4)
console.log(myLinkedList.printList())
myLinkedList.reverse()
console.log(myLinkedList.printList())
```

## Implementation (doubly linked list)

```javascript
class DoubleNode {

  constructor(value){
    this.value = value
    this.next = null
    this.prev = null
  }

}

class DoublyLinkedList {

  constructor(value){
    this.head = new DoubleNode(value)
    this.tail = this.head
    this.length = 1
  }

  append(value){
    const newNode = new DoubleNode(value)
    newNode.prev = this.tail;
    this.tail.next = newNode // this modify the pointer at this.head
    this.tail = newNode
    this.length++
    return this
  }

  prepend(value){
    const newNode = new DoubleNode(value)
    newNode.next = this.head
    this.head.prev = newNode
    this.head = newNode
    this.length++;
    return this
  }

  printList(){
    const array = []
    let currentNode = this.head
    while(currentNode !== null){
      array.push(currentNode.value)
      currentNode = currentNode.next
    }
    return array.toString()
  }

  printListReverse(){
    const array = []
    let currentNode = this.tail
    while(currentNode !== null){
      array.push(currentNode.value)
      currentNode = currentNode.prev
    }
    return array.toString()
  }

  insert(index, value){
    if(index === 0){
      this.prepend(value)
      return this
    }
    if(index >= this.length){
      this.append(value)
      return this;
    }
    const newNode = new DoubleNode(value)
    const prevNode = this.traverseToIndex(index-1)
    const nextNode = prevNode.next
    prevNode.next = newNode
    newNode.prev = prevNode
    newNode.next = nextNode
    nextNode.prev = newNode
    this.length++
    return this
  }

  traverseToIndex(index){
    let counter = 0
    let currentNode = this.head
    while(counter !== index){
      currentNode = currentNode.next
      counter++;
    }
    return currentNode
  }

  traverseToIndexReverse(index){
    let counter = this.length
    let currentNode = this.tail
    while(counter !== 0){
      currentNode = currentNode.prev
      counter--;
    }
    return currentNode
  }

  remove(index){
    if(index == 0){
      this.head = this.head.next
      this.head.prev = null
      return this
    }
    if(index >= this.length)
      return this;
    const prevNode = this.traverseToIndex(index-1)
    const toDelete = prevNode.next
    prevNode.next = toDelete.next
    if(!toDelete.next)
      this.tail = prevNode
    else
      toDelete.next.prev = prevNode
    this.length--
    return this
  }

}

const myLinkedList = new DoublyLinkedList(10)
myLinkedList.append(5)
myLinkedList.append(16)
myLinkedList.prepend(1)
myLinkedList.insert(1, 99)
console.log(myLinkedList.printList())
myLinkedList.remove(0)
console.log(myLinkedList.printList())
console.log(myLinkedList.printListReverse())
```




# Stacks and Queues

Stacks and Queues are very similira, and are what we call **linear data structures** (allows us to traverse the data sequentially: one by one).
The reason that they are very similar, is that they are implemented almost the same way.
Unlike an array, you don't use this data structures to access random elements: you have to perform operations only in the head or tail of the structure (such as push, pop, unshift, ecc). So, we have less methods, so that you can **limit** the numbers of operations and so have more **control** over the data.

## Stacks

Stacks do operations in a **LIFO** order. Last In, First Out (imagine a pile of plates, or the browser history).
The implementation of a stack can be done with arrays or linked lists (it's likely indifferent).

### Operations

* Lookup -> O(n)
* pop -> O(1)
* push -> O(1)
* peek -> O(1)

## Queues

Queues do operations in a **FIFO** order. First In, First Out. It's the opposite of stacks.
The implementation of a queue should be done only with linked lists, because arrays slow down the procedure to enqueue and dequeue an item (unshift indexes).

### Operations

* Lookup -> O(n)
* enqueue -> O(1)
* dequeue -> O(1)
* peek -> O(1)

## Pros and cons

PROS:
* Fast operations
* Fast peek
* ordered

CONS:
* Slow Lookup

## Implementation (stacks)

```javascript
class Node {
  constructor(value){
    this.value = value
    this.next = null
  }
}

class Stack {

  constructor(){
    this.top = null
    this.bottom = null
    this.length = 0
  }

  peek(){
    console.log(this.top)
    return this.top
  }

  push(value){
    const newNode = new Node(value)
    if(this.length==0){
      this.top = newNode
      this.bottom = newNode
    } else {
      const oldTop = this.top
      this.top = newNode
      this.top.next = oldTop
    }
    this.length++
    return this
  }

  pop(){
    if(!this.top)
      return null
    if(this.top === this.bottom)
      this.bottom = null
    const oldTop = this.top
    this.top = this.top.next
    this.length--
    return this
  }

  toString(){
    const items = []
    let currentItem = this.top
    while(currentItem){
      items.push(currentItem.value)
      currentItem = currentItem.next
    }
    const stringyfied = items.toString()
    console.log(stringyfied, `--- Items: ${this.length}`)
    return stringyfied
  }

}

const myStack = new Stack()
myStack.push('Google')
myStack.push('Udemy')
myStack.push('Discord')
myStack.toString()
myStack.pop()
myStack.toString()
myStack.pop()
myStack.toString()
myStack.pop()
myStack.toString()
myStack.push('Google')
myStack.toString()
myStack.peek()


console.log('===============')



class ArrayStack {

  constructor(){
    this.data = []
  }

  peek(){
    return this.data[this.data.length-1]
  }

  push(value){
    this.data.push(value)
  }

  pop(){
    this.data.pop()
  }

  toString(){
    console.log(this.data.toString())
  }

}
const myArrayStack = new ArrayStack()
myArrayStack.push('Google')
myArrayStack.push('Udemy')
myArrayStack.push('Discord')
myArrayStack.toString()
myArrayStack.pop()
myArrayStack.toString()
myArrayStack.pop()
myArrayStack.toString()
myArrayStack.pop()
myArrayStack.toString()
myArrayStack.push('Google')
myArrayStack.toString()
myArrayStack.peek()
```

## Implementation (queues)

```javascript
class Node {

  constructor(value){
    this.value = value
    this.next = null
  }

}

class Queue {

  constructor(){
    this.first = null
    this.last = null
    this.length = 0
  }

  peek(){
    return this.first
  }

  enqueue(value){
    const newNode = new Node(value)
    if(!this.length){
      this.first = newNode
      this.last = this.first
    } else {
      this.last.next = newNode
      this.last = newNode
    }
    this.length++
    return this.toString()
  }

  dequeue(){
    if(!this.first)
      return this.toString()
    if(this.first === this.last)
      this.last = null
    this.first = this.first.next
    this.length--
    return this.toString()
  }

  toString(){
    const values = []
    let currentNode = this.first
    while(currentNode){
      values.push(currentNode.value)
      currentNode = currentNode.next
    }
    console.log(values.toString())
    return values.toString()
  }

}

const myQueue = new Queue()
myQueue.enqueue('Ilaria')
myQueue.enqueue('Veronica')
myQueue.dequeue()
myQueue.dequeue()
myQueue.enqueue('Grace')
myQueue.enqueue('Veronica')
myQueue.enqueue('Tongi')
myQueue.enqueue('Patongi')
myQueue.dequeue()
myQueue.dequeue()
myQueue.dequeue()
console.log(myQueue.peek())
myQueue.dequeue()
```




# Trees

Trees are a data structure that is hierarchical (unlike linked lists ecc, that are linear), so a node can go in multiple directions.
A tree is composed with a root node, which has children. Each children node has a parent and eventually other sub-children.
A *"Leaf"* is a group of sub children of a specific node.
*"Siblings"*, are nodes in the same level of another one.
So, the elements of a tree are:

* A **Root**
* A **Parent node**
* A **Child node**
* A **Leaf**
* **Siblings**

Actaully, linked lists are a type of tree that have only a child per node.

## Binary trees

In bynary trees, each node has a maximum of two children. So, each node will have only a left and right property, and not an array of children.

There are two types of binary Trees:

* **Perfect binary tree** - Each node have two childre, and they all end in the same level (all the leafs node are full).
* **Full binary tree** - A node has either 0 or 2 children, but never 1.

The first one it's called "*perfect*" because is really really efficient, so it's desirable.
They have 2 interesting properties:
1. The number of total nodes in each level double as we move down the tree.
2. The number of nodes on the last level is equal to the sum of the number of nodes of all the other levels plus one.

### Perfect binary trees

The time complexity for a perfect binary tree, is O(log n).
What deos it means?
It means that for each, level, the time complexity is the number of the level to the power of 2.
```
Level 0: 2^0 = 1
Level 1: 2^1 = 2
Level 2: 2^2 = 4
Level 3: 2^3 = 8
```
So, if we want to know the number of nodes that a perfect binary tree has, the formula would be:
```
NumberOfNodes = 2^h - 1
```
Where `h` is the height of the binary tree.

#### Operations

* Lookup -> `O(log N)`
* Insert -> `O(log N)`
* delete -> `O(log N)`

## Binary Search Trees

As the name suggests, binary trees are particulary good for searching: they're great for comparing things.
This data structures preserve relationships.
When it comes to binary search trees, these are the rules:

1. All child nodes in the tree to the right of the root node must be greater than the current node (keep going on the right, the numbers/values of nodes constantly increase).
So, going on the left, the numbers/values decrease.
2. A node can only have up to two children (beacuse it's a binary tree)

This makes seaching for a specific value a very efficient operation.

Binary seach trees can have a problem: they can be **unbalanced**. This means that, eventually, a side of the tree has a lot more levels on one side rather than the other (for example, if we add constantly numbers that are greater than the previous one, the tree starts to be unbalanced on the right). In this case, the time complexity function turns eventually to a `O(n)`, because we have to look for *every* item to perform an operation.
So, in the best case, we have all `O(log N)` operations. In the worst, `O(n)`.
Fortunately, there are methods to balance a binary search trees, but can be complicated and time consuming.

### Pros and Cons of BNS

Pros:
* Better than `O(n)`
* Ordered
* Flexible size

Cons:
* No `O(1)` operations

## Balanced Tree

In production you should use a balanced BST (for performance), so, usually, you're gonna come up with an AVL Tree or Red Black Tree, which have algorithms that automatically balance the tree

## Binary Heaps

Binary Heaps can be of two types:

* Max Hepas
* Min Heaps

In Max Heaps, every child node belong to a node that has a greater value (unlike BST), so the root it's actually the maximum value.

In Min Heaps it's exactly the opposite, so the root it's the minimun value, and the child nodes are greater that the parent node.

Heaps are less ordered, so operations like lookup are slower than Binary Search Tree, but, Binary Heaps are really really great at doing comparative operations.

Binary Heaps preserve the order of insertion and you can actually implements them with arrays.

The order of insertions in Binary Heaps Trees is left to right.

One thing where Binary Heaps are really good at, are **Priority Queues**

#### Priority Queues

Priority queues are another strucutre where each element has a priority, so the higher the priority of an element, the higher will be the position that the element has in the queue (you can think about an emergency room).

**IMPLEMENTATION**:

```javascript
class PriorityQueue {

  constructor(comparator = (a,b) => a > b){
    this._heap = [];
    this._comparator = comparator;
  }

  size(){
    return this._heap.length;
  }

  isEmpty(){
    return this.size() === 0;
  }

  peek(){
    return this._heap[0];
  }

  push(value){
    this._heap.push(value);
    this._siftUp();
    return this.size();
  }

  pop(){
    if(this.size() > 1)
      this._swap(0, this.size() - 1);
    const poppedValue = this._heap.pop();
    this._siftDown();
    return poppedValue;
  }

  _parent(idx){
    return Math.floor((idx-1)/1);
  }

  _leftChild(idx){
    return idx * 2 + 1;
  }

  _rightChild(idx){
    return idx * 2 + 2
  }

  _swap(i, j){
    const temp = this._heap[i];
    this._heap[i] = this._heap[j];
    this._heap[j] = temp;
  }

  _compare(i, j){
    return this._comparator(this._heap[i], this._heap[j]);
  }

  _siftUp(){
    let nodeIdx = this.size()-1;
    while(nodeIdx > 0 && this._compare(nodeIdx, this._parent(nodeIdx))){
      this._swap(nodeIdx, this._parent(nodeIdx));
      nodeIdx = this._parent(nodeIdx);
    }
  }

  _siftDown(){
    let nodeIdx = 0;
    while((this._leftChild(nodeIdx) < this.size() && this._compare(this._leftChild(nodeIdx), nodeIdx)) || (this._rightChild(nodeIdx) < this.size() && this._compare(this._rightChild(nodeIdx), nodeIdx))) {
      const greaterNodeIdx = this._rightChild(nodeIdx) < this.size() && this._compare(this._rightChild(nodeIdx), this._leftChild(nodeIdx)) ? this._rightChild(nodeIdx) : this._leftChild(nodeIdx);
      this._swap(greaterNodeIdx, nodeIdx);
      nodeIdx = greaterNodeIdx;
    }
  }

}
```

### Operations

* Lookup -> `O(n)`
* Insert -> `O(log N)` (but best case `O(1)`, unlike BST)
* delete -> `O(log N)` (but best case `O(1)`, unlike BST)

### Pros and Cons of Binary Heaps

Pros:
* Better than `O(n)`
* Priority
* Flexible size
* Fast insert

Cons:
* Slow lookup

## Trie

Is a specialized tree used in searching (most often in texts). In most cases it can outperform BST, Hash tables, and other structures.
It's not binary (it can have multiple children).
For example, a node with a value of 'N' will have as children all the possible words that stats with that letter (e.g. 'News' and 'Not')
Visually:
```
     N
   /   \
  E     O
  |     |
  W     T
  |
  S
```
The root node of a Tire is usually empty.

What can be good for?
As said, in text searching it outperforms almost every data structure, so, common uses cases are: browser autocomplete, get words in a dictionary, ecc 
So, to find a word, the Big O will be `O(length of the word)`

## Implementation (BST)

```javascript
class Node {

  constructor(value){
    this.left = null
    this.right = null
    this.value = value
  }

}

class BinarySearchTree {

  constructor(){
    this.root = null
  }

  insert(value){
    const newNode = new Node(value)
    if(!this.root){
      this.root = newNode
    } else {
      let currentNode = this.root
      while(true){
        if(value < currentNode.value){
          if(!currentNode.left){
            currentNode.left = newNode
            return this
          }
          currentNode = currentNode.left
        } else {
          if(!currentNode.right){
            currentNode.right = newNode
            return this
          }
          currentNode = currentNode.right
        }
      }
    }
  }

  lookup(value){
    let currentNode = this.root
    while(currentNode){
      if(value === currentNode.value)
        return true
      if(value < currentNode.value)
        currentNode = currentNode.left
      else
        currentNode = currentNode.right
    }
    return false
  }

  remove(value){
    if(!this.root)
      return false;
    let parentNode = null
    let currentNode = this.root
    while(currentNode){
      if(value === currentNode.value){
        if(parentNode){
          let rightNode = currentNode.right
          if(rightNode){
            let subTreeMinNode = rightNode.left
            if(subTreeMinNode){
              while(true){
                if(!subTreeMinNode.left && !subTreeMinNode.right){
                  subTreeMinNode.left = currentNode.left
                  subTreeMinNode.right = currentNode.right
                  if(value < parentNode.value)
                    parentNode.left = subTreeMinNode
                  else
                    parentNode.right = subTreeMinNode
                  return this
                }
                if(subTreeMinNode.left)
                  subTreeMinNode = subTreeMinNode.left
                else
                  subTreeMinNode = subTreeMinNode.right
              }
            } else {
              rightNode.left = currentNode.left
              if(value < parentNode.value)
                parentNode.left = rightNode
              else
                parentNode.right = rightNode
              return this
            }
          } else {
            if(value < parentNode.value)
              parentNode.left = currentNode.left
            else
              parentNode.right = currentNode.left
            return this
          }
        } else {
          this.root = null
        }
        return this
      }
      parentNode = currentNode
      if(value < currentNode.value)
        currentNode = currentNode.left
      else
        currentNode = currentNode.right
    }
    return this
  }

}

const tree = new BinarySearchTree()
tree.insert(9)
tree.insert(4)
tree.insert(6)
tree.insert(20)
tree.insert(170)
tree.insert(15)
tree.insert(1)
tree.insert(160)
tree.insert(150)
tree.insert(155)
console.log(tree.lookup(170))
tree.remove(20)
console.log(tree)
/* 
        9
    4      20   
  1   6  15   170
            160
          150
            155
*/
```




# Graphs

Graphs are one of the most useful and used data stractures in computer science when it comes in modelling real life.
Graphs are simply a set of values that are related in a pair-wise fashion.
Graphs are composed by **nodes** (vertex) connected to each other with **edges**.
They can rapresent, for example, connections between people, links in the network, roads, and so on.
There are many characteristics to define a Graphs.

## Types of graphs

First of all, they can be **Directed** or **undirected**. Directed (the edges have a direction where they point to) are useful for describing, for example, traffic flow, or the followers of a social network. Undirected are used simply to describe connections between elements.

Graphs can also be **Weighted** and **Unweighted**. Weighted graphs are graphs where not only the Node has a value, but also the edges. So, for example, if you have to find the shortest path (or most efficient path), you have to go through nodes based on the weight of the edges.

Another way to describe a Graph is **Cyclic** or **Acyclic**. When you can go to a node, and another, and another, and then go back to the original node, it's called cyclic graph. Cyclic graphs are usually related to weighted graphs.

Summarizing, there are 3 properties of Graphs:
* **Directed** or **undirected**
* **Weighted** or **Unweighted**
* **Cyclic** or **Acyclic**

## Graph data

There are many ways to work with the data of a Graph:
* With an **Edge list**. With this, we can store in an array the connections between a node an another one. In other words, we can store the **edges**. For examples:
  ```javascript
  const graph = [[0,2], [2,3], [2,1], [1,3]]
  ```
* With an **Adjacent List**. In this method, we use the index (position) of the element to identify the node, and we save the nodes that the node in that position is connected to (Rather then be a list of lists, it can also be an object where the keys are the indexes, and the values are the lists of nodes that the node at the given index is related to). In the next example, the node with the value `0` is connected to the node `2`, the node with the value `1` is connected to the nodes `2` and `3`, and so on...
  ```javascript
  const graph = [[2], [2,3], [0,1,3], [1,2]]
  ```
* With an **Adjacent Matrix**, you can store a matrix of zeros and ones (0-1) that represent rather or not the node in a given index of the first dimension of the matrix has connection with the given index of the second dimension of the matrix (even here, it can also be an object where the keys are the indexes and the values are the array of connections). For example:
  ```javascript
  const graph = [
    [0, 0, 1, 0],
    [0, 0, 1, 1],
    [1, 1, 0, 1],
    [0, 1, 1, 0],
  ]
  ```

## Pros and cons

Pros:
* Relationships

Cons:
* Scaling is hard


## Implementation

```javascript
class Graph {

  constructor(){
    this.numberOfNodes = 0
    this.adjacentList = {}
  }

  addVertex(node){
    this.adjacentList[node] = []
    this.numberOfNodes++
  }

  addEdge(node1, node2){
    if(this.adjacentList[node1] == undefined || this.adjacentList[node2] == undefined)
      return
    this.adjacentList[node1].push(node2)
    this.adjacentList[node2].push(node1)
  }

  showConnections(){
    Object.keys(this.adjacentList).forEach((node) => {
      console.log(`${node} --> ${this.adjacentList[node].join(' ')}`)
    })
  }

}

const myGraph = new Graph()
myGraph.addVertex('0')
myGraph.addVertex('1')
myGraph.addVertex('2')
myGraph.addVertex('3')
myGraph.addVertex('4')
myGraph.addVertex('5')
myGraph.addVertex('6')
myGraph.addEdge('3', '1')
myGraph.addEdge('3', '4')
myGraph.addEdge('4', '2')
myGraph.addEdge('4', '5')
myGraph.addEdge('1', '2')
myGraph.addEdge('1', '0')
myGraph.addEdge('0', '2')
myGraph.addEdge('6', '5')

myGraph.showConnections()
```

------

# Algorithms

Man, a big section about algorithms is coming...

# Recursion

Recursion is the procedure of a function calling itslef (in a controlled way).
Recursion is really good for tasks that have repeated sub-tasks to do.
One down side of recursion, is that the program must allocate memory in the call stack to know the order of the functions to call.
Another possible down side, is that recursion is possibly **infinite**. This happens when the function continue calling itself without ever exiting, causing a **stack overflow** error. For this reason, a recursive function **must** have a "base case", or a point where the function ends the sub-tasks and have to return something.

The steps to follow are:

1. Identify the base case
2. Identify the recursive case
3. Get closer and closer and return when needed. Usually you have 2 returns (one that returns the result of recursion, and one that returns the result of the base case)

## When to use recursion

Every time you are using a tree or converting something into a tree, consider recursion.

1. Divided into a number of subproblems that are smaller instances of the same problem.
2. Each instance of the subproblem is identical in nature.
3. The solutions of each subproblem can be combined to solve the problem at hand

Divide and Conquer using Recursion.


## Pros and cons

Pros:
* DRY (don't repeat yourself)
* Readability

Cons:
* Large Stack

## Example (Fibonacci)

```javascript
function findFactorialRecursive(number){
  if(number <= 1)
    return 1
  return number * findFactorialRecursive(number-1)
}

console.log(findFactorialRecursive(5))


function fibonacciRecursive(n){
  if(n < 2)
    return n
  return fibonacciRecursive(n-1) + fibonacciRecursive(n-2)
}

function reverseString(string){
  if(string.length <= 1)
    return string
  return string[string.length-1] + reverseString(string.substr(0,string.length-1))
}

console.log(reverseString('yoyo mastery'))
```




# Sorting

In production, usually you don't use bult-in methods for sorting a group of data, beacuse you'll surely have custom-data to work on.
For this reason, it's important to know at least the most common algorithms for sorting.

There are 3 very simple, basic, sorting algorithms, which are: **Bubble Sort**, **Insertionn Sort**, **Selection Sort**.
There are also the **Merge Sort** and the **Quick sort**, but they are more complex.


## List of main Sorting algorithms

Here the list of the main sorting algorithms with the associated best case complexity:

* Bubble Sort -> `O(n)`
* Selection Sort - `O(n^2)`
* Insertion Sort -> `O(n)`
* Merge Sort -> `O(n log n)`
* Quick sort -> `O(n log n)`
* Heap Sort -- *NOT IMPLEMENTED*
* Radix Sort -- *NON IMPLEMENTED*
* Counting Sort -- *NON IMPLEMENTED*

### Bubble Sort

Bubble sort comes from the idea of "bubbling up" items. For example, let's order the following sequence in ascending order:
```
2 1 5 7 6 3 8 9 4
```
Shortly, you look of the first 2 items, and you swap them if the first one is greater, then you move on by one, and you swap the two item if the first one is grater than the second one, the you move on by one, and so on... Arrived at the end of the sequence, you will do it again from the start, and repeat this procedure untile there is nothing to swap any longer.
It's one of the simplest sorting algorithms, but it's one of the most inefficient.

### Selection Sort

It's, again, one of the most simple algorithms. It works like that: let's pick again the example used in bubble sort:
```
2 1 5 7 6 3 8 9 4
```
Selection sort would work like that: it will scan the whole array to find the smallest value, then it will place this smallest value in the first position of the array. Then, starting from the second index, it will scan again, and place the found smallest value on the second position of the sequence, and so on...
Again, it's not really efficient (even worst then bubble sort).

### Insertion Sort

Insertion sort it's not the most efficient algorithm, but there are cases where it really is. In detail, it's really efficient when the sequence it's "almost sorted".
```
6 5 3 1 8 7 2 4
```
Insertion sorting works like that: It will scan the sequnce index by index, and order the sub, sequence that it create. What it means, is that, starting from 6, it will compare the 6 with the next value, which is 5. 5 il less than 6, so it will go first. Then we add 3, that is less than 6 and less than 5, so we put it first, and so on.

### Merge Sort

Merge Sort it's a pretty good sorting algorithm, bacause of it's time complexity of `O(n log n)`. It's a *"Divide & Conquer"* algorithm. The concept of merge sort, is to split in half the sequence, then split it again and again until we reach a "1" element list. Here, we compare the first two items of each sub-list, we order the sub-lists, creating a 2-elements sized ordered array. Then, we combine each 2 sized list to get a 4 elements sized orderd list, and so on, until we reach the full size array.
In other words, it works like a reverse tree (that's why we have a `O(n log n)` time complexity).

### Quick Sort

Quick sort it's another *"Divide & Conquer"* algorithm. Even this has a time complexity function of `O(n log n)`.
The concept behind quick sort, it's to have a "pivot". Basically, you choose a random number of the sequence, then compare it to all its left-siblings. If the compared number is greater, you put it on the right of the random number, otherwise it stays where it it. For example:
```
3 7 8 5 2 1 9 5 [4] -> (4 is the chosen random number)
```
The first result of the algorithm will be:
```
3 1 2 4 5 8 9 5 7
```
Beacuse you put all the values grater than 4 on it's right side.
Here, you know than that the 4 is in the right position. You can so split the array on the right, and split the array on the left, and apply the same procedure until you have a full sorted array.

## Which is the best?

Insertion Sort should be used if the dataset is small, or if the data are nearly sorted.
Bubble sort? You're kinda nerver gonna use it...
Also Selection Sort.
Merge Sort, it's really really good because of Divide & Conquer, and should be used if you wonder of worst case scenario (that is also `O(n log n)`), but it's expensive in terms of space.
While Quick Sort is really efficient and use optimal space, so it's commonly used, but to avoid the worst case you should pick a good pivot.

## Comparison Sort vs Non-Comparison Sort

All the algorithms we've seen until now are comparison algorithms. This means that to sort a sequence of values, you have to *compare* values to each other.
In comparison Sort, it's mathematically impossible to have a time complexity of better than `O(n log n)` (in the worst case).
But, there are also **Non-Comparison sorts**.
This means that you can sort values without compare them to each other. This makes everything extremely efficient, but also really complex.
Non-comparison sorting algorithms are:
* Counting sort
* Radix Sort
The downside of non-comparison algorithms, is that **they only works with numbers**.

## Bubble sort Implementation

```javascript
const sequence = [2, 1, 5, 7, 6, 3, 8, 9, 4]

function bubbleSort(arr){
  let sorted = false
  while(!sorted){
    sorted = true
    for(let i=0; i<arr.length-1; i++){
      if(arr[i] > arr[i+1]){
        // Swap values
        const temp = arr[i]
        arr[i] = arr[i+1]
        arr[i+1] = temp
        sorted = false
      }
    }
  }
  return arr
}

console.log(bubbleSort(sequence))
```

## Insertion Sort Implementation

```javascript
const sequence = [6, 5, 3, 1, 8, 7, 2, 4]

function insertionSort(arr){
  const length = arr.length
  for(let i=0; i<length; i++){
    if(arr[i] < arr[0]){
      arr.unshift(arr.splice(i,1)[0])
    } else {
      for(let j=1; j<i; j++){
        if(arr[i] > arr[j-1] && arr[i] < arr[j])
          arr.splice(j, 0, arr.splice(i,1)[0])
      }
    }
  }
  return arr
}

console.log(insertionSort(sequence))
```

## Merge Sort Implementation

```javascript
const sequence = [2, 1, 5, 7, 6, 3, 8, 9, 4]

function mergeSort (array) {
  if (array.length === 1) {
    return array
  }
  // Split Array in into right and left
  const length = array.length;
  const middle = Math.floor(length / 2)
  const left = array.slice(0, middle) 
  const right = array.slice(middle)
  // console.log('left:', left);
  // console.log('right:', right);

  
  return merge(
    mergeSort(left),
    mergeSort(right)
  )
}

function merge(left, right){
  const result = [];
  let leftIndex = 0;
  let rightIndex = 0;
  while(leftIndex < left.length && 
        rightIndex < right.length){
     if(left[leftIndex] < right[rightIndex]){
       result.push(left[leftIndex]);
       leftIndex++;
     } else{
       result.push(right[rightIndex]);
       rightIndex++
    }
  }  
  // console.log(left, right)
  return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
}

const answer = mergeSort(sequence);
console.log(answer);
```

## Quick Sort Implementation

```javascript
const sequence = [3, 7, 8, 5, 2, 1, 9, 5, 6, 4]

function quickSort(arr){
  const left = []
  const right = []
  if(arr.length <= 1)
    return arr
  const pivot = arr[arr.length-1]
  for(let i=0; i<arr.length-1; i++){
    if(arr[i] < pivot)
      left.push(arr[i])
    else
      right.push(arr[i])
  }
  return quickSort(left).concat([pivot]).concat(quickSort(right))
}

console.log(quickSort(sequence))
```

## Selection Sort Implementation

```javascript
const sequence = [2, 1, 5, 7, 6, 3, 8, 9, 4]

function selectionSort(arr){
  let smallestIndex = 0
  for(let i=0; i<arr.length; i++){
    smallestIndex = i
    for(let j=i+1; j<arr.length; j++){
      if(arr[j] < arr[smallestIndex])
        smallestIndex = j
    }
    const smallestValue = arr[smallestIndex]
    arr[smallestIndex] = arr[i]
    arr[i] = smallestValue
  }
  return arr
}

console.log(selectionSort(sequence))
```




# Searching algorithms

Searching is something we do a lot in all types of data. We search something almost everyday, and there is a something available to search in every data. For these reasons, searching is a really important topic.

## Types of searching

* Linear Search -> `O(n)`
* Binary Search -> `O(log n)`
* Depth First Search -> `O(n)`
* Breadth First Search -> `O(n)`

### Searching items in arrays

The first two types of searching (Linear and Binary searching) are used for arrays.

#### Linear Search

In computer science, Linear Search is the process of finding a value within a list. It sequentially checks each item of the list until it finds the correct value. It's simply a loop. In the worst case, the item to find is the last item, so it will have a time complexity of `O(n)`.

#### Binary Search

Always in lists, Binary search is a method that works for ordered arrays, that consists of dividing the array each time to find the correct values. For example:

```
1 4 6 9 12 34 45
```

In the above example, we pick the middle value (9). If the value is greater then the value we want, we go to the left and we repeat the same procedure until we find the correct value, otherwise we go to the right (and do the same).
If you think about it, you're really creating a binary search tree. This means that you'll have a time complexity of `O(log n)`

### Graph & Tree traversal

If, for eaxmple, we want to add the `color` property in each node of (for example), a tree. To to date you _have_ to visit every node, causing it to have a linear time complexity. But, especially in tree and Graphs, how do you do that?
You can traverse a tree and a graph with algorithms like Depth First Searhc (DFS) and Breadth First Search (BFS).

#### Breadth First Search

In Breadth First Search you simply move in the tree from the root node and then left to right of each node. So:
```
Root -> Left node -> Right node -> Left of left ... -> Right of right... ...
```
In other words, you _scan each level_. This use more memory, because you have to store each child node to look in the array of children.
In a Graph, it essentially converts it in a Tree.

#### Depth First Seach

In Depth First search, you move down on the first child node until the node doesn't have any children (you go as deep as possible), then you move up and scan the other children of the parent, and so on. This means that you scan the tree from the bottom to the top.
This algorithm use less memory than BFS, but it's a litte more complex.
There are three ways of visiting a tree using DFS:
* **Inorder** - From bottom left, to top, to bottom right (in Binary Search three, the result will be an ordered array)
* **Preorder** - From top, to bottom left, to right. Start from the parent and then go left and right. Really useful if you wanna re-create the tree (it will be ordered by "children").
* **Postorder** - From bottom left, to bottom right, to top.

#### BFS vs DFS

Both algorithm are used to travers a tree/graph, and they have the same efficiency, so, when to use one over the other?
What matters is the order of the data.

##### Pros and cons of BFS

PROS:
* Really Good at finding shortest path (especially in graphs)
* Closer Nodes, so if you know that the position of an element will be likely in the "upper" tree, BFS is better.

CONS:
* More memory

##### Pros and cons of DFS

PROS:
* Less memory
* Opposite of BFS, so if you know that the position of an element will be likely in the "lower" level of the tree, DFS is better. 
* Does Path Exists? Perfect to know if a path *exists*

CONS:
* Can get slow

## Dijkstra and Bellman Algorithms

Dijkstra and Bellman algorithms are used, like BFS, to find the shortest path between two nodes.
What's the difference between BFS and this two algorithms?
Dijkstra and Bellman algorithms are used especially for Graphs, and they consider also the weight that an edge has in a path. This means that the path will be calculated not based on the number of nodes, but based on the sum of the edges that you have to cross to arrive in another node.

When to use one versus the other?
Bellman's algorithm takes care also of negative numbers as weights on the edges, but it's a little more complicated (`O(n^2)` on worst case), while Dijkstra's algorithm is more efficient.


## Implementation (BFS-DFS)

```javascript
class Node {

  constructor(value){
    this.left = null
    this.right = null
    this.value = value
  }

}

class BinarySearchTree {

  constructor(){
    this.root = null
  }

  insert(value){
    const newNode = new Node(value)
    if(!this.root){
      this.root = newNode
    } else {
      let currentNode = this.root
      while(true){
        if(value < currentNode.value){
          if(!currentNode.left){
            currentNode.left = newNode
            return this
          }
          currentNode = currentNode.left
        } else {
          if(!currentNode.right){
            currentNode.right = newNode
            return this
          }
          currentNode = currentNode.right
        }
      }
    }
  }

  lookup(value){
    let currentNode = this.root
    while(currentNode){
      if(value === currentNode.value)
        return true
      if(value < currentNode.value)
        currentNode = currentNode.left
      else
        currentNode = currentNode.right
    }
    return false
  }

  remove(value){
    if(!this.root)
      return false;
    let parentNode = null
    let currentNode = this.root
    while(currentNode){
      if(value === currentNode.value){
        if(parentNode){
          let rightNode = currentNode.right
          if(rightNode){
            let subTreeMinNode = rightNode.left
            if(subTreeMinNode){
              while(true){
                if(!subTreeMinNode.left && !subTreeMinNode.right){
                  subTreeMinNode.left = currentNode.left
                  subTreeMinNode.right = currentNode.right
                  if(value < parentNode.value)
                    parentNode.left = subTreeMinNode
                  else
                    parentNode.right = subTreeMinNode
                  return this
                }
                if(subTreeMinNode.left)
                  subTreeMinNode = subTreeMinNode.left
                else
                  subTreeMinNode = subTreeMinNode.right
              }
            } else {
              rightNode.left = currentNode.left
              if(value < parentNode.value)
                parentNode.left = rightNode
              else
                parentNode.right = rightNode
              return this
            }
          } else {
            if(value < parentNode.value)
              parentNode.left = currentNode.left
            else
              parentNode.right = currentNode.left
            return this
          }
        } else {
          this.root = null
        }
        return this
      }
      parentNode = currentNode
      if(value < currentNode.value)
        currentNode = currentNode.left
      else
        currentNode = currentNode.right
    }
    return this
  }

  breadthFirstSearch(){
    let currentNode = this.root
    let list = []
    let queue = []
    queue.push(currentNode)
    while(queue.length > 0){
      currentNode = queue.shift()
      list.push(currentNode.value)
      if(currentNode.left)
        queue.push(currentNode.left)
      if(currentNode.right)
        queue.push(currentNode.right)
    }
    console.log(list)
    return list
  }

  breadthFirstSearchRecursive(queue, list){
    if(!queue.length)
      return list
    let currentNode = queue.shift()
    list.push(currentNode.value)
    if(currentNode.left)
      queue.push(currentNode.left)
    if(currentNode.right)
      queue.push(currentNode.right)
    return this.breadthFirstSearchRecursive(queue, list)
  }

  depthFirstSearchInorder(){
    return traverseInOrder(this.root, [])
  }

  depthFirstSearchPostorder(){
    return traversePostOrder(this.root, [])
  }

  depthFirstSearchPreorder(){
    return traversePreOrder(this.root, [])
  }
}

function traverseInOrder(node, list){
  if(node.left)
    traverseInOrder(node.left, list)
  list.push(node.value)
  if(node.right)
    traverseInOrder(node.right, list)
  return list
}

function traversePostOrder(node, list){
  if(node.left)
    traversePostOrder(node.left, list)
  if(node.right)
    traversePostOrder(node.right, list)
  list.push(node.value)
  return list
}

function traversePreOrder(node, list){
  list.push(node.value)
  if(node.left)
    traversePreOrder(node.left, list)
  if(node.right)
    traversePreOrder(node.right, list)
  return list
}

const tree = new BinarySearchTree()
tree.insert(9)
tree.insert(4)
tree.insert(6)
tree.insert(20)
tree.insert(170)
tree.insert(15)
tree.insert(1)
//tree.breadthFirstSearch()
console.log(tree.breadthFirstSearchRecursive([tree.root], []))
console.log(tree.depthFirstSearchInorder())
console.log(tree.depthFirstSearchPreorder())
console.log(tree.depthFirstSearchPostorder())
```

---

# Dynamic Programming

Dynamic Programming it's just an _optimization technique_ that use something called "**caching**". If you have something that you can cache, than you can use dynamic programming.
Is a way to solve problems by breaking it down and resolve sub-problems and storing their solutions in case of other similar problems occurs.
Caching is just a way to speed up programs, used to access data faster.
**Memoization** is a particular type of caching.

## Memoization

Is a specific form of caching that involves caching that returns values based of its parameters.
In Javascript, you have to declare a function that returns a function (closure). The parent function will have a cahcing variable, and the inner function will execute based on that caching variable. So, if the cache already has a value associated to a particular parameter, it'll immediately return it, rather than re-executing the whole function.
This **extremely** increment the efficiency of a function, especially if it's recursive (see fibonacci example).

### Rules of memoization

To know when to use memoization, you can follow this pattern:

1. Can be divided into subproblem
2. Recursive solution
3. Are there repetitive subproblems?
4. Memoize subproblems
5. Demand a raise from your boss :D

## Examples

Base caching

```javascript
function addTo80(n){
  console.log('long time')
  return n+80
}

function memoizedAddTo80(){
  let cache = {}
  // Closures
  return function(n){
    if(n in cache)
      return cache[n]
    console.log('long time')
    cache[n] = n + 80
    return cache[n]
  }
}

const memoize = memoizedAddTo80()
console.log(memoize(5))
console.log(memoize(5))
console.log(memoize(5))
```

Fibonacci with caching

```javascript
let calculations = 0
// O(2^n)
function fibonacci(n){
  calculations++;
  if(n < 2)
    return n
  return fibonacci(n-1) + fibonacci(n-2)
}

console.log(fibonacci(30))
console.log('Calculations: ', calculations) // REALLY REALLY BAD
// 832040 calculations to only get 30th value
console.log('=================')


let calculationMemoized = 0
// O(n)
function fibonacciMaster(){
  let cache = {}
  return function fib(n){
    calculationMemoized++;
    if(n in cache)
      return cache[n]
    if(n < 2)
      return n
    cache[n] = fib(n-1) + fib(n-2)
    return cache[n]
  }
}
const fibonacciMem = fibonacciMaster()

console.log(fibonacciMem(30))
console.log('Calculations: ', calculationMemoized)
```




# Greetings

Oooof...
Good boy, you now are an expert of Data Structures and Algorithms.
Give yourself a big pat on your back...

Love you <3
