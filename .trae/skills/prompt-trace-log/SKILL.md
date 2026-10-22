---
name: prompt-trace-log
description: 每次执行任务前把用户提示词按固定格式附加写入 yao_prompt.md 留痕，并按编号页面规范创建课程 HTML 作业。Use when the user asks to create or modify course pages in this project, or asks to record or log prompts. Do not use for tasks outside this project.
---

# Prompt Trace Log（提示词留痕 + 课程页面规范）

本 skill 固化项目 `yao_prompt.md` 首条记录（2026-10-12）确立的工作流：**所有提示词在执行之前先留痕**。配合 `.trae/rules/project_rules.md`（`yl_` 命名前缀、HTML 注释块）一起执行。

## 1. 执行前留痕（必须最先执行）

用户提出任何开发类请求后、动手实现之前，先把该条提示词**附加**到项目根目录 `yao_prompt.md`，格式与文件中既有记录完全一致：

```markdown
---

## 时间：YYYY-MM-DD HH:mm:ss
- 使用的模型：<当前实际使用的模型名>
- 提示词内容：<用户提示词原文，一字不改>

---
```

规则：

- **只追加，不覆盖**——禁止改写、删除或重排历史记录
- 新记录放在文件末尾，与上一条记录之间空一行
- 时间用本地时间，格式 `YYYY-MM-DD HH:mm:ss`；拿不到秒时用 `00:00:00` 补齐
- 模型名无法确定时写 `trae` 即可，不要虚构具体版本号
- 一次会话中的多轮修改请求，每轮用户消息各留痕一条
- 留痕失败不阻塞任务，但要在最终回复中说明留痕未成功

## 2. 课程页面规范

从提示词历史（11-1、11-2、13-1 ~ 13-4）提炼的页面创建约定：

- **文件名**：`章-序.html`（如 `11-1.html`、`13-2.html`）。章节号与序号由用户指定；未指定时先询问，不要自创编号
- **返回首页**：每个新建页面正文中必须有指向 `index.html` 的超链接，样式与页面风格一致
- **head 注释块**：`姓名：姚琳`、`日期：YYYY-MM-DD`、`功能名称：<本页功能>`，位于 `<head>` 内 `<title>` 附近
- **命名前缀**：新代码一律 `yl_`（变量、CSS 类、ID）；旧代码已有的 `yao_` 前缀不做全量重命名，仅改动处遵循新规则
- **技术选型**：图标相关任务遵循 `icon-ui-pair` skill（字体图标 + SVG 成对生成、CDN 本地化到 `cdn/`）
- **第三方类名**：`fa-solid`、`iconfont`、`data-lucide` 等保持原样，不加前缀

## 3. 执行顺序

1. 留痕（第 1 节）
2. 创建或修改页面（第 2 节 + project_rules.md）
3. 回复用户时，附上本次写入 `yao_prompt.md` 的记录条目，确认留痕完成
