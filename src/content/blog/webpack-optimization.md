---
title: 'Webpack 打包优化实践'
publishDate: '2024-10-28'
description: '分享 Webpack 打包优化的实用技巧，包括代码分割、Tree Shaking、缓存策略等，帮助减小打包体积和提升构建速度。'
tags:
  - Webpack
  - 构建工具
  - 前端
language: 'zh-CN'
draft: false
---

Webpack 是现代前端开发中最重要的构建工具之一。优化 Webpack 配置可以显著提升构建速度和减小打包体积。

## 代码分割

代码分割是减少初始加载时间的关键。

```javascript
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10,
        },
        common: {
          minChunks: 2,
          priority: 5,
          reuseExistingChunk: true,
        },
      },
    },
  },
};
```

## Tree Shaking

Tree Shaking 可以移除未使用的代码。

```javascript
// package.json
{
  "sideEffects": false
}

// webpack.config.js
module.exports = {
  mode: 'production', // 生产模式自动启用 Tree Shaking
};
```

## 缓存策略

利用缓存可以加快重复构建速度。

```javascript
module.exports = {
  cache: {
    type: 'filesystem',
    buildDependencies: {
      config: [__filename],
    },
  },
  optimization: {
    moduleIds: 'deterministic',
    runtimeChunk: 'single',
  },
};
```

## 性能优化

### 使用多进程构建

```javascript
const TerserPlugin = require('terser-webpack-plugin');

module.exports = {
  optimization: {
    minimizer: [
      new TerserPlugin({
        parallel: true,
        terserOptions: {
          compress: {
            drop_console: true,
          },
        },
      }),
    ],
  },
};
```

### 减少解析范围

```javascript
module.exports = {
  resolve: {
    modules: ['node_modules'],
    extensions: ['.js', '.json'],
    alias: {
      '@': path.resolve(__dirname, 'src'),
    },
  },
};
```

## 资源优化

### 图片优化

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|jpeg|gif)$/i,
        type: 'asset',
        parser: {
          dataUrlCondition: {
            maxSize: 8 * 1024, // 8kb
          },
        },
      },
    ],
  },
};
```

### 压缩 CSS

```javascript
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');

module.exports = {
  optimization: {
    minimizer: [
      new CssMinimizerPlugin(),
    ],
  },
};
```

## 开发体验优化

### 热模块替换（HMR）

```javascript
module.exports = {
  devServer: {
    hot: true,
  },
};
```

### Source Map

```javascript
module.exports = {
  devtool: 'source-map', // 开发环境
  // devtool: 'hidden-source-map', // 生产环境
};
```

## 分析工具

使用分析工具了解打包情况。

```javascript
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  plugins: [
    new BundleAnalyzerPlugin(),
  ],
};
```

## 总结

Webpack 优化是一个持续的过程，需要根据项目实际情况调整配置。通过代码分割、Tree Shaking、缓存等策略，我们可以显著提升构建性能和用户体验。