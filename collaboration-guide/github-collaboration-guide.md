# Git 与 GitHub 协作指南 {.unnumbered}

本指南面向第一次用 GitHub 合作写书的作者。Git 保存每次修改的历史，GitHub 用于同步、讨论和审阅。最重要的工作方式是：**每项任务先建 Issue，再在独立分支小步提交，通过 Pull Request 审阅后合并。**

## 一、开始前准备

安装 Git 和 VS Code，并向仓库维护者提供 GitHub 用户名。首次使用 Git 时设置提交身份：

```bash
git config --global user.name "你的姓名"
git config --global user.email "你的 GitHub 邮箱"
```

不要把密码或访问令牌写进项目文件、命令记录或聊天。GitHub 如要求认证，优先使用 VS Code／GitHub 官方登录流程或 SSH；密钥和令牌由本人在本机配置。

本书仓库地址为：

- GitHub 页面：<https://github.com/zhiyuanryanchen/AI-For-Structural-Estimation>
- SSH 地址：`git@github.com:zhiyuanryanchen/AI-For-Structural-Estimation.git`

首次下载项目：

```bash
git clone git@github.com:zhiyuanryanchen/AI-For-Structural-Estimation.git
cd AI-For-StructuralEstimation
```

`clone` 只在第一次下载项目时使用。以后同步用 `pull` 和 `push`。默认分支为 `main`；首次使用 SSH 时，需先在自己的 GitHub 账户配置 SSH 公钥。

## 二、每天开始前

先进入项目目录并确认状态：

```bash
git status
git branch --show-current
```

确认自己没有未完成修改后，切换到默认分支并获取最新内容：

```bash
git switch main
git pull --ff-only
```

`pull` 是把 GitHub 上的更新下载并整合到本地。`--ff-only` 在历史分叉时停止，避免初学者无意生成复杂合并。若命令失败，不要使用 `reset --hard` 或强制覆盖；先保存工作并向维护者求助。

## 三、Issue：先说明要做什么

Issue 是任务和讨论记录。开始工作前创建 Issue，标题写清对象和动作，例如：

```text
样章：补充 Nested Logit 参数约定与边界解释
```

正文至少包含：

- 背景与目标。
- 涉及的章节或文件。
- 预计交付物。
- 需要谁提供素材或审阅。
- 完成标准和未决问题。

一个 Issue 尽量只处理一个清晰任务。理论修改、交互作业和无关格式调整不要塞进同一个 Issue。

## 四、Branch：每项任务使用独立分支

从最新 `main` 创建分支：

```bash
git switch main
git pull --ff-only
git switch -c theory/nested-logit-derivation
```

推荐命名：

- `theory/...`：理论推导和文献。
- `interaction/...`：AI 交互、Prompt 与作业。
- `chapter/...`：章节整合。
- `docs/...`：协作或写作说明。
- `fix/...`：明确错误修复。

不要直接在 `main` 上写作。分支隔离不同任务，让修改可以单独审阅和撤回。

## 五、修改与检查

只编辑源文件，例如 `.qmd`、`.md`、代码、数据说明和配置。不要编辑或提交：

- `_book/`：网页构建产物。
- `_exports/`：Word、EPUB 和 PDF。
- `.quarto/`、临时日志和本地密钥。

修改后查看状态和差异：

```bash
git status
git diff
```

提交前至少运行与本次修改最接近的检查。纯文本章节通常运行：

```bash
quarto render --to html
```

若修改公式、代码或实验，还需运行相应实验和测试。网页编译成功不能证明经济学内容正确。

## 六、Commit：保存一个有意义的小步骤

Git 的提交分两步。先把准备提交的文件加入暂存区：

```bash
git add chapters/10-logit-demand.qmd
```

再次检查：

```bash
git diff --staged
```

然后提交：

```bash
git commit -m "docs: clarify nested logit parameter convention"
```

推荐提交类型：

