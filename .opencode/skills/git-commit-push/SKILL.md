---
name: git-commit-push
description: 提交并推送整个项目到 GitHub 仓库 https://github.com/vy2707/yl。Use when the user asks to commit、push、提交、推送到 github、提交到github、上传到 github, or wants to sync the whole project to GitHub.
---

# 提交并推送到 GitHub

把当前项目的全部改动提交并推送到 GitHub 仓库 `https://github.com/vy2707/yl`。

## 工作流程

按顺序执行以下步骤：

1. **检查状态**

   ```bash
   git status
   git remote -v
   ```

2. **确保 github 远程存在**（若不存在则添加）

   ```bash
   git remote add github https://github.com/vy2707/yl
   ```

   若已存在但 URL 不对，修正：

   ```bash
   git remote set-url github https://github.com/vy2707/yl
   ```

3. **暂存全部改动**

   ```bash
   git add -A
   ```

4. **提交**

   ```bash
   git commit -m "<中文提交信息>"
   ```

   提交信息需用中文、简洁地概括本次改动（如"新增 15-1、630-2 页面与项目规则"）。
   若用户提供了提交信息，直接使用。

5. **推送到 GitHub**

   ```bash
   git push -u github master:main
   ```

   - 本地默认分支为 `master`；GitHub 空仓库默认分支为 `main`，故用 `master:main` 映射。
   - 若 GitHub 上已存在 `main` 分支且历史不一致，需先 `git pull github main --rebase` 或与用户确认是否强推，**不要擅自 `--force`**。

## 注意事项

- 不要改动 `origin`（可能指向 Gitee 等其他远程），推送一律走 `github` 远程。
- 提交前先 `git status` 与 `git diff` 确认，只提交预期文件，绝不提交密钥。
- 除非用户明确要求，不要 `--force`、不要 amend、不要改 git 配置。
