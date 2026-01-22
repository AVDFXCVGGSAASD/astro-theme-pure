---
title: 'Node.js 异步编程实践'
publishDate: '2024-09-22'
description: '深入探讨 Node.js 的异步编程模式，包括回调、Promise、async/await 的使用，以及错误处理和性能优化技巧。'
tags:
  - Node.js
  - 异步编程
  - JavaScript
language: 'zh-CN'
draft: false
---

Node.js 的核心特性之一就是异步非阻塞 I/O。理解并正确使用异步编程模式，对于编写高效的 Node.js 应用至关重要。

## 异步编程的演进

### 回调函数（Callback）

最早的异步模式，但容易导致"回调地狱"。

```javascript
fs.readFile('file1.txt', (err, data1) => {
  if (err) throw err;
  fs.readFile('file2.txt', (err, data2) => {
    if (err) throw err;
    // 处理数据
  });
});
```

### Promise

Promise 提供了更好的异步处理方式。

```javascript
fs.promises.readFile('file1.txt')
  .then(data1 => fs.promises.readFile('file2.txt'))
  .then(data2 => {
    // 处理数据
  })
  .catch(err => {
    console.error(err);
  });
```

### async/await

async/await 让异步代码看起来像同步代码。

```javascript
async function readFiles() {
  try {
    const data1 = await fs.promises.readFile('file1.txt');
    const data2 = await fs.promises.readFile('file2.txt');
    // 处理数据
  } catch (err) {
    console.error(err);
  }
}
```

## 并发控制

### 顺序执行

```javascript
// 逐个执行
for (const url of urls) {
  await fetch(url);
}
```

### 并行执行

```javascript
// 同时执行所有请求
await Promise.all(urls.map(url => fetch(url)));
```

### 限制并发

```javascript
// 限制同时执行的请求数
async function limitConcurrency(tasks, limit) {
  const results = [];
  for (let i = 0; i < tasks.length; i += limit) {
    const batch = tasks.slice(i, i + limit);
    const batchResults = await Promise.all(batch.map(task => task()));
    results.push(...batchResults);
  }
  return results;
}
```

## 错误处理

### Promise 错误处理

```javascript
async function fetchData() {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Fetch error:', error);
    throw error; // 重新抛出或处理
  }
}
```

### 全局错误处理

```javascript
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection at:', promise, 'reason:', reason);
});

process.on('uncaughtException', (error) => {
  console.error('Uncaught Exception:', error);
  process.exit(1);
});
```

## 性能优化

1. **使用流处理大文件**：避免将整个文件加载到内存
2. **合理使用缓存**：减少重复计算和 I/O
3. **批量处理**：合并多个操作
4. **使用 Worker Threads**：处理 CPU 密集型任务

## 总结

异步编程是 Node.js 的核心，掌握不同的异步模式及其适用场景，能够帮助我们编写更高效、更可维护的代码。async/await 让异步代码更易读，但也要注意错误处理和性能优化。