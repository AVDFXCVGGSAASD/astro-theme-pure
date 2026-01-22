---
title: 'React Hooks 最佳实践'
publishDate: '2024-07-15'
description: '分享 React Hooks 的使用经验和最佳实践，包括 useState、useEffect、useMemo 等常用 Hook 的正确使用方式，避免常见陷阱。'
tags:
  - React
  - Hooks
  - 前端
language: 'zh-CN'
draft: false
---

React Hooks 自 16.8 版本引入以来，彻底改变了我们编写 React 组件的方式。本文将分享一些 Hooks 的最佳实践，帮助你写出更优雅、更高效的代码。

## useState 的使用

`useState` 是最基础的 Hook，用于管理组件状态。

```typescript
// ✅ 好的做法：使用函数式更新
const [count, setCount] = useState(0);
const increment = () => setCount(prev => prev + 1);

// ❌ 避免：直接依赖状态值
const increment = () => setCount(count + 1);
```

## useEffect 的依赖管理

`useEffect` 用于处理副作用，正确管理依赖数组至关重要。

```typescript
// ✅ 好的做法：明确依赖
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Tick');
  }, 1000);
  
  return () => clearInterval(timer);
}, []); // 空依赖数组表示只在挂载时执行

// ❌ 避免：遗漏依赖
useEffect(() => {
  fetchData(userId); // 如果 userId 变化，需要重新获取
}, []); // 缺少 userId 依赖
```

## useMemo 和 useCallback

这两个 Hook 用于性能优化，但不要过度使用。

```typescript
// ✅ 好的做法：只在计算昂贵时使用
const expensiveValue = useMemo(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);

// ❌ 避免：过度优化简单计算
const simpleValue = useMemo(() => a + b, [a, b]);
```

## 自定义 Hooks

将逻辑抽取到自定义 Hooks 中，提高代码复用性。

```typescript
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);
  
  const decrement = useCallback(() => {
    setCount(prev => prev - 1);
  }, []);
  
  return { count, increment, decrement };
}
```

## 常见陷阱

1. **不要在循环、条件或嵌套函数中调用 Hooks**
2. **确保依赖数组完整**
3. **清理副作用（如定时器、订阅）**

## 总结

Hooks 让 React 函数组件更加强大，但需要遵循其规则和最佳实践。通过合理使用 Hooks，我们可以写出更清晰、更易维护的代码。