---
title: '微前端架构设计'
publishDate: '2024-11-15'
description: '介绍微前端架构的设计理念和实践方案，包括模块联邦、单应用架构、iframe 方案等，帮助团队构建可扩展的前端应用。'
tags:
  - 微前端
  - 架构设计
  - 前端
language: 'zh-CN'
draft: false
---

微前端是一种将前端应用拆分为多个独立、可部署的小型应用的架构模式。它允许不同团队独立开发、测试和部署各自的前端应用。

## 微前端的优势

1. **技术栈独立**：不同应用可以使用不同的框架
2. **独立部署**：每个应用可以独立发布
3. **团队自治**：不同团队可以独立开发
4. **增量升级**：可以逐步升级技术栈

## 实现方案

### 1. 模块联邦（Module Federation）

Webpack 5 的模块联邦是最现代的微前端方案。

```javascript
// 主应用 webpack.config.js
module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'host',
      remotes: {
        remoteApp: 'remote@http://localhost:3001/remoteEntry.js',
      },
    }),
  ],
};

// 远程应用 webpack.config.js
module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'remote',
      filename: 'remoteEntry.js',
      exposes: {
        './Button': './src/Button',
      },
    }),
  ],
};
```

### 2. 单应用架构（Single-SPA）

Single-SPA 是一个用于构建微前端的 JavaScript 框架。

```javascript
import { registerApplication, start } from 'single-spa';

registerApplication({
  name: 'app1',
  app: () => System.import('app1'),
  activeWhen: '/app1',
});

registerApplication({
  name: 'app2',
  app: () => System.import('app2'),
  activeWhen: '/app2',
});

start();
```

### 3. iframe 方案

最简单的微前端实现方式，但存在一些限制。

```html
<iframe src="http://localhost:3001" frameborder="0"></iframe>
```

## 路由管理

微前端需要统一的路由管理。

```javascript
// 主应用路由
const routes = [
  {
    path: '/app1/*',
    component: () => import('./App1'),
  },
  {
    path: '/app2/*',
    component: () => import('./App2'),
  },
];
```

## 状态管理

不同应用之间可能需要共享状态。

```javascript
// 全局状态管理
class GlobalStore {
  constructor() {
    this.state = {};
    this.listeners = [];
  }
  
  setState(newState) {
    this.state = { ...this.state, ...newState };
    this.listeners.forEach(listener => listener(this.state));
  }
  
  subscribe(listener) {
    this.listeners.push(listener);
    return () => {
      this.listeners = this.listeners.filter(l => l !== listener);
    };
  }
}

export const globalStore = new GlobalStore();
```

## 样式隔离

避免不同应用的样式冲突。

```javascript
// 使用 Shadow DOM
class MicroApp extends HTMLElement {
  connectedCallback() {
    const shadow = this.attachShadow({ mode: 'open' });
    shadow.innerHTML = `
      <style>
        /* 样式只作用于 Shadow DOM 内部 */
      </style>
      <div>应用内容</div>
    `;
  }
}
```

## 最佳实践

1. **统一技术规范**：虽然可以使用不同框架，但需要统一规范
2. **共享依赖**：避免重复打包相同依赖
3. **错误边界**：隔离错误，避免影响其他应用
4. **性能监控**：监控每个应用的性能

## 总结

微前端架构适合大型团队和复杂应用。选择合适的方案需要考虑团队规模、技术栈、部署环境等因素。模块联邦是目前最推荐的方案，它提供了良好的开发体验和性能。