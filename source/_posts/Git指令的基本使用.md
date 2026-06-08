---
title: Git指令的基本使用
date: 2026-06-08 21:14:34
cover: /images/20260608.jpg 
tags:
    - 技术
categories:
    - Let's go
feature: true
---
# Git 完全指南

> 从零开始，系统掌握 Git 的核心概念与常用操作。

---

## 目录

1. [Git 是什么](#1-git-是什么)
2. [安装与初始配置](#2-安装与初始配置)
3. [核心概念：四个区域](#3-核心概念四个区域)
4. [初始化仓库](#4-初始化仓库)
5. [记录变更：add 与 commit](#5-记录变更add-与-commit)
6. [查看状态与历史](#6-查看状态与历史)
7. [分支管理](#7-分支管理)
8. [合并与变基](#8-合并与变基)
9. [远程仓库](#9-远程仓库)
10. [撤销与回退](#10-撤销与回退)
11. [标签管理](#11-标签管理)
12. [实用技巧](#12-实用技巧)
13. [常见工作流](#13-常见工作流)
14. [常见问题与解决](#14-常见问题与解决)

---

## 1. Git 是什么

Git 是一个**分布式版本控制系统**，由 Linus Torvalds 于 2005 年为 Linux 内核开发而创建。

### 版本控制解决什么问题？

- 记录文件的每一次修改，可以随时回到任意历史版本
- 多人协作时，能合并不同人的改动，管理冲突
- 每个人都有完整的仓库副本，不依赖中央服务器也能工作

### Git 与 GitHub 的区别

| | Git | GitHub |
|---|---|---|
| 本质 | 本地运行的版本控制工具 | 基于 Git 的代码托管平台（网站） |
| 离线使用 | ✅ 可以 | ❌ 需要联网 |
| 替代品 | Mercurial、SVN | GitLab、Gitee、Bitbucket |

---

## 2. 安装与初始配置

### 安装

```bash
# macOS（推荐通过 Homebrew）
brew install git

# Ubuntu / Debian
sudo apt install git

# Windows
# 下载 Git for Windows: https://git-scm.com/download/win
```

验证安装：

```bash
git --version
# 输出类似：git version 2.43.0
```

### 初始配置

在首次使用前，需要告诉 Git 你是谁。这些信息会记录在每次提交里。

```bash
# 设置用户名和邮箱（全局生效）
git config --global user.name "张三"
git config --global user.email "zhangsan@example.com"

# 设置默认编辑器（可选，默认是 vim）
git config --global core.editor "code --wait"   # VS Code
git config --global core.editor "nano"          # nano

# 设置默认分支名为 main（推荐，与 GitHub 一致）
git config --global init.defaultBranch main

# 查看所有配置
git config --list

# 查看某项配置
git config user.name
```

### 配置文件的位置

| 范围 | 文件位置 | 说明 |
|---|---|---|
| 系统级 | `/etc/gitconfig` | 对所有用户生效 |
| 全局级 | `~/.gitconfig` | 对当前用户生效 |
| 仓库级 | `.git/config` | 只对当前仓库生效，优先级最高 |

---

## 3. 核心概念：四个区域

理解这四个区域是用好 Git 的基础：

```
工作区 (Working Directory)
    │  git add
    ▼
暂存区 (Staging Area / Index)
    │  git commit
    ▼
本地仓库 (Local Repository)
    │  git push
    ▼
远程仓库 (Remote Repository)
```

- **工作区**：你实际看到和编辑的文件目录
- **暂存区**：一个临时区域，存放即将提交的改动（准备好的「快照草稿」）
- **本地仓库**：`.git` 目录，存储完整的历史记录
- **远程仓库**：托管在服务器上的仓库，如 GitHub

### 文件的四种状态

```
未追踪 (Untracked)   ──git add──▶  已暂存 (Staged)
已修改 (Modified)    ──git add──▶  已暂存 (Staged)
已提交 (Committed)   ──修改文件──▶  已修改 (Modified)
已暂存 (Staged)      ──git commit──▶ 已提交 (Committed)
```

---

## 4. 初始化仓库

### 方式一：在本地新建仓库

```bash
mkdir my-project
cd my-project
git init

# 输出：Initialized empty Git repository in /path/to/my-project/.git/
```

这会在当前目录创建一个隐藏的 `.git` 文件夹，里面存储了所有的版本信息。**不要手动修改或删除 `.git` 目录。**

### 方式二：克隆已有仓库

```bash
# 克隆到当前目录下的同名文件夹
git clone https://github.com/user/repo.git

# 克隆到指定文件夹名
git clone https://github.com/user/repo.git my-folder

# 通过 SSH 克隆（需要配置 SSH Key，推送时不需要输密码）
git clone git@github.com:user/repo.git

# 只克隆最近 1 次提交（浅克隆，速度更快，适合大型仓库）
git clone --depth 1 https://github.com/user/repo.git
```

---

## 5. 记录变更：add 与 commit

### git add — 加入暂存区

```bash
# 添加单个文件
git add README.md

# 添加多个文件
git add file1.txt file2.txt

# 添加整个目录
git add src/

# 添加所有改动（包括新文件、修改、删除）
git add .
git add -A    # 与 git add . 效果相同（Git 2.x）

# 交互式选择改动（按代码块精细控制，非常实用）
git add -p
# 进入交互模式后：
#   y = 暂存这个改动块
#   n = 跳过
#   s = 把当前块再拆分成更小的块
#   q = 退出
```

### .gitignore — 忽略文件

在仓库根目录创建 `.gitignore` 文件，列出不想追踪的文件或目录：

```gitignore
# 忽略所有 .log 文件
*.log

# 忽略 node_modules 目录
node_modules/

# 忽略 build 目录
/build

# 忽略某个具体文件
config/secrets.yml

# 但不忽略这个例外
!important.log

# 忽略所有 .env 文件（但不忽略 .env.example）
.env
!.env.example
```

> 💡 **注意**：`.gitignore` 只能忽略**尚未被追踪**的文件。如果文件已经被 `git add` 过，需要先用 `git rm --cached <file>` 取消追踪。

常见语言的 `.gitignore` 模板：[github.com/github/gitignore](https://github.com/github/gitignore)

### git commit — 提交

```bash
# 提交并写提交信息（会打开编辑器）
git commit

# 直接在命令行写提交信息
git commit -m "feat: 添加用户登录功能"

# 跳过 git add，直接提交所有已追踪文件的改动
# （不包含新增的未追踪文件）
git commit -am "fix: 修复按钮点击无响应的问题"

# 修改最后一次提交（信息写错了或漏加文件）
# 注意：只能修改还没推送到远程的提交
git commit --amend -m "feat: 添加用户登录功能（修正）"
git commit --amend --no-edit   # 不修改信息，只补充文件
```

### 好的提交信息规范

推荐使用 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

```
<类型>(<范围>): <简短描述>

<详细说明（可选）>

<关联 Issue（可选）>
```

常用类型：

| 类型 | 用途 |
|---|---|
| `feat` | 新增功能 |
| `fix` | 修复 bug |
| `docs` | 文档修改 |
| `style` | 格式调整（不影响逻辑） |
| `refactor` | 代码重构 |
| `test` | 增加测试 |
| `chore` | 构建流程、工具变更 |

示例：
```
feat(auth): 添加 OAuth2 第三方登录支持

支持微信、GitHub 两种登录方式。
Token 存储在 Redis，有效期 7 天。

Closes #42
```

---

## 6. 查看状态与历史

### git status — 查看当前状态

```bash
git status

# 简洁模式（更紧凑）
git status -s
# 输出示例：
#  M README.md     （M = Modified，前面空格表示在工作区，第二列表示暂存区）
# MM src/app.js   （两个 M = 工作区和暂存区都有改动）
# A  new.txt      （A = Added，已加入暂存区的新文件）
# ?? config.bak   （?? = Untracked，未追踪文件）
```

### git diff — 查看具体改动

```bash
# 对比工作区 vs 暂存区（尚未 git add 的改动）
git diff

# 对比暂存区 vs 最新提交（已 git add 但未 commit 的改动）
git diff --staged
git diff --cached    # 与上面等价

# 对比两个提交之间的差异
git diff abc123 def456

# 只看哪些文件有改动，不看具体内容
git diff --name-only

# 对比工作区与某个提交
git diff HEAD~2
```

### git log — 查看提交历史

```bash
# 默认格式（详细）
git log

# 每条提交一行（最常用）
git log --oneline

# 带分支图形（查看分支结构非常直观）
git log --oneline --graph --all

# 查看最近 5 条
git log -5

# 查看某个文件的改动历史
git log --oneline -- README.md

# 查看某个文件每次提交的具体改动
git log -p README.md

# 按作者过滤
git log --author="张三"

# 按时间过滤
git log --since="2024-01-01" --until="2024-12-31"

# 按提交信息关键词过滤
git log --grep="fix"

# 自定义格式（非常灵活）
git log --pretty=format:"%h %ad %s" --date=short
# %h = 短 hash，%ad = 提交日期，%s = 提交信息首行
```

### git show — 查看某次提交

```bash
# 查看最新提交的详细内容
git show

# 查看指定提交
git show abc123

# 查看某提交中的某个文件内容
git show abc123:src/main.js

# 只显示提交信息，不显示 diff
git show abc123 --stat
```

### git blame — 查看每行的作者

```bash
# 查看文件每行最后一次修改是谁做的
git blame README.md

# 只看某几行（第 10 到 20 行）
git blame -L 10,20 README.md

# 忽略空白字符的改动
git blame -w README.md
```

---

## 7. 分支管理

分支是 Git 最强大的特性之一。本质上，一个分支就是一个指向某个提交的指针，创建和切换分支的开销极低。

### git branch — 查看与管理分支

```bash
# 查看所有本地分支（当前分支前有 * 号）
git branch

# 查看所有分支，含远程分支
git branch -a

# 查看各分支最新的提交信息
git branch -v

# 查看哪些分支已经合并到当前分支（可以安全删除）
git branch --merged

# 查看哪些分支还没有合并
git branch --no-merged

# 创建新分支（不切换过去）
git branch feature/login

# 删除分支（已合并的分支）
git branch -d feature/login

# 强制删除分支（未合并也删除，谨慎使用）
git branch -D feature/login

# 重命名当前分支
git branch -m new-name
```

### git switch — 切换分支（推荐）

`git switch` 是 Git 2.23 引入的新命令，语义比 `git checkout` 更清晰。

```bash
# 切换到已有分支
git switch main

# 创建并切换到新分支
git switch -c feature/user-profile

# 切换到上一个分支（-）
git switch -

# 从指定提交创建新分支
git switch -c hotfix/bug123 abc1234
```

### git checkout（旧写法，仍然有效）

```bash
git checkout main
git checkout -b feature/login     # 创建并切换
git checkout -                    # 切换到上一个分支
```

---

## 8. 合并与变基

### git merge — 合并分支

```bash
# 把 feature/login 合并到当前分支
git switch main
git merge feature/login

# 合并时强制生成一个合并提交（即使可以 fast-forward）
# 有助于保留「功能分支」的痕迹
git merge --no-ff feature/login

# 把另一个分支的改动压缩成一个提交再合并（整洁但丢失历史）
git merge --squash feature/login
git commit -m "feat: 完成登录功能"
```

**Fast-forward vs Non-fast-forward：**

```
Fast-forward（默认，历史是线性的）：
main:    A─B
feature:     C─D
合并后：  A─B─C─D  （main 直接移动到 D）

No-fast-forward（保留分支结构）：
合并后：  A─B─────M  （M 是合并提交）
              └─C─D─┘
```

### 解决合并冲突

当两个分支修改了同一文件的同一位置，就会产生冲突：

```bash
git merge feature/login
# 输出：CONFLICT (content): Merge conflict in README.md
# Automatic merge failed; fix conflicts and then commit the result.
```

打开冲突文件，会看到：

```
<<<<<<< HEAD
这是当前分支（main）的内容
=======
这是要合并进来的分支（feature/login）的内容
>>>>>>> feature/login
```

解决步骤：

```bash
# 1. 手动编辑文件，删除冲突标记，保留正确内容
# 2. 标记为已解决
git add README.md

# 3. 完成合并提交
git commit

# 如果想放弃合并
git merge --abort
```

### git rebase — 变基

变基会把当前分支的提交「移植」到目标分支的最新提交之后，使历史记录保持线性。

```bash
# 将当前分支变基到 main
git rebase main

# 交互式变基：整理最近 3 个提交（合并、重写、删除）
git rebase -i HEAD~3

# 变基时遇到冲突：
# 1. 解决冲突文件
# 2. git add <file>
# 3. git rebase --continue
# 放弃变基：
git rebase --abort
```

**交互式变基的操作选项：**

```
pick   abc123  feat: 添加登录页面     ← 保留
squash def456  fix: 修复样式问题       ← 合并到上一个提交
reword ghi789  docs: 更新文档         ← 重新写提交信息
drop   jkl012  WIP: 临时保存          ← 删除这个提交
```

### merge vs rebase 怎么选？

| | merge | rebase |
|---|---|---|
| 历史记录 | 保留分支结构，有合并提交 | 线性历史，更整洁 |
| 安全性 | 安全，不修改已有提交 | **不要对已推送的分支用** |
| 适用场景 | 合并功能分支到主分支 | 整理本地提交后再推送 |

---

## 9. 远程仓库

### git remote — 管理远程地址

```bash
# 查看远程仓库（-v 显示地址）
git remote -v
# 输出示例：
# origin  https://github.com/user/repo.git (fetch)
# origin  https://github.com/user/repo.git (push)

# 添加远程仓库（origin 是约定俗成的默认名称）
git remote add origin https://github.com/user/repo.git

# 修改远程地址（比如从 HTTPS 换到 SSH）
git remote set-url origin git@github.com:user/repo.git

# 删除远程仓库关联
git remote remove origin

# 重命名远程
git remote rename origin upstream
```

### git push — 推送

```bash
# 首次推送，并建立本地分支与远程分支的追踪关系
git push -u origin main

# 之后可以直接
git push

# 推送指定分支
git push origin feature/login

# 推送所有本地分支
git push --all origin

# 删除远程分支
git push origin --delete feature/old-feature

# 强制推送（危险！会覆盖远程历史，慎用）
git push --force
# 更安全的强制推送：如果远程有新提交则拒绝
git push --force-with-lease
```

### git fetch — 获取远程信息

```bash
# 获取所有远程仓库的最新信息（不自动合并）
git fetch

# 获取指定远程
git fetch origin

# 获取后查看远程新提交
git log origin/main --oneline

# 手动合并
git merge origin/main
```

### git pull — 拉取并合并

`git pull` = `git fetch` + `git merge`

```bash
# 拉取并合并（默认）
git pull

# 拉取并用 rebase 代替 merge（历史更整洁）
git pull --rebase

# 拉取指定远程分支
git pull origin main

# 只拉取不合并（等价于 git fetch）
git fetch origin
```

---

## 10. 撤销与回退

### git restore — 丢弃改动（推荐，Git 2.23+）

```bash
# 丢弃工作区中某个文件的修改（不可逆！）
git restore README.md

# 丢弃所有工作区改动
git restore .

# 将文件从暂存区撤出（回到工作区，改动保留）
git restore --staged README.md

# 从指定提交恢复文件
git restore --source abc123 README.md
```

### git reset — 回退提交

`git reset` 移动 HEAD 指针，有三种模式：

```bash
# --soft：撤销提交，改动保留在暂存区
git reset --soft HEAD~1

# --mixed（默认）：撤销提交，改动保留在工作区
git reset HEAD~1
git reset --mixed HEAD~1

# --hard：撤销提交，改动完全丢弃（危险！）
git reset --hard HEAD~1

# 回退到指定提交
git reset --hard abc123

# HEAD~N 表示往前 N 个提交
# HEAD~1 = 上一个提交
# HEAD~3 = 往前 3 个提交
```

**使用场景：**

| 场景 | 推荐命令 |
|---|---|
| 提交信息写错了 | `git commit --amend` |
| 想把最后几个提交合并后重新提交 | `git reset --soft HEAD~N` |
| 代码写崩了想全部放弃 | `git reset --hard HEAD` |
| 撤销已推送的提交 | `git revert` （见下文） |

### git revert — 安全撤销（适合已推送的提交）

`git revert` 创建一个新提交来「反做」指定提交，不改变已有历史，适合团队协作。

```bash
# 撤销最新提交
git revert HEAD

# 撤销指定提交
git revert abc123

# 撤销但不自动提交（可以先检查再提交）
git revert -n abc123

# 撤销一个范围内的提交
git revert HEAD~3..HEAD
```

### git cherry-pick — 挑选提交

把某个分支上的某个（或某几个）提交应用到当前分支：

```bash
# 挑选单个提交
git cherry-pick abc123

# 挑选多个提交
git cherry-pick abc123 def456

# 挑选一段范围的提交
git cherry-pick abc123..def456

# 挑选但不自动提交
git cherry-pick -n abc123
```

---

## 11. 标签管理

标签（Tag）通常用于标记发布版本。

```bash
# 查看所有标签
git tag

# 查找特定标签
git tag -l "v1.*"

# 创建轻量标签（只是个指针）
git tag v1.0.0

# 创建附注标签（推荐，包含作者、日期、信息）
git tag -a v1.0.0 -m "第一个正式版本"

# 给历史提交打标签
git tag -a v0.9.0 abc123 -m "预发布版本"

# 查看标签详情
git show v1.0.0

# 推送单个标签到远程
git push origin v1.0.0

# 推送所有标签到远程
git push origin --tags

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0

# 从标签创建分支（比如修复旧版本的 bug）
git switch -c hotfix/v1.0.1 v1.0.0
```

---

## 12. 实用技巧

### git stash — 临时储存

在需要临时切换分支但当前工作还没完成时非常有用：

```bash
# 储存当前改动（工作区 + 暂存区）
git stash

# 储存并添加描述
git stash push -m "登录功能开发到一半"

# 查看储存列表
git stash list
# 输出：
# stash@{0}: On feature/login: 登录功能开发到一半
# stash@{1}: WIP on main: abc1234 fix: 修复问题

# 恢复最近一次储存（并从列表中删除）
git stash pop

# 恢复指定储存（保留在列表中）
git stash apply stash@{1}

# 删除指定储存
git stash drop stash@{0}

# 清空所有储存
git stash clear

# 把储存中的改动创建为新分支
git stash branch new-branch stash@{0}
```

### git reflog — 找回「丢失」的提交

`reflog` 记录了 HEAD 的所有移动历史，即使提交被 reset 删除了也能找回：

```bash
# 查看 HEAD 的移动历史
git reflog

# 输出示例：
# abc123 HEAD@{0}: reset: moving to HEAD~1
# def456 HEAD@{1}: commit: feat: 添加了重要功能  ← 刚才 reset 掉的提交
# ...

# 恢复到那个「丢失」的提交
git reset --hard def456
```

### git bisect — 二分查找 bug

当你知道某个版本有 bug，但不知道是哪次提交引入的，可以用二分法快速定位：

```bash
git bisect start

# 标记当前版本有 bug
git bisect bad

# 标记某个已知没问题的旧版本
git bisect good v1.0.0

# Git 会自动切到中间某个提交，测试后标记：
git bisect good    # 这个版本没问题
git bisect bad     # 这个版本有问题

# 直到找到第一个「bad」提交，结束后：
git bisect reset
```

### git shortlog — 统计贡献

```bash
# 按作者统计提交数量
git shortlog -sn

# 输出示例：
# 42  张三
# 18  李四
#  5  王五
```

### 有用的别名配置

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --all --decorate"

# 之后就可以用：
git st
git lg
```

---

## 13. 常见工作流

### 个人项目 / 简单工作流

```
main 分支
  ├── 直接在 main 开发
  └── 每个功能提交后 push
```

适合：个人项目、小型团队、原型开发。

### GitHub Flow（推荐给大多数团队）

```
main（始终可部署）
  ├── feature/login
  ├── fix/navbar-bug
  └── docs/api-update
```

流程：

```bash
# 1. 从 main 创建功能分支
git switch -c feature/user-profile main

# 2. 开发、提交
git add .
git commit -m "feat: 添加用户头像上传功能"

# 3. 推送并发起 Pull Request
git push -u origin feature/user-profile

# 4. 代码审查通过后，合并到 main
# （通常在 GitHub 网页端操作）

# 5. 删除功能分支
git branch -d feature/user-profile
git push origin --delete feature/user-profile
```

### 参与开源项目（Fork 工作流）

```bash
# 1. 在 GitHub 上 Fork 项目（点网页上的 Fork 按钮）

# 2. 克隆自己 Fork 的仓库
git clone https://github.com/你/repo.git
cd repo

# 3. 添加原仓库为 upstream 远程
git remote add upstream https://github.com/原作者/repo.git

# 4. 创建功能分支
git switch -c fix/typo-in-readme

# 5. 开发、提交、推送到自己的 Fork
git commit -am "fix: 修正 README 中的拼写错误"
git push origin fix/typo-in-readme

# 6. 在 GitHub 上从自己的分支向原仓库发起 Pull Request

# 7. 同步上游更新
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

---

## 14. 常见问题与解决

### Q: 不小心 `git add` 了不该加的文件

```bash
# 从暂存区撤出（改动保留在工作区）
git restore --staged 文件名

# 撤出所有暂存的内容
git restore --staged .
```

### Q: 提交信息写错了

```bash
# 修改最后一次提交信息（未推送）
git commit --amend -m "正确的提交信息"

# 已推送，不推荐修改，但若必须：
git commit --amend -m "正确的提交信息"
git push --force-with-lease
```

### Q: 误删了文件想恢复

```bash
# 恢复工作区中删除的文件（删除未暂存）
git restore 文件名

# 从某次提交中恢复文件
git restore --source abc123 文件名
```

### Q: 合并冲突不知道如何解决

```bash
# 查看哪些文件有冲突
git status

# 用 mergetool（可视化工具）解决
git mergetool

# 放弃合并
git merge --abort
```

### Q: 想把多个「乱七八糟」的提交整理成一个

```bash
# 交互式变基，整理最近 N 个提交
git rebase -i HEAD~N
# 把需要合并的提交前面的 pick 改成 squash（或 s）
```

### Q: 不小心把密码/密钥提交了怎么办

```bash
# 1. 立刻在服务端吊销/重置这个密钥
# 2. 从历史中删除（仅本地有效）
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch secrets.txt' \
  --prune-empty --tag-name-filter cat -- --all

# 3. 强制推送（需要让所有协作者重新克隆仓库）
git push origin --force --all

# 推荐使用更现代的工具：git-filter-repo
# pip install git-filter-repo
# git filter-repo --path secrets.txt --invert-paths
```

> ⚠️ 即使从历史中删除，被推送过的内容仍可能被缓存。密钥一经暴露，**必须立刻重置**，不能只靠删除历史来解决安全问题。

---

## 快速参考卡

```
初始化
  git init                    本地新建仓库
  git clone <url>             克隆远程仓库

记录变更
  git status                  查看当前状态
  git add <file>              加入暂存区
  git add .                   暂存所有改动
  git commit -m "<msg>"       提交
  git diff                    查看未暂存的改动
  git diff --staged           查看已暂存的改动

历史
  git log --oneline --graph   查看历史（图形）
  git show <hash>             查看某次提交
  git blame <file>            查看每行作者

分支
  git branch                  查看分支列表
  git switch -c <name>        创建并切换分支
  git switch <name>           切换分支
  git branch -d <name>        删除分支
  git merge <branch>          合并分支
  git rebase <branch>         变基

远程
  git remote -v               查看远程地址
  git push -u origin main     首次推送并关联
  git push                    推送
  git pull                    拉取并合并
  git fetch                   只拉取不合并

撤销
  git restore <file>          丢弃工作区改动
  git restore --staged <file> 撤出暂存区
  git reset --soft HEAD~1     撤销提交（改动进暂存区）
  git reset --hard HEAD~1     撤销提交（改动全丢弃）
  git revert <hash>           安全撤销（生成新提交）
  git stash                   临时储存改动
  git stash pop               恢复储存
```

---

*本文档基于 Git 2.x，建议保持 Git 版本在 2.23 以上以使用 `git switch`、`git restore` 等现代命令。*
