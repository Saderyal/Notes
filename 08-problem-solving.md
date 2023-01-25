# Problems

This "chapter" is created basically to talk about the problems that affect my personali- No I'm just kidding. This "chapter" is created to improve you're developer experience and level up by solving "common" (no they're not common) problems, which are usually asked to solve in FAANG interviews. Here I'll use JS, but you can replicate it as any programming language.

More specifically, here we're talking about **technical interviews**.

There are _3 keys_ in technical interviews:

1. Techinical knowledge
2. Critical & Abstract Problem Solving
3. Communication

Befeore starting seeing problems, there are some step to follow before thinking about a solution for that problem, which are:

1. Verify the constraints. (e.g. Are all the numbers positive or can be negative? Are there duplicates? ecc.).
Basically, **_Always ask questions to clear your mind!!_**
2. **_Write out some test cases._**. This step is really important to clear your mind and focus on finding a solution for that problem.
3. **_Figure out a solution without code_**. Think only about a logical solution to solve the problem.
4. **_Write your solution_**, exaplining all the steps and your considerations. Talk a lot!
5. **_Double check for errors_**. You don't wanna have a variable called "_cum_" instead of "_sum_" in an interview 🗿.
6. **_Test your solution_** with different inputs, and follow the code step by step to see if it correctly solves the problem.
7. **_Analyze space and Time Complexity_** of your solution. How many resources utilize your solution in a big-size problem (millions of elements in an array, for example).
8. **_Optimize your solution_** (if possible).

Ok. Now, we can solve our mental problems.
Note that there are different solutions for each problem, but I'll only give the most performant one (speaking about the time complexity).

## Two Sum

Difficulty: _Easy_

**PROBLEM**:

> Given an array of integers, return the indices of the two numbers that add up to a given target.

Example:
```
[1,3,7,9,2] -> Search two numbers that give 11 as sum
```

In the example, you want to return the indeces of the two numbers that add up to 11.

**SOLUTION**:

Do a loop in the array and save its value and corresponding index in an hash. If in the hash there is an item which value corresponds to the subtraction of the search value - current value, return the index of the current value and the index of the existing value in the hash.

This has a time complexity of `O(n)`.

```javascript
function solution(nums, target) {
  const numsMap = {}
  for (let p=0; p<nums.length; p++) {
    const currentMapValue = numsMap[nums[p]]
    if(currentMapValue >= 0){
      return [currentMapValue, p]
    } else {
      const numberToFind = target - nums[p]
      numsMap[numberToFind] = p
    }
  }
  return null;
}
```

## Container With Most Water

Difficulty: _Medium_

**PROBLEM**:

> You are given an array of positive integers where each integer represents the height of a vertical line on a chart. Find two lines which together with the x-axis forms a container that would hold the greatest amount of water. Return the area of water it would hold.

Example:

```
[1,8,6,2,9,4] -> The maximum area is 8 and 9, with a base of 3 units)

        |  
  |-----|  
  |     |  
  | |   |  
  | |   |  
  | |   | |
  | |   | |
  | | | | |
| | | | | |
1 8 6 2 9 4
```

**SOLUTION**:

By shifting two pointers (one from the start and one from the end), we know that if the biggest value gets larger, it doesn't matter, because only the smaller value will have an impact on the area. So, we start shifting the value (to right and left, respectively) and we calculate the area only if the smaller value increases. We will then return the maximum area we found.

This has a time complexity of `O(n)`.

```javascript
function solution(heights){
  let p1 = 0;
  let p2 = nums.length-1;
  let maxArea = 0;
  while(p1 < p2){
    const height = Math.min(heights[p1], heights[p2]);
    const width = p2 - p1;
    const area = height * width;
    maxArea = Math.max(maxArea, area);
    if(heights[p1] <= heights[p2])
      p1++
    else
      p2 --
  }
  return maxArea;
}
```

## Trapping Rainwater

Difficulty: _hard_

**PROBLEM**:

> Given an array of integers representing an elevation map where the width of each bar is 1, return how much rainwater can be trapped.

Example:

```
[0,1,0,2,1,0,3,1,0,1,2]

How much rainwater can be trapped here?
The sides of the chart are not walls.
            _
      _    | |      _
  _  | |_  | |_   _| |
_| |_|   |_|   |_|   |
0 1 0 2 1 0 3 1 0 1 2 
```

**SOLUTION**:

With two pointers (one from the beginning and one from the end of the array), we keep track of the maximum value respectively on the right of the first pointer and the maximum value on the left of the second pointer. You compare the values of the left and right pointers, and move the right one only when the value of the left one is greater than the value of the right. You increase the total by the difference of the current maximum minus the current value of the pointer.
It's difficult to explain.

This has a time complexity of `O(n)`.

```javascript
function solution(heights){
  let totalWater = 0;
  let left = 0;
  let right = heights.length-1;
  let maxLeft = 0;
  let maxRight = 0;
  while (left < right){
    if(heights[left] <= heights[right]){
      if(heights[left] >= maxLeft)
        maxLeft = heights[left];
      else
        totalWater += maxLeft - heights[left];
      left++;
    } else {
      if(heights[right] >= maxRight)
        maxRight = heights[right];
      else
        totalWater += maxRight - heights[right];
      right--;
    }
  }
  return totalWater;
}
```

## Typed Out Strings

Difficulty: _Easy_

**PROBLEM**:

> Given two string S and T, return if they equal when both are typed out. Any '#' that appears in the string counts as a backspace (delete the previous character). Consecutive n '#' deletes n previous characters. Case sensitivity matters.

Example:

```
S: "ab#c" = "ac"
T: "az#c" = "ac"

S == T
```

**SOLUTION**:

Starting from the end of each string, we move our pointers to the left and compare the characters. If we find an '#', we move the pointer of n consecutive '#' found, and resume comparing those characters.

This has a time complexity of `O(a+b)` (where `a` and `b` are the lenght of the strings).

This has a space complexity of `O(1)`.

```javascript
function solution(S, T){
  let p1 = S.length-1;
  let p2 = T.length-1;
  while(p1 >= 0 || p2 >= 0){
    if(S[p1] === '#' || T[p2] === '#'){
      if(S[p1] === '#'){
        let backcount = 2;
        while(backcount > 0){
          p1--;
          backcount--;
          if(S[p1] === '#')
            backcount += 2;
        }
      }
      if(T[p2] === '#'){
        let backcount = 2;
        while(backcount > 0){
          p1--;
          backcount--;
          if(T[p2] === '#')
            backcount += 2;
        }
      }
    } else {
      if(S[p1] !== T[p2]){
        return false;
      } else {
        p1--;
        p2--;
      }
    }
  }
  return true;
}
```

## Longest Substring Without Repeating Characters

Difficulty: _Easy_

**PROBLEM**:

> Given a string, find the lenght of the longest substring without repeating characters. All characters are lowe cased.

Example:

```
"abccabb" -> "abc" or "cab" -> The maximum length is 3.
```

**SOLUTION**:

In this problem we can adopt the "**Sliding window**" technique: form a window over some portion of sequential data (sequential order which is important), then move that window throughout the data to capture different parts of it. If you know what I'm talking about, it's the concept used in Convolutional Layers in Machine Learning.
Here, we'll adjust the size of the sliding window based on the maximum length that we have found. We also keep track of seen characters in an hash map (character: index).
Me move the window in the duplicate character we found. If we find a character that our hash map already has, but the index is minor than the index of our current pointer, we don't count it as a duplicate.

This solution has a time complexity of `O(n)`.


```javascript
function solution(s){
  if(s.length <= 1)
    return s.length;
  const seenChars = {};
  let left = 0;
  let longest = 0;
  for(let right = 0; right < s.length; right++){
    const currentChar = s[right];
    const prevSeenChar = seenChars[currentChar];
    if(prevSeenChar >= left){
      left = prevSeenChar+1;
    }
    seenChars[currentChar] = right;
    longest = Math.max(longest, rigth - left + 1);
  }
  return longest;
}
```

## Valid Palindrome

Difficulty: _Easy_

**PROBLEM**:

> Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring case sensitivity.

Example:

```
"aabaa" -> true
"babba" -> false
"A man, a plan, a canal: Panama" -> true
```

**SOLUTION**:

With two pointers, one from the start and one from the end, analyze the string. If the character at pointer is different from the other, it's not palindrom.

This solution has a time complexity of `O(n)`.

```javascript
function solution(s){
  s = s.replace(/[^A-Za-z0-9]/g, '').toLowerCase();
  let left = 0;
  let right = s.length-1;
  while (left < right){
    if(s[left] !== s[right])
      return false
    left++;
    right--;
  }
  return true;
}
```

## Almost Palindrome

**PROBLEM**:

> Given a string, determine if it is almost a palindrome. A string is almost a palindrome if it becomes a plindrome by removing 1 letter. Consider only alphanumeric characters and ignore case sensitivity.

Example:

```
"raceacar" -> remove "a" -> "racecar" -> true
```

SOLUTION:

With two pointers, if we find a character that mismatch, remove one of the two and see if it's a palindome. If it's not, try to remove the other.

This solution has a time complexity of `O(n)`.

```javascript
function validSubPalindrome(s, left, right){
  while (left < right){
    if(s[left] !== s[right])
      return false
    left++;
    right--;
  }
  return true;
}

function solution(s){
  s = s.replace(/[^A-Za-z0-9]/g, '').toLowerCase();
  let left = 0;
  let right = s.length-1;
  while (left < right){
    if(s[left] !== s[right]){
      let alt1String = s.substr(left, right-1);
      let alt1String = s.substr(left+1, right);
      return validSubPalindrome(s, left+1, right) || validSubPalindrome(s, left, right-1);
    }
    left++;
    right--;
  }
  return true;
}
```

## Reverse a Linked List

Difficulty: _Medium_.

**PROBLEM**:

> Given a Linked List, return it in reverse.

Example:

```
1 -> 3 -> 5   =   5 -> 3 -> 1
```

**SOLUTION**:

You keep two pointers, one in the current node and one in the next node. You increase them one by one and change the pointer of the first to the next node of the second, then point the second to the first.

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(1)`.

```javascript
function solution(head){
  let prev = null;
  let current = head;
  while(current){
    let next = current.next;
    current.next = prev;
    prev = current;
    current = next;
  }
  return prev;
}
```

## M, N Reversal

Difficulty: _Medium_.

**PROBLEM**:

> Given a linked list and numbers `m` and `n`, return it back with only positions m to n in reverse.

Example:

```
1 -> 2 -> 3 -> 4 -> 5 -> null

m = 2    n = 4

Output: 1 -> 4 -> 3 -> 2 -> 5 -> null
```

**SOLUTION**:

By Splitting the problem in two parts, one to reverse a linked list (previosly seen), and one to connect the "non relevant" nodes to the reversed linked list, we can solve the problem.
The only difference is that the reversal operation must stop not when the next node is null but when it's equal to `m`. Then, we connect the pieced to the head and to the tail of that reversed list.

This solution has a time complexity of `O(n)`.

```javascript
function solution(head, m, n){
  let currentPos = 1;
  let currentNode = head;
  let start = head;
  while(currentPos < m){
    start = currentNode;
    currentNode = currentNode.next;
    currentPos++;
  }
  let newList = null;
  let tail = currentNode;
  while(currentPos >= m && currentPos <= n){
    const next = currentNode.next;
    currentNode.next = newList;
    newList = currentNode;
    currentNode = next;
    currentPos++;
  }
  start.next = newList;
  tail.next = currentNode;
  if(m > 1)
    return head;
  else
    return newList;
}
```

## Merge Multi-Level Doubly Linked List

Difficulty: _Easy_.

**PROBLEM**:

> Given a doubly linked list, list nodes also have a child property that can point to a separate doubly linked list. These child lists can also have one or more child doubly linked lists of their own, and so on. Return the list as a single level flattened doubly linked list.

Example:

```
1 <-> 2 <-> 3
      |
      4 <-> 5 <-> 6
            |
            7

Flattened:

1 <-> 2 <-> 4 <-> 5 <-> 7 <-> 6 <-> 3
```

**SOLUTION**:

This problem clearly have a sub-problem, so we can for sure use recursion to flatten each child in the parent for each node. But, let's focus on the clear way.
We can travel the linked list and see if a node has a child. If true, we save the `.next` pointer and start travelling the node by setting the `.next` to the child. We repeat the same procedure for the child, and finally merge the tail of the child to the saved next pointer.

This solution has a time complexity of `O(n)`.

```javascript
function solution(head){
  if(!head)
    return head;
  let currentNode = head;
  while(currentNode !== null){
    if(currentNode.child === null){
      currentNode = currentNode.next;
    } else {
      let tail = currentNode.child;
      while(tail.next !== null){
        tail = tail.next;
      }
      tail.next = currentNode.next;
      if(tail.next !== null)
        tail.next.prev = tail;
      currentNode.next = currentNode.child;
      currentNode.next.prev = currentNode;
      currentNode.child = null;
    }
  }
  return head;
}
```

## Cycle Detection

Difficulty: _Easy_.

**PROBLEM**:

> Detect if a linked list has a cycle, so return false if the linked list has no cycle, otherwise return the node where the cycle begins.

Example:

```
              > 4 -> 5
            /         \
1 -> 2 -> 3            6
            \         /
              8 <- 7 <
```

**SOLUTION**:

To solve this problem we can use the **_Floyd's Tortoise and Hare algorithm_**. Basically, we have two pointers, one that goes 1 step forward per iteration, and one that goes 2 steps forward per iteration. If the two pointers meet each other, you found a cycle.

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(1)`.

```javascript
function solution(head){
  let hare = head;
  let tortoise = head;
  while(true){
    hare = hare.next;
    tortoise = tortoise.next;
    if(hare === null || hare.next === null)
      return false;
    hare = hare.next;
    if(tortoise === hare)
      break;
  }
  let p1 = head;
  let p2 = tortoise;
  while(p1 !=== p2){
    p1 = p1.next;
    p2 = p2.next;
  }
  return p1;
}
```

## Valid Parentheses

Difficulty: _Easy_.

**PROBLEM**:

> Given a string containing only parenthesis, determine if it is valid. The string is valid if all parentheses close.

Example:

```
"{([])}" -> True
"{[]()}" -> True
"" -> True
"{(]}" -> False
"{([)]}" -> False
"{([]" -> False
```

**SOLUTION**:

For this solution we can use a `Stack`. We add in the stack only opening parentheses. If we find the corresponding closing parentheses, we pop that item from the stack. If at the end of the string we have no item in the stack, it is correct. If we find a closing tag different from the last stack item opening tag, the string is not correct (also if there are still elements in the stack).

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(n)`.

```javascript
const parens = {
  '(': ')',
  '[': ']',
  '{': '}',
}
function solution(s){
  const stack = [];
  for(let i=0; i<s.length; i++){
    if(parens[s[i]]){
      stack.push(s[i]);
    } else {
      const leftBracket = stack.pop();
      const correctBracket = parens[leftBracket]
      if(s[i] !== correctBracket)
        return false;
    }
  }
  return stack.length === 0;
}
```

## Minimum Brackets To Remove

Difficulty: _Medium_

**PROBLEM**:

> Given a string only containing round brackets "(" and ")" and lowercase characters, remove the least amount of brackets so the string is valid. A string is considered valid if it is empty or if there are brackets, they all close.

Example:

```
"a)bc(d)" -> "abc(d)" -> Valid
"(ab(c)a" -> "(abc)a" or "ab(c)a" -> Valid
"))" -> "" -> Valid
"aaaaa" -> Valid
```

**SOLUTION**:

By scanning the string, if we find a closing parenthesis that have not a corresponding opening parenthesis in a stack that we use to store the order of the opening parenthesis. At the end of the string, you also remove all the remaining parentheses.

This has a time complexity of `O(n)`.

This has a space complexity of `O(n)`.

```javascript
function solution(str){
  const res = str.split("");
  const stack = [];
  for(let i = 0; i < res.length; i++) {
    if(res[i] === '(')
      stack.push(i);
    else if(res[i] === ')' && stack.length)
      stack.pop();
    else if(res[i] === ')')
      res[i] = '';
  }
  while(stack.length){
    const curIdx = stack.pop();
    res[curIdx] = '';
  }
  return res.join('');
}
```

## Implement Queue With Stacks

Difficulty: _Easy_.

**PROBLEM**:

> Implement the class Queue using Stacks. The queue methods you need to implement are enqueue, dequeue, peek, and empty.

I can't define an example for this. I'm sorry 🗿.

**SOLUTION**:

Basically, a queue it's a reverse of the stack, If we keep two different stacks, we can pop element on the second to determine the queue order. So, we push in the first, we pop in the second.

```javascript
class QueueWithStacks {

  constructor(){
    this.in = [];
    this.out = [];
  }

  // `O(1)`
  enqueue(val){
    this.in.push(val);
  }

  // `O(n)`
  dequeue(){
    if(this.out.length === 0){
      while(this.in.length){
        this.out.push(this.in.pop());
      }
    }
    return this.out.pop();
  }

  // `O(n)`
  peek(){
    if(this.out.length === 0){
      while(this.in.length){
        this.out.push(this.in.pop());
      }
    }
    return this.out[this.out.length - 1];
  }

  // `O(1)`
  empty(){
    return this.in.length === 0 && this.out.length === 0;
  }
}
```

## Kth Largest Element

Difficulty: _Medium_

**PROBLEM**:

> Given a unsorted array, return the kth largest element. It is the kth largest element in sorted order, not the kth distinct element.

Example:

```
k=2
[5,3,1,6,4,2] -> [1,2,3,4,5,6] = 5

k=4
[2,3,1,2,4,2] -> [1,2,2,2,3,4] = 2
```

**SOLUTION**:

We can use the Hoare's Quickselect algorithm to achive the best solution, and the return the kth element starting from the end of the sorted array.
Note: Hoare is the same person who developed the quicksort algorithm.

This solution has a time complexity of `O(n log n)` (but `O(n)` in the best case 😎) (but also `O(n^2)` in the worst case 💩).

This solution has a space complexity of `O(log n)`.

```javascript
function quickSelect(array, left, right, idxToFind){
  if(left < right){
    const partitionIdx = partition(array, left, right);
   if(partitionIdx === idxToFind){
    return array[partitionIdx];
   } else if(idxToFind < idxToFind) {
      return quickSelect(array, left, partitionIdx-1, idxToFind);
   } else {
    return quickSelect(array, left, partitionIdx+1, idxToFind);
   }
  }
}

function partition(array, left, right){
  const pivot = array[right];
  let partitionIdx = left;
  for(let j=left; j<right; j++){
    if(array[j] < pivot){
      swap(array, partitionIdx, j);
      partitionIdx++;
    }
  }
  swap(array, partitionIdx, right);
  return partitionIdx;
}

function swap(array, i, j){
  const temp = array[i];
  array[i] = array[j];
  array[j] = temp;
}

function solution(array, k){
  const idxToFind = array.length - k;
  quickSelect(array, 0, array.length-1, idxToFind);
  return array[idxToFind];
}
```

## Start And End Of Target

Difficulty: _Medium_

**PROBLEM**:

> Given an array of integers sorted in ascending order, return the starting and ending index of a given target value in an array, i.e. [x, y]. Your solution should run in `O(log n)`.

Example:

```
target = 5

[1,3,3,5,5,5,8,9] -> [3,5]
start: 3   ending: 5
```

**SOLUTION**:

We can use the binary search algorithm to first search for our target. Then, we can do another binary search on the left and right array of the found value, and repeat this operation until we don't find the target anymore.

This solution has a time complexity of `O(log n)`.

This solution has a space complexity of `O(1)`.

```javascript
function binarySearch(nums, left, right, target){
   while(left <= right){
    const mid = Math.floot((left+right)/2);
    const foundVal = nums[mid];
    if(foundVal === target)
      return mid;
    else if(foundVal < target)
      left = mid+1;
    else
      right = mid-1;
  }
  return -1;
}

function solution(nums, target){
  if(num.length === 0)
    return [-1,-1];
  const firstPos = binarySearch(nums, 0, nums.length-1, target);
  if(firstPos === -1)
    return [-1,-1];
  let startPos = firstPos;
  let endPos = firstPos;
  let temp1;
  let temp2;
  while(startPos !== -1){
    temp1 = startPos;
    startPos = binarySearch(nums, 0, startPos-1, target);
  }
  startPos = temp1;
  while(endPos !== -1){
    temp2 = endPos;
    endPos = binarySearch(nums, endPos+1, nums.length-1, target);
  }
  endPos = temp2;
  return [startPos, endPos];
}
```

## Maximum Depth of Binary Tree

Difficulty: _Easy_

**PROBLEM**:

> Given a binary tree, find its maximum depth. Maximum depth is the number of nodes along the longest path from the root node to the furthest lead node.

Example:

```
Depth = 5

      °
    /   \
   °     °
 /      / \
°      °   °
 \      \
  °      °
   \
    °
```

**SOLUTION**:

We can use the Depth First Search algorithm (DFS) to find the deepest level of the tree.

This has a time complexity of `O(n)`.

This has a space complexity of `O(log n)` (`O(n)` in the worst case).

```javascript
function solution(node, currentDepth) {
  if(!node)
    return currentDepth;
  currentDepth++;
  return Math.max(
    solution(node.left, currentDepth),
    solution(node.right, currentDepth)
  );
}
```

## Level Order of Binary Tree

Difficulty: _Medium_

**PROBLEM**:

> Given a binary tree, return the level order traversal of the nodes' values as an array.

Example:

```
      3
    /   \
   6     1
 /  \     \
9    2     4
 \ 
  5 
 /
8

Result: [[3], [6,1], [9,2,4], [5], [8]]
```

**SOLUTION**:

We can use the Breadth First Search algorithm to traverse the tree, but with a little modification. We keep track with a counter, the current level nodes to process, so, after that count is finished, we know that we also finished to process a level.

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(n)`.

```javascript
function solution(root){
  const result = [];
  if(!root)
    return result;
  const queue = [root];
  while(queue.length){
    let length = queue.length;
    let count = 0;
    const currentLevelValues = [];
    while(count < length){
      const currentNode = queue.shift();
      currentLevelValues.push(currentNode.value);
      if(currentNode.left)
        queue.push(currentNode.left);
      if(currentNode.right)
        queue.push(currentNode.right);
      count++;
    }
    result.push(currentLevelValues);
  }
  return result;
}
```

## Right Side View of Tree

Difficulty: _Medium_

**PROBLEM**:

> Given a binary tree, image you're standing to the right of the tree. Return an array of the values of the nodes you can see ordered from top to bottom.

Example:

```
      1         <-
    /   \       
   2     3      <-
 /  \     \
4    5     6    <-
 \ 
  7             <-
 /
8               <-

Solution: [1, 3, 6, 7, 8]
```

**SOLUTION**:

We can use a BFS algorithm just like we did in the previous problem. we then add in the result each last index node of each level.
In this problem, even the DFS algorithm is valid. We are going to use that. By prioritizing the right side, we can immediately add in the solution the most-right value. Than, by travelling other nodes, we can check if a value of `solution[level]` value is present. If it is, we ignore it, otherwise we add it to the solution. Note that we must keep track of the level of each node.
We can so use the DFS algorithm with a pre-order traversal.

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(n)`.

```javascript
function dfs(node, currentLevel, result){
  if(!node)
    return;
  if(currentLevel >= result.length)
    result.push(node.value);
  if(node.right)
    dfs(node.right, currentLevel+1, result);
  if(node.left)
    dfs(node.left, currentLevel+1, result);
}

function solution(root){
  const result = [];
  dfs(root, 0, result);
  return result;
}
```

## Number of Nodes in Complete Tree

Difficulty: _Hard_

**PROBLEM**:

> Given a complete binary tree, count the number of nodes.

Note: A complete tree is a tree that contains all levels fullfilled of nodes, except for the last one.

Example:

```
         °
      /     \
     °       °
   /  \     / \
  °    °   °   °
 /  \
°    ° 

Solution: 8
```

**SOLUTION**:

We can derive the "full" levels by `(2^(maxLevel-1))-1`, then, we add the number of remaining nodes in the last level. So, we first have to travel the tree to get its height. Then, we can do a binary search to find the indexes of the last level nodes. It's difficult to exaplain without a visual representation, but basically, if you have 8 nodes in the last level, you apply a binary search to an array equals to `[0,1,2,3,4,5,6,7]`.

This solution has a time complexity of `O(log^2 n)`.

This solution has a space complexity of `O(1)`.

```javascript
function solution(root){
  if(!root)
    return 0;
  const height = getTreeHeight(root);
  if(height === 0)
    return 1;
  const upperCount = Math.pow(1, height) - 1;
  let left = 0;
  let right = upperCount;
  while(left < right){
    let idxToFind = Math.ceil((left + right) / 2);
    if(nodeExists(idxToFind, height, root))
      left = idxToFind;
    else
      right = idxToFind - 1;
  }
  return upperCount + left + 1;
}

function getTreeHeight(root){
  let height = 0;
  while(root.left !== null){
    height++;
    root = root.left;
  }
  return height;
}

function nodeExists(idxToFind, height, node){
  let left = 0;
  let right = Math.pow(2, height) - 1;
  let count = 0;
  while(count < height){
    let midOfNode = Math.ceil((left + right) / 2);
    if(idxToFind >= midOfNode){
      node = node.right;
      left = midOfNode;
    } else {
      node = node.left;
      right = midOfNode - 1;
    }
    count++;
  }
  return node !== null;
}
```

## Validate Binary Search Tree

Difficulty: _Medium_

**PROBLEM**:

> Given a binary tree, determine if it is a valid binary search tree.

Example:

```
      12
    /   \
   8     18
 /  \    / \
5   10  14  4

This is a valid binary search tree.
```

**SOLUTION**:

We need to implement a Depth First Search algorithm to traverse the tree, and then make the appropriate checks to determine if the tree is also a valid binary search tree. The traverse order is important. In this case, we should use a Pre-Order traversal.
In the left side, we just have to check if the left is less than parent. Same thing (but opposite) on the right. For right nodes of left side, and left nodes of right side, we have to check if its value is betwenn the parent and the parent of the parent. But, it would be expensive to store all those value. All we have to do, is just set some boundaries for each node, which starts from Infinity. 

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(n)`.

```javascript
function solution(root){
  if(!root)
    return true;
  return dfs(root, -Infinity, Infinity);
}

function dfs(node, min, max){
  if(node.vaue <= min || node.vaue >= max)
    return false;
  if(node.left){
    if(!dfs(node.left, min, node.value))
      return false;
  }
  if(ndoe.right){
    if(!dfs(node.right, node.value, max))
      return false;
  }
  return true;
}
```

## Number of Islands

Difficulty: _Medium_

**PROBLEM**:

> Given a 2D array containing only 1's (land) and 0's, count the number of Islands. An island is land connected horizontally or vertically.

Example:

```
1,1,1,1,0
1,1,0,1,0
1,1,0,0,1
0,0,0,1,1

It has 2 islands.
```

**SOLUTION**:

We can use BFS algorithm (of 2D arrays, not trees), and switch to 0 the 1s that we found, so that we don't go in the same island twice.

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(n)`.

```javascript
const directions = [
  [-1, 0], //up
  [0, 1], //right
  [1, 0], //down
  [0, -1] //left
]

function solution(){
  if(matrix.length === 0)
    return 0;
  let islandCount = 0;
  for(let row=0; row < matrix.length; row++){
    for(let col=0; col < matrix[0].length; col++){
      if(matrix[row][col] === 1){
        islandCount++;
        matrix[row][col] = 0;
        const queue = [];
        queue.push([row,col]);
        while(queue.length){
          const curPos = queue.shift();
          const curRow = curPos[0];
          const curCol = curPos[1];
          for(let i = 0; i < directions.length; i++){
            const currentDir = directions[i];
            const nextRow = currentRow + currentDir[0];
            const nextCol = currentCol + currentDir[1];
            if(nextRow < 0 || nextRow >= matrix.length || nextCol < 0 || nextCol >= matrix[0].length){
              continue;
            }
            if(matrix[nextRow][nextCol] === 1){
              queue.push([nextRow, nextCol]);
              matrix[nextRow][nextCol] = 0;
            }
          }
        }
      }
    }
  }
  return islandCount;
}
```

## Rotting Oranges

Difficulty: _Medium_

**PROBLEM**:

> Given a 2D array containing 0's (empty cell), 1's (fresh orange) and 2's (rotten orange). Every minute, all fresh orange immediately adjacent 4 directions to rotten oranges will rot. How many minutes must pass until all oranges are rotten?

Note: If an organge is isolated (so can't become rot), we return -1. If the matrixi is empty, we return 0.

Example:

```
2,1,1,0,0
1,1,1,0,0
0,1,1,1,1
0,1,0,0,1

It will take 7 minutes.
```

**SOLUTION**:

It's immediately clear that we should use the BFS algorithm to rot all near oranges adjacent to a rotten orange. First, we must find all rotten oranges, then we add to a queue the coordinates of the rotten oranges, and we process them and rot all adjacent oranges. We also add to the queue the newly rotten oranges, and increment our minutes counter.

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(n)`.

```javascript
const directions = [
  [-1, 0], //up
  [0, 1], //right
  [1, 0], //down
  [0, -1] //left
]

const ROTTEN = 2;
const FRESH = 1;
const EMPTY = 0;

function solution(matrix){
  if(matrix.length === 0)
    return 0;
  const queue = [];
  let freshOranges = 0;
  for(let row = 0; row < matrix.length; row++){
    for(let col = 0; col < matrix.length; col++){
      if(matrix[row][col] === ROTTEN)
        queue.push([row, col]);
      else if(matrix[row][col] === FRESH)
        freshOranges++;
    }
  }
  let currentQueueSize = queue.length;
  let minutes = 0;
  while(queue.length){
    if(currentQueueSize === 0){
      minutes++;
      currentQueueSize = queue.length;
    }
    const currentOrange = queue.shift();
    currentQueueSize--;
    const row = currentOrange[0];
    const col = currentOrange[1];
    for(let i = 0; i < direction.length; i++){
      const currentDir = direction[i];
      const nextRow = currentDir[0]+row;
      const nextCol = currentDir[1]+col;
      if(nextRow < 0 || nextRow >= matrix.length || nextCol < 0 || nextCol >= matrix.length)
        continue;
      if(matrix[nextRow][nextCol] === FRESH){
        matrix[nextRow][nextCol] = ROTTEN;
        queue.push([nextRow, nextCol]);
      }
    }
  }
  if(freshOranges > 0)
    return -1;
  return minutes;
}
```

## Walls And Gates

Difficulty: _Medium_

**PROBLEM**:

> Given a 2D array containing -1's (walls), 0's (gates) and INF's (empty room). Fill each empty room with the number of steps to the nearest gate. If it is impossible to reach a gate, leave INF as the value. INF is equal to `2147483647`.

Example:

```
INF, -1, 0, INF,
 -1,INF,INF, -1,
INF, -1,INF, -1,
  0, -1,INF,INF,

Becomes:

INF,-1, 0, 1,
 -1, 2, 1,-1,
  1,-1, 2,-1,
  0,-1, 3, 4,
```

**SOLUTION**:

For this problem you have to think in "reverse". You actually have to focus on the gates, not on the INF values. So, starting from each gates, you have to do a DFS travelling of the matrix. You increment a counter for each iteration of the algorithm, and replace the value of an empty room with the value of the current counter. If a room has already a value (because it's accessible from another gate), you only replace the value if the actual counter is less than the room value. In the other case, you stop the search from that point, because the neighbor room will surely have a less value.

This solution has a time complexity of `O(n)`.

This solution has a space complexity of `O(n)`.

```javascript
const directions = [
  [-1, 0], //up
  [0, 1], //right
  [1, 0], //down
  [0, -1] //left
]

const WALL = -1;
const GATE = 0;
const EMPTY = 2147483647;

function dfs(matrix, row, c ol, currentStep){
  if(row < 0 || row >= matrix.length || col < 0 || col >= matrix.length || currentStep > matrix[row][col])
    return;
  matrix[row][col] = currentStep;
  for(let i=0; i<directions.length; i++){
    const currentDir = directions[i];
    dfs(matrix, row+currentDir[0], col+currentDir[1], currentStep+1);
  }
}

function solution(matrix){
  for(let row=0;row<matrix.length; row++){
    for(let col=0;col<matrix.length; col++){
      if(matrix[row][col] === GATE){
        dfs(matrix, row, col, 0);
      }
    }
  }
  return matrix;
}
```

## Time Needed to Inform All Employees

Difficulty: _Medium_

**PROBLEM**:

> A company has n Emplyees with unique IDs from 0 to n-1. The heead of the company has the ID headID. You will receive a managers array where `managers[i]` is the ID of the manager for employee `i`. Each employee has one direct manager. The company head has no manager (managers[headID] = -1). It's guaranteed the subordination relationships will have a tree structure. Figure out the total time of minutes to inform all employees (given an informTime array).

Example:

```
8 Employees: 0, 1, 2, 3, 4, 5, 6, 7

headID = 4

managers = [2, 2, 4,  6, -1, 4, 4, 5]

informTime = [0, 0, 4, 0, 7, 3, 6, 0]

Solution: 13
```

**SOLUTION**:

From the question we can understand that there are no cycles in the graph, it can't be unconnected, and it's not weighted.
We can use an Adjacency List to represent our graph, becasue there's no need to use an Adjacency Matrix.
Then, we can use the DFS Traversal of the graph to calculate the inform time of a complete branch of the graph.

This solution has a time complexity of `O(n)`.

```javascript
function dfs(currentID, adjList, informTime){
  if(adjList[currentID].length === 0)
    return 0;
  let max = 0;
  const subordinates = adjList[currentID];
  for(let i = 0; i < subordinates.length; i++){
    max = Math.max(max, dfs(subordinates[i], adjList, informTime));
  }
  return max + informTime;
}

function solution(n, headID, managers, informTime){
  const adjList = managers.map(() => []);
  for(let e = 0; e < n; e++){
    const manager = managers[e];
    if(manager === -1)
      continue;
    adjList[manager].push(e);
  }
  return dfs(headID, adjList, informTime);
}
```

## Course Scheduler

Difficulty: _Medium_

**PROBLEM**:

> There are a total of n course to take, labeled from 0 to n-1. Some courses have prerequisites courses. This is expressed as a pair (i.e. [1, 0]) which indicates you must take course 0 before taking course 1 (in the example). Given the total number of courses and an array of prerequisites pairs, return if it is possible to finish all courses.

Example:

```
n = 6 -> 6 courses: 0,1,2,3,4,5

prerequisites = [
  [1,0],
  [2,1],
  [2,5],
  [0,3],
  [4,3],
  [3,5],
  [4,5],
]

    3 - 4
  / | /
0 - 5
 \   \
  1 - 2

TRUE -> It's possible to finish all courses.
```

**SOLUTION**:

Basically, the question ask to detect a cycle in the graph (which would be the only case when a person couldnt take a course, because a course is dependent on another one, which is dependent on the same, so it's not possible to take that course).
We can solve the problem using the Topological Sort algorithm. The topological order orders the graph's elements by the number of collections a node has. This is not applicable to a Graph which has a cycle. IT must be a DAG (Directed Acyclic Graph). That algorithm basically removes the nodes that has 0 connections, and updated the counter of connection of each node. It repeats untile there are no more nodes in the graph.
We can use that to know where the algorithm "brakes", so that we detect a cycle and return false.

This solution has a time complexity of `O(n^2)`.

This solution has a space complexity of `O(n^2)`.

```javascript
function solution(n, prerequisites) {
  const inDegree = new Array(n).fill(0);
  const adjList = inDegree.map(() => []);
  for(let i = 0; i < prerequisites.length; i++) {
    const pair = prerequisites[i];
    inDegree[pair[0]]++;
    adjList[pair[1]].push(pair[0]);
  }
  const stack = [];
  for(let i = 0; i < inDegree.length; i++){
    if(inDegree[i] === 0)
      stack.push(i);
  }
  let count = 0;
  while(stack.length){
    const current = stack.pop();
    count++;
    const adjacent = adjList[current];
    for(let i=0; i < adjacent.length; i++){
      const next = adjacnet[i];
      inDegree[next]--;
      if(inDegree[next] === 0)
        stack.push(next);
    }
  }
  return count === n;
}
```

## Network Time Delay

Difficulty: _Medium_

**PROBLEM**:

> There are n network nodes labelled 1 to N. Given a times array, containing edges represented by arrays `[ u, v, w ]` where `u` is the source node, `v` is the target node, and `w` is the time taken to travel from the source to the target node. Send a signal from node k, return how long it takes for all nodes to receive the signal. Return -1 if it's impossible.

Example:

```
n = 5    nodes = 1, 2, 3, 4, 5

times = [
  [1, 2, 9],
  [1, 4, 2],
  [2, 5, 1],
  [4, 2, 4],
  [4, 5, 6],
  [3, 2, 3],
  [5, 3, 7],
  [3, 1, 5],
]

k=1
```

**SOLUTION**:

We can use the Dijkstra's algorithm, because it's clearly a weighted graph. If one of the mininum path is Infinity, it means that the graph is not fully connected, so we return -1. In the other cases, we simply apply the algorithm and return the minimum path.

This solution has a time complexity of `O(ElogN)` (E is the number of edges).

```javascript
function solution(times, N, k){
  const distances = new Array(N).fill(Infinity);
  const adjList = distances.map(() => []);
  distances[k-1] = 0;
  const heap = new PriorityQueue((a, b) => distances[a] < distances[b]); // Implementation done in the 01-data-structures-and-algotihms chapter
  heap.push(k-1);
  for(let i = 0; i < times.length i++){
    const source = times[i][0];
    const target = times[i][1];
    const weight = times[i][2];
    adjList[source-1].push([target-1, weight]);
  }
  // Dijkstras algorithm
  while(!heap.isEmpty()){
    const currentVertex = heap.pop();
    const adjacent = adjList[currentVertex];
    for(let i=0; i < adjacent.length; i++){
      const neighboringVertex = adjacent[i][0];
      const weight = adjacent[i][1];
      if(distances[currentVertex] + weight < distances[neighboringVertex]){
        distances[neighboringVertex] = distances[currentVertex] + weight;
        heap.push(neighboringVertex);
      }
    }
  }
  const answer = Math.max(...distances);
  return answer === Infinity ? -1 : answer;
}
```

## Minimum Cost of Climbing Stairs

Difficulty: _Easy_

**PROBLEM**:

> For a given staircase, the i-th step is assigned a non-negative const indicated by a cost array. Once you pay the cost for a step, you can either climb one or two steps. Find the minimum cost to reach the top of the staircase. Your first step can either be the first or second step.

Example:

```
const = [10,15,30]
       _
     _|FINISH
   _|30
 _|15
|10

Here the minumun cost is 15, that we achieve by starting from 15 and take a 2 step to go straight to the finish.
```

**SOLUTION**:

For this problem, we can use a dynamic programming approach.
We want the minimum cost of a recursive function that analyze the current possible path starting from n or n+1. We can store each combination in a map to save time (dynamic programming).
We want a top-down view, so, we start from the end of the array (and continue untile n=0).
I'll build different solutions.

```javascript
// 1 - Recursive Memoizing solution
// Time: O(n) (Without memoizing values, it would be `O(2^2)`!!).
// Space: O(n)
This solution has a time complexity of `O(n)`.
function solution1(cost){
  const n = cost.length;
  const dp = [];
  return Math.min(minCost(n-1, cost, dp), minCost(n-2, cost, dp));
}

function minCost(i, cost, dp){
  if(i<0)
    return 0;
  if(i === 0 || i === 1)
    return cost[i];
  if(dp[i] !== undefined)
    return dp[i];
  dp[i] = cost[i] + Math.min(minCost(i-1, cost, dp), minCost(i-2, cost, dp));
  return dp[i];
}

/* ========================================================= */

// 2 - Iterative Solution
// Time: O(n)
// Space: O(n)
function solution2(cost){
  const dp = [];
  const n = cost.length;
  for(let i = 0; i < n; i++){
    if(i < 2){
      dp[i] = cost[i];
    } else {
      dp[i] = cost[i] + Math.min(dp[i-1], dp[i-2]);
    }
  }
  return Math.min(dp[n-1], dp[n-2]);
}

/* ========================================================= */

// 3 - Optimal Solution
// Time: O(n)
// Space: O(1)
function solution3(cost){
  const n = const.length;
  if(n===0)
    return 0;
  if(n===1)
    return const[0];
  let dpOne = cost[0];
  let dpTwo = cost[1];
  for(let i = 2; i < n; i++){
    const current = cost[i] + Math.min(dpOne, dpTwo);
    dpOne = dpTwo;
    dpTwo = current;
  }
  return Math.min(dpOne, dpTwo);
}
```

## Knight Probability in Chessboard

Difficulty: _Medium_

**PROBLEM**:

> On a Given NxN chessboard, a knight piece will start at the r-th row and c-th column. The knight will attempt to make k moves. A knight can move in 8 possible ways. Each move will choose one of these 8 at random. The knight continues moving until it finishes k moves or it moves off the chessboard. Return the probability that the knight is on the chessboard after it finishes moving.

Example:

```
N=6
r=2, c=2
k=2

• = next move

   0 1 2 3 4 5
0 |_|•|_|•|_|_|
1 |•|_|_|_|•|_|
2 |_|_|♞|_|_|_|
3 |•|_|_|_|•|_|
4 |_|•|_|•|_|_|
5 |_|_|_|_|_|_|

Result: 0.53125
```

**SOLUTION**:

We have to approch the problem with dynamic programming, because we have to explore each case to return a solution which we are sure about. Explornig each case would be an enourmous computational complexity, so we _must_ use dynamic programming. The logic will be similar to the previous problem.
You can get that is a dynamic programming problem becuase you can return on the previous position with a knight move, so you'll have to fiugre out the same probabilities.

```javascript
const directions = [
  [-2, -1],
  [-2, 1],
  [-1, 2],
  [1, 2]
  [2, 1],
  [2, -1],
  [1, -2],
  [-1, -2]
];

// Time: O(8^k)
// Space: O(8^k)
function solution1(N, k, r, c){
  if(k === 0)
    return 1;
  if(r < 0 || r >= N || c < 0 || c >= N)
    return 0;
  let res = 0;
  for(let i=0; i<directions.length; i++){
    const dir = directions[i];
    res += knightProbability(N, k-1, r+dir[0], c+dir[1]) / 8;
  }
  return res;
}

/* ========================================================= */

// Time: O(k*n^2)
// Space: O(k*n^2)
function solution2(N, k, r, c){
  const dp = new Array(k+1).fill(0).map(
    () => new Array(N).fill(0).map(
      () => new Array(N).fill(undefined)
    )
  );
  return recurse(N, k, r, c, dp);
}

function recurse(N, k, r, c, dp){
  if(k === 0)
    return 1;
  if(r < 0 || r >= N || c < 0 || c >= N)
    return 0;
  if(dp[k][r][c] !== undefined)
    return dp[k][r][c];
  let res = 0;
  for(let i=0; i<directions.length; i++){
    const dir = directions[i];
    res = recurse(N, k-1, r+dir[0], c+dir[1], dp) / 8;
  }
  dp[k][r][c] = res;
  return dp[k][r][c];
}

/* ========================================================= */

// Time: O(k*n^2)
// Space: O(n^2)
function solution3(N, k, r, c){
  const prevDp = new Array(N).fill(0).map(() => new Array(N).fill(0));
  const currDp = new Array(N).fill(0).map(() => new Array(N).fill(0));
  prevDp[r][c] = 1;
  for(let step=1; step<=k; step++){
    for(let row=0; row<N; row++){
      for(let col=0; col<N; col++){
        for(let i=0; i<directions.length; i++){
          const dir = directions[i];
          const prevRow = row + dir[0];
          const prevCol = col + dir[1];
          if(prevRow >= 0 && prevRow < N && prevCol >= 0 && prevCol < N){
            currDp[row][col] += prevDp[prevRow][prevCol] / 8;
          }
        }
      }
    }
    prevDp = currDp;
    currDp = new Array(N).fill(0).map(() => new Array(N).fill(0));
  }
  let res = 0;
  for(let i=0; i<N; i++){
    for(let j=0; j<N; j++){
      res += prevDp[i][j]
    }
  }
  return res;
}
```

## Sudoku Solver

Difficulty: _Hard_

**PROBLEM**:

> Create a function that solves for any 9x9 sudoku puzzle given.

Example:

```
It would be crazy to implement
an example for this problem.

I'm sorry...
```

**SOLUTION**:

For this hard problem we can use _Backtracking_, which is a brute force, recursive, solution.

This solution has a time complexity of `O((9!)^9)` (LOL) (With a brute force solution, it would be `O(9^81)` (LOOOOL)).

This solution has a space complexity of `O(1)` (`O(81)`).

```javascript
function getBoxId(row, col){
  const colVal = Math.floor(col / 3);
  const rowVal = Math.floor(row / 3);
  return rowVal + colVal;
}

function isValid(box, row, col, num){
  if(box[num] || row[num] || col[num])
    return false;
  return true;
}

function solveBacktrack(board, boxes, rows, cols, r, c){
  if(r === board.length || c === board[0].length)
    return true;
  if(board[r][c] === '.'){
    for(let num=1; num<=9; num++){
      const numVal = num.toString();
      board[r][c] = numVal;
      const boxId = getBoxId(r,c);
      const box = boxes[boxId];
      const row = rows[r];
      const col = cols[c];
      if(isValid(box, row, col, numVal)){
        box[numVal] = true;
        col[numVal] = true;
        row[numVal] = true;
        if(c === board[0].length-1){
          if(solveBacktrack(board, boxes, rows, cols, r+1, 0))
            return true;
        } else {
          if(solveBacktrack(board, boxes, rows, cols, r, c+1))
            return true;
        }
        delete box[numVal];
        delete row[numVal];
        delete col[numVal];
      }
      board[r][c] = '.';
    }
  } else {
    if(c === board[0].length){
      if(solveBacktrack(board, boxes, rows, cols, r+1, 0))
        return true;
    } else {
      if(solveBacktrack(board, boxes, rows, cols, r, c+1))
        return true;
    }
  }
}

function solution(board){
  const n = board.length;
  const boxes = new Array(n);
  const rows = new Array(n);
  const cols = new Array(n);
  for(let i = 0; i < n; i++){
    boxes[i] = {}
    rows[i] = {};
    cols[i] = {};
  }
  for(let r=0; r < n; r++){
    for(let c=0; c < n; c++){
      if(board[r][c] !== '.'){
        const val = board[r][c];
        const boxId = getBoxId(r,c);
        boxes[boxId][val] = true;
        rows[r][val] = true;
        cols[c][val] = true;
      }
    }
  }
  solveBacktrack(board, boxes, rows, cols, 0, 0);
}
```

## Monarchy

Difficulty: _Medium_

**PROBLEM**:

> Given the following interface, implement its methods.
> ```java
> interface Monarchy {
>   void birth(String child, String parent);
>   void death(String name);
>   List<String> getOrderOfSuccession();
> }
> ```

Example:

```
        ------- Jake -----
      /          |         \
  Catherine     Tom      Celine
    /   \                   |
  Jane  Mark              Peter
   |
Farah

getOrderOfSuccession() 
// returns [Jake, Catherine, Jane, Farah, Mark, Tom, Celine, Peter]

```

**SOLUTION**:

```javascript
class Person {
  constructor(name){
    this.name = name;
    this.isAlive = true;
    this.children = [];
  }
}
class Monarchy {
  constructor(king){
    this.king = new Person(king);
    this._persons = {
      [this.king.name]: this.king
    };
  }
  birth(childName, perentName){
    const parent = this._persons[parentName];
    const newChild = new Person(childName);
    parent.children.push(newChild);
    this._persons[childName] = newChild;
  }
  death(name){
    const person = this._persons[name];
    if(person === undefined)
      return null;
    person.isAlive = false;
  }
  getOrderOfSuccession(){
    const order = [];
    this._dfs(this.king, order);
    return order;
  }
  _dfs(currentPerson, order){
    if(currentPerson.isAlive)
      order.push(currentPerson.name);
    for(let i=0; i<currentPerson.children.length; i++){
      this._dfs(currentPerson.children[i], order);
    }
  }
}
```

## Implement Prefix Trie

Difficulty: _Medium_

**PROBLEM**:

> Implement a trie with insert, search, and startsWith methods.
> ```java
> interface Trie {
>   void insert(String word);
>   Boolean search(String word);
>   Boolean startsWith(String prefix);
> }
> ```

Example:

```
            root
           /  |
          a   d
         /    |
        p     o
       /      |
      p       g
      |
      l
       \
        e 
```

**SOLUTION**:

```javascript
class TrieNode {
  constructor(){
    this.end = false;
    this.keys = {};
  }
}

class Trie {
  constructor(){
    this.root = new TrieNode();
  }
  insert(word, node=this.root){
    if(word.length === 0){
      node.end = true;
      return;
    }
    if(!node.keys[word[0]]){
      node.keys[word[0]] = new TrieNode();
      this.insert(word.substring(1), node.keys[word[0]]);
    } else {
      this.insert(word.substring(1), node.keys[word[0]]);
    }
  }
  search(word, node=this.root){
    if(word.length === 0 && node.end)
      return true;
    if(word.length === 0 && !node.end)
      return false;
    if(!node.keys[word[0]])
      return false;
    this.search(word.substring(1), node.keys[word[0]]);
  }
  startsWith(prefix, node=this.root){
    if(prefix.length === 0)
      return true;
    if(!node.keys[prefix[0]])
      return false;
    this.startsWith(prefix.substring(1), node.keys[prefix[0]]);
  }
}
```

## Solve my psychological problems

Difficulty: _Impossible_

**PROBLEM**:

> Given a list of psychological problems, return true if all the problems are solvable.

Example:

```
['ADHD', 'Low Self Esteem', 'No money', 'Relationships problems']

Returns false
```

**SOLUTION**:

You can simply return false.

```javascript
function solution(problems){
  return false;
}
```


Thank you 🗿🥇