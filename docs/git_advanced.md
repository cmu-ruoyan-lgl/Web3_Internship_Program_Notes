```markdown
# Git 进阶操作学习笔记

## 目录
1. [交互式暂存 (Interactive Staging)](#1-交互式暂存-interactive-staging)
2. [Git Stash 临时存储](#2-git-stash-临时存储)
3. [Git Rebase 变基](#3-git-rebase-变基)
4. [Git Cherry-Pick 拣选提交](#4-git-cherry-pick-拣选提交)
5. [综合操作示例](#5-综合操作示例)

---

### 1. 交互式暂存 (Interactive Staging)
**作用**：精细控制提交内容，逐块选择更改  
**命令**：
```bash
git add -p
```

**常用操作选项**：
| 选项 | 功能说明               |
|------|------------------------|
| `y`  | 暂存当前块             |
| `n`  | 跳过当前块             |
| `s`  | 拆分当前块为更小片段   |
| `e`  | 手动编辑当前块         |
| `q`  | 退出交互式暂存模式     |

---

### 2. Git Stash 临时存储
**作用**：临时保存工作进度，方便切换任务

| 命令                          | 功能说明                     |
|-------------------------------|------------------------------|
| `git stash`                   | 保存当前工作进度             |
| `git stash list`              | 查看所有存储的进度栈         |
| `git stash apply`             | 应用最近一次存储的进度       |
| `git stash pop`               | 应用并删除最近一次存储的进度 |
| `git stash drop stash@{n}`    | 删除指定存储（n为栈索引号）  |
| `git stash clear`             | 清空所有存储进度             |

---

### 3. Git Rebase 变基
**作用**：将分支更改移到另一分支顶部，保持提交历史线性

#### 基础变基
```bash
git rebase <目标分支>
# 示例：将当前分支变基到 main
git rebase main
```

#### 交互式变基（改写提交历史）
```bash
git rebase -i <起始commit>
# 示例：修改最近3个提交
git rebase -i HEAD~3
```

**交互式选项**：
| 命令       | 功能说明                             |
|------------|--------------------------------------|
| `pick`     | 保留提交（默认）                     |
| `reword`   | 修改提交信息                         |
| `edit`     | 暂停以编辑提交内容                   |
| `squash`   | 合并到前一个提交（保留提交信息）     |
| `fixup`    | 合并到前一个提交（丢弃当前提交信息） |
| `drop`     | 删除该提交                           |

---

### 4. Git Cherry-Pick 拣选提交
**作用**：将特定提交应用到当前分支

#### 基础用法
```bash
git cherry-pick <commit-hash>
# 示例：应用提交 abc123
git cherry-pick abc123
```

#### 冲突处理流程
1. 解决冲突后标记为已解决：
   ```bash
   git add <冲突文件>
   ```
2. 继续拣选操作：
   ```bash
   git cherry-pick --continue
   ```

---

### 5. 综合操作示例
```bash
# 1. 精细暂存修改
git add -p

# 2. 临时保存未完成的工作
git stash

# 3. 查看存储列表
git stash list

# 4. 恢复工作进度
git stash apply

# 5. 变基到 main 分支
git rebase main

# 6. 交互式修改最近3个提交
git rebase -i HEAD~3
# 在编辑器中执行：squash 合并提交/reword 修改信息

# 7. 从 feature 分支移植特定提交
git checkout main
git cherry-pick abc123
```
### 6.scope

```bash
feat: 新功能、新特性
fix: 修改 bug
perf: 更改代码，以提高性能（在不影响代码内部行为的前提下，对程序性能进行优化）
refactor: 代码重构（重构，在不影响代码内部行为、功能下的代码修改）
docs: 文档修改
style: 代码格式修改, 注意不是 css 修改（例如分号修改）
test: 测试用例新增、修改
build: 影响项目构建或依赖项修改
revert: 恢复上一次提交
ci: 持续集成相关文件修改
chore: 其他修改（不在上述类型中的修改）
release: 发布新版本
workflow: 工作流相关文件修改
```