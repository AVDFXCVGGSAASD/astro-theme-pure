---
title: '算法与数据结构基础'
publishDate: '2025-01-08'
description: '介绍常用的算法和数据结构，包括数组、链表、栈、队列、树、图等，以及排序、搜索等基础算法，帮助提升编程能力。'
tags:
  - 算法
  - 数据结构
  - 编程
language: 'zh-CN'
draft: false
---

算法和数据结构是编程的基础。掌握常用的算法和数据结构，能够帮助我们写出更高效、更优雅的代码。

## 数据结构

### 数组（Array）

数组是最基本的数据结构，支持随机访问。

```javascript
const arr = [1, 2, 3, 4, 5];
// 访问：O(1)
// 插入/删除：O(n)
```

### 链表（Linked List）

链表适合频繁插入和删除的场景。

```javascript
class ListNode {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

// 访问：O(n)
// 插入/删除：O(1)
```

### 栈（Stack）

后进先出（LIFO）的数据结构。

```javascript
class Stack {
  constructor() {
    this.items = [];
  }
  
  push(item) {
    this.items.push(item);
  }
  
  pop() {
    return this.items.pop();
  }
  
  peek() {
    return this.items[this.items.length - 1];
  }
}
```

### 队列（Queue）

先进先出（FIFO）的数据结构。

```javascript
class Queue {
  constructor() {
    this.items = [];
  }
  
  enqueue(item) {
    this.items.push(item);
  }
  
  dequeue() {
    return this.items.shift();
  }
}
```

### 树（Tree）

树是重要的非线性数据结构。

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

// 前序遍历
function preorder(root) {
  if (!root) return;
  console.log(root.val);
  preorder(root.left);
  preorder(root.right);
}
```

## 排序算法

### 快速排序

平均时间复杂度 O(n log n)。

```javascript
function quickSort(arr) {
  if (arr.length <= 1) return arr;
  
  const pivot = arr[Math.floor(arr.length / 2)];
  const left = arr.filter(x => x < pivot);
  const middle = arr.filter(x => x === pivot);
  const right = arr.filter(x => x > pivot);
  
  return [...quickSort(left), ...middle, ...quickSort(right)];
}
```

### 归并排序

稳定的排序算法，时间复杂度 O(n log n)。

```javascript
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  
  return merge(left, right);
}

function merge(left, right) {
  const result = [];
  let i = 0, j = 0;
  
  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      result.push(left[i++]);
    } else {
      result.push(right[j++]);
    }
  }
  
  return result.concat(left.slice(i)).concat(right.slice(j));
}
```

## 搜索算法

### 二分搜索

适用于有序数组，时间复杂度 O(log n)。

```javascript
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] === target) {
      return mid;
    } else if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  
  return -1;
}
```

### 深度优先搜索（DFS）

```javascript
function dfs(node, target) {
  if (!node) return false;
  if (node.val === target) return true;
  
  return dfs(node.left, target) || dfs(node.right, target);
}
```

### 广度优先搜索（BFS）

```javascript
function bfs(root, target) {
  const queue = [root];
  
  while (queue.length > 0) {
    const node = queue.shift();
    if (node.val === target) return true;
    
    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
  }
  
  return false;
}
```

## 动态规划

动态规划是解决重叠子问题的有效方法。

```javascript
// 斐波那契数列
function fibonacci(n) {
  const dp = [0, 1];
  
  for (let i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }
  
  return dp[n];
}
```

## 总结

算法和数据结构是编程的基础技能。通过学习和实践，我们可以：

- 写出更高效的代码
- 解决复杂的编程问题
- 提升代码质量和可维护性
- 在面试和实际工作中更有优势

持续练习和思考，是掌握算法和数据结构的关键。