# Git 代码管理完整指南

## 目录
1. [Git 基础概念](#git-基础概念)
2. [初始化和配置](#初始化和配置)
3. [基本工作流程](#基本工作流程)
4. [分支管理](#分支管理)
5. [远程仓库操作](#远程仓库操作)
6. [回退和恢复](#回退和恢复)
7. [常见问题解决](#常见问题解决)
8. [最佳实践](#最佳实践)

---

## Git 基础概念

### 什么是 Git？
Git 是一个分布式版本控制系统，用于跟踪文件的变化历史。

### 核心概念
- **仓库(Repository)**: 存储项目代码和版本历史的地方
- **工作区(Working Directory)**: 你正在编辑的文件所在的目录
- **暂存区(Staging Area)**: 准备提交的文件临时存放区
- **提交(Commit)**: 保存当前状态的快照

### Git 的三种状态
```
工作区 → 暂存区 → 版本库
  ↓        ↓        ↓
已修改   已暂存    已提交
```

---

## 初始化和配置

### 1. 安装 Git
```bash
# Ubuntu/Debian
sudo apt install git

# macOS
brew install git

# Windows
# 下载安装包: https://git-scm.com/
```

### 2. 全局配置
```bash
# 配置用户信息
git config --global user.name "你的名字"
git config --global user.email "你的邮箱@example.com"

# 配置默认分支名
git config --global init.defaultBranch main

# 配置编辑器
git config --global core.editor "code --wait"  # VS Code
# 或
git config --global core.editor "vim"

# 查看配置
git config --list
```

### 3. 项目初始化
```bash
# 在现有项目中初始化
cd your-project
git init

# 或者克隆远程仓库
git clone https://github.com/username/repo.git
```

---

## 基本工作流程

### 1. 查看状态
```bash
# 查看当前状态
git status

# 简洁格式
git status -s
```

### 2. 添加文件到暂存区
```bash
# 添加单个文件
git add filename.py

# 添加多个文件
git add file1.py file2.py

# 添加所有修改的文件
git add .

# 添加所有文件（包括删除的）
git add -A
```

### 3. 提交更改
```bash
# 提交暂存区的文件
git commit -m "提交说明"

# 跳过暂存区直接提交（只对已跟踪文件有效）
git commit -am "提交说明"

# 修改最后一次提交
git commit --amend -m "新的提交说明"
```

### 4. 查看历史
```bash
# 查看提交历史
git log

# 一行显示
git log --oneline

# 查看图形化历史
git log --graph --oneline --all

# 查看具体文件的历史
git log filename.py
```

### 5. 查看差异
```bash
# 查看工作区与暂存区的差异
git diff

# 查看暂存区与最后提交的差异
git diff --staged

# 查看两个提交之间的差异
git diff commit1 commit2
```

---

## 分支管理

### 1. 分支基础操作
```bash
# 查看所有分支
git branch

# 查看远程分支
git branch -r

# 创建新分支
git branch feature-name

# 切换分支
git checkout feature-name

# 创建并切换到新分支
git checkout -b feature-name

# 或使用新命令
git switch feature-name
git switch -c feature-name
```

### 2. 合并分支
```bash
# 切换到主分支
git checkout main

# 合并其他分支
git merge feature-name

# 删除已合并的分支
git branch -d feature-name

# 强制删除分支
git branch -D feature-name
```

### 3. 解决冲突
```bash
# 当合并出现冲突时
# 1. 查看冲突文件
git status

# 2. 编辑冲突文件，解决冲突标记
# <<<<<<< HEAD
# 你的更改
# =======
# 他人的更改
# >>>>>>> branch-name

# 3. 添加解决后的文件
git add conflicted-file

# 4. 完成合并
git commit
```

---

## 远程仓库操作

### 1. 远程仓库管理
```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin https://github.com/username/repo.git

# 修改远程仓库 URL
git remote set-url origin https://github.com/username/newrepo.git

# 删除远程仓库
git remote remove origin
```

### 2. 推送和拉取
```bash
# 第一次推送（建立跟踪关系）
git push -u origin main

# 后续推送
git push

# 推送所有分支
git push --all

# 拉取更新
git pull

# 仅获取更新但不合并
git fetch

# 强制推送（危险操作）
git push --force
```

### 3. GitHub 操作
```bash
# 创建仓库后的标准流程
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/repo.git
git push -u origin main
```

---

## 回退和恢复

### 1. 撤销工作区更改
```bash
# 撤销单个文件的更改
git checkout -- filename

# 撤销所有更改
git checkout -- .

# 或使用新命令
git restore filename
```

### 2. 撤销暂存区更改
```bash
# 取消暂存单个文件
git reset HEAD filename

# 取消所有暂存
git reset HEAD

# 或使用新命令
git restore --staged filename
```

### 3. 回退提交
```bash
# 软回退（保留工作区和暂存区）
git reset --soft HEAD~1

# 混合回退（保留工作区，清空暂存区）
git reset --mixed HEAD~1
# 或
git reset HEAD~1

# 硬回退（清空所有更改）
git reset --hard HEAD~1

# 回退到指定提交
git reset --hard commit-hash
```

### 4. 反向提交
```bash
# 创建一个新提交来撤销指定提交
git revert commit-hash

# 撤销最后一次提交
git revert HEAD
```

### 5. 临时保存更改
```bash
# 保存当前更改
git stash

# 查看保存的更改
git stash list

# 恢复最近的保存
git stash pop

# 恢复指定的保存
git stash apply stash@{0}

# 删除保存
git stash drop stash@{0}
```

---

## 常见问题解决

### 1. 忘记切换分支就开始工作
```bash
# 保存当前更改
git stash

# 切换到正确分支
git checkout correct-branch

# 恢复更改
git stash pop
```

### 2. 提交信息写错了
```bash
# 修改最后一次提交信息
git commit --amend -m "正确的提交信息"
```

### 3. 文件不小心添加到了暂存区
```bash
# 从暂存区移除文件
git reset HEAD filename
```

### 4. 想要忽略某些文件
```bash
# 创建 .gitignore 文件
echo "*.log" >> .gitignore
echo "node_modules/" >> .gitignore
echo ".env" >> .gitignore

# 添加到版本控制
git add .gitignore
git commit -m "添加 .gitignore"
```

### 5. 已经提交的文件想要忽略
```bash
# 从版本控制中删除但保留本地文件
git rm --cached filename

# 添加到 .gitignore
echo "filename" >> .gitignore

# 提交更改
git add .gitignore
git commit -m "忽略 filename"
```

---

## 最佳实践

### 1. 提交信息规范
```bash
# 好的提交信息格式
git commit -m "feat: 添加用户登录功能"
git commit -m "fix: 修复数据库连接错误"
git commit -m "docs: 更新 API 文档"
git commit -m "refactor: 重构用户服务模块"
```

### 2. 分支命名规范
```bash
# 功能分支
git checkout -b feature/user-login
git checkout -b feat/payment-system

# 修复分支
git checkout -b fix/database-connection
git checkout -b hotfix/security-patch

# 发布分支
git checkout -b release/v1.2.0
```

### 3. 工作流程建议
```bash
# 1. 开始新功能前先更新主分支
git checkout main
git pull

# 2. 创建功能分支
git checkout -b feature/new-feature

# 3. 开发过程中定期提交
git add .
git commit -m "实现基础功能"

# 4. 完成后合并到主分支
git checkout main
git merge feature/new-feature

# 5. 删除功能分支
git branch -d feature/new-feature

# 6. 推送更新
git push
```

### 4. 团队协作建议
- **小而频繁的提交**: 每个提交只包含一个逻辑更改
- **描述性的提交信息**: 清楚说明做了什么
- **使用分支**: 为每个功能或修复创建分支
- **代码审查**: 使用 Pull Request 进行代码审查
- **定期同步**: 经常拉取远程更新

### 5. 安全提示
- **永远不要提交敏感信息**: 密码、API密钥等
- **使用 .gitignore**: 忽略不应该版本控制的文件
- **谨慎使用 --force**: 强制推送可能覆盖他人的工作

---

## 快速参考

### 常用命令一览
```bash
# 基础操作
git init                    # 初始化仓库
git status                  # 查看状态
git add .                   # 添加所有文件
git commit -m "message"     # 提交更改
git log --oneline          # 查看历史

# 分支操作
git branch                  # 查看分支
git checkout -b name        # 创建并切换分支
git merge name              # 合并分支
git branch -d name          # 删除分支

# 远程操作
git remote add origin url   # 添加远程仓库
git push -u origin main     # 首次推送
git pull                    # 拉取更新

# 回退操作
git reset --hard HEAD~1     # 硬回退
git stash                   # 暂存更改
git stash pop               # 恢复暂存
```

---

这个指南涵盖了 Git 的核心功能。随着使用经验的积累，你会发现 Git 是一个非常强大的工具。记住：**多练习，多实践，遇到问题时查阅文档或搜索解决方案**。
