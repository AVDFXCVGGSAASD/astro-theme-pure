---
title: 'Git 工作流实践'
publishDate: '2024-08-20'
description: '介绍 Git 工作流的最佳实践，包括分支策略、提交规范、代码审查流程等，帮助团队更好地协作开发。'
tags:
  - Git
  - 版本控制
  - 开发工具
language: 'zh-CN'
draft: false
---

Git 是现代软件开发中不可或缺的工具。掌握良好的 Git 工作流实践，不仅能提高个人效率，还能促进团队协作。

## 分支策略

### Git Flow

Git Flow 是一种流行的分支模型，适合有固定发布周期的项目。

- `main/master`: 主分支，始终保持可发布状态
- `develop`: 开发分支，集成最新功能
- `feature/*`: 功能分支
- `release/*`: 发布分支
- `hotfix/*`: 热修复分支

### GitHub Flow

GitHub Flow 更简单，适合持续部署的项目。

1. 从 `main` 创建功能分支
2. 在分支上开发
3. 创建 Pull Request
4. 代码审查后合并到 `main`
5. 立即部署

## 提交规范

良好的提交信息有助于理解代码变更历史。

```
<type>(<scope>): <subject>

<body>

<footer>
```

类型包括：
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档更新
- `style`: 代码格式
- `refactor`: 重构
- `test`: 测试
- `chore`: 构建/工具变更

示例：
```
feat(auth): 添加用户登录功能

实现了基于 JWT 的用户认证系统，包括登录、登出和 token 刷新功能。

Closes #123
```

## 代码审查

代码审查是保证代码质量的重要环节。

### 审查清单

- [ ] 代码符合项目规范
- [ ] 没有明显的 bug
- [ ] 有适当的测试覆盖
- [ ] 文档已更新
- [ ] 性能考虑合理

### 审查技巧

- 保持建设性的反馈
- 解释为什么需要修改
- 及时响应审查请求
- 尊重他人意见

## 常用命令

```bash
# 创建并切换到新分支
git checkout -b feature/new-feature

# 暂存更改
git stash

# 交互式 rebase
git rebase -i HEAD~3

# 查看提交历史
git log --oneline --graph

# 撤销最后一次提交（保留更改）
git reset --soft HEAD~1
```

## 总结

良好的 Git 工作流实践需要团队协作和持续改进。通过规范的分支策略、清晰的提交信息和有效的代码审查，我们可以提高开发效率和代码质量。