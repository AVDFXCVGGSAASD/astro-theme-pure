---
title: 'CSS Grid 布局实战'
publishDate: '2024-10-10'
description: '深入介绍 CSS Grid 布局系统，包括基本概念、常用属性、实战案例等，帮助开发者掌握现代 CSS 布局技术。'
tags:
  - CSS
  - Grid
  - 前端
language: 'zh-CN'
draft: false
---

CSS Grid 是强大的二维布局系统，它让我们能够以更直观、更灵活的方式创建复杂的网页布局。

## Grid 基础

Grid 布局将容器分为行和列，形成网格。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: auto;
  gap: 20px;
}
```

## 常用属性

### grid-template-columns / grid-template-rows

定义网格的行和列。

```css
/* 固定宽度 */
grid-template-columns: 200px 200px 200px;

/* 使用 fr 单位 */
grid-template-columns: 1fr 2fr 1fr;

/* 使用 repeat() */
grid-template-columns: repeat(3, 1fr);

/* 使用 auto-fit */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
```

### gap

设置网格间距。

```css
gap: 20px; /* 行和列间距相同 */
row-gap: 20px;
column-gap: 30px;
```

### grid-area

定义网格项的位置。

```css
.item {
  grid-column: 1 / 3; /* 从第1列到第3列 */
  grid-row: 1 / 2;    /* 从第1行到第2行 */
  
  /* 或使用 grid-area */
  grid-area: 1 / 1 / 2 / 3;
}
```

## 实战案例

### 响应式卡片布局

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}
```

### 经典布局（Header, Sidebar, Main, Footer）

```css
.layout {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 200px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }
```

### 杂志式布局

```css
.magazine {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(200px, auto);
  gap: 15px;
}

.featured {
  grid-column: span 2;
  grid-row: span 2;
}
```

## Grid vs Flexbox

- **Grid**: 二维布局，适合整体页面布局
- **Flexbox**: 一维布局，适合组件内部布局

两者可以结合使用，Grid 用于整体结构，Flexbox 用于组件内部。

## 浏览器支持

现代浏览器都支持 CSS Grid，对于旧浏览器可以使用 `@supports` 提供降级方案。

```css
@supports (display: grid) {
  .container {
    display: grid;
  }
}
```

## 总结

CSS Grid 提供了强大的布局能力，让我们能够更轻松地创建复杂的响应式布局。掌握 Grid 的关键属性和使用场景，可以显著提升开发效率。