- `docs:` 书稿、说明和参考资料。
- `feat:` 新实验或交互功能。
- `fix:` 修复公式、代码或构建错误。
- `test:` 增加验证或复现检查。
- `chore:` 配置和维护工作。

一次提交只做一件事。不要使用 `git add .` 盲目加入全部文件；优先明确列出文件。Commit 是保存在本机的历史，尚未自动上传 GitHub。

## 七、Push：把分支上传到 GitHub

第一次上传当前分支：

```bash
git push -u origin theory/nested-logit-derivation
```

以后继续上传只需：

```bash
git push
```

`push` 会把本地提交发送到 GitHub，不会自动合并进 `main`。不要使用 `--force`；如遇到拒绝，先检查远程更新和分支名称。

## 八、Pull Request：请求审阅与合并

在 GitHub 打开刚推送的分支，创建 Pull Request（PR），目标分支选择 `main`。PR 标题概括修改，正文包含：

```markdown
## 修改内容
- 补充 Nested Logit 参数约定
- 增加边界情形说明

## 验证
- [ ] Quarto HTML 构建通过
- [ ] 理论公式由负责人复核
- [ ] 未修改无关章节

Closes #<Issue编号>
```

PR 是审阅空间，不只是合并按钮。审阅者可以逐行评论；作者在同一分支继续修改、提交并 `push`，PR 会自动更新。不要为回应每条意见另开新 PR。

合并前至少满足：

- 与 Issue 范围一致。
- 自动或本地构建通过。
- 理论、交互或数据由相应负责人审阅。
- 讨论已解决，未把未决判断伪装成定论。

由维护者或约定负责人合并。初学阶段优先使用 GitHub 的 Squash and merge，使一个 PR 在 `main` 中形成一条清晰记录。

## 九、同步主分支与清理分支

PR 合并后回到本地：

```bash
git switch main
git pull --ff-only
git branch -d theory/nested-logit-derivation
```

删除本地分支不会删除已合并的历史。GitHub 上的远程分支可在 PR 页面删除。

## 十、工作中如何获取别人更新

如果自己的分支尚未完成，而 `main` 已更新：

```bash
git status
git fetch origin
git merge origin/main
```

合并前确保当前修改已提交。若发生冲突，Git 会在文件中标记双方内容。不要随意删掉另一方修改，也不要让 AI 未经理解自动选择全部一侧。与涉及该段内容的作者确认，完成修改后运行：

```bash
git add <已解决的文件>
git commit
```

初学者遇到冲突时优先暂停并联系维护者。冲突是两组修改需要人工决定，不是 Git 损坏。

## 十一、常见错误与恢复原则

### 忘记建分支，已经在 `main` 修改

只要尚未提交，可以直接创建分支保留修改：

```bash
git switch -c docs/my-current-work
```

### 拉取前有未提交修改

先提交到当前任务分支。不要为了拉取更新而删除文件。若修改只是临时实验，向维护者确认后再使用 stash；初学指南不把 stash 设为默认流程。

### 提交了不应提交的文件

不要重写已经共享的历史。新增一次提交删除文件并完善 `.gitignore`，在 PR 中说明。若文件含密钥或敏感数据，立即通知维护者并轮换密钥；普通删除不能消除历史泄露。

### 命令不确定

先运行只读命令：

```bash
git status
git log --oneline --decorate -10
git diff
```

禁止在未确认后果时运行 `git reset --hard`、`git clean -fd`、`git push --force` 或大范围覆盖命令。

## 十二、首次协作练习

1. 维护者创建练习 Issue。
2. 每位作者从最新 `main` 创建自己的 `docs/...` 分支。
3. 只修改协作指南中的一处文字或个人分工说明。
4. 运行 Quarto HTML 构建。
5. 创建一个小提交并推送。
6. 创建 PR，邀请另一位作者审阅。
7. 根据一条审阅意见继续修改并推送。
8. 合并后拉取最新 `main` 并删除本地分支。

完成这次闭环后，再进入样章理论、交互和模拟数据工作。
