# AGENTS.md

本文件供 AI 编程代理（Agent）在本仓库中工作时遵循。适用于所有 HTML、CSS、JS 文件的编写与修改。

## 1. 命名规范

所有自定义命名必须以 `yl_` 为前缀：

- **JS 变量名**：`yl_total`、`yl_index`
- **CSS 类名**：`yl_main`、`yl_header`、`yl_tabbar`、`yl_item`
- **ID 名称**：`yl_content`、`yl_nav`
- **JS 函数名/组件名**：`yl_initTabbar`

示例：

```html
<div id="yl_content" class="yl_main">
  <nav class="yl_tabbar">
    <a class="yl_item yl_item_active" href="#">首页</a>
  </nav>
</div>
```

```css
.yl_main { width: 100%; }
.yl_item_active { color: #000; }
```

```javascript
const yl_items = document.querySelectorAll('.yl_item');
```

## 2. HTML 页面注释要求

每个 HTML 页面必须在 `<head>` 内（`<title>` 附近）包含以下格式的注释块：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
	<meta charset="UTF-8">
	<!--
	姓名：姚琳
	日期：2026-10-22
	功能名称：移动端底部导航（字体图标版）
	-->
	<title>页面标题</title>
</head>
```

- **姓名**：固定为 `姚琳`
- **日期**：该页面的创建或最后修改日期，格式 `YYYY-MM-DD`
- **功能名称**：本页实现的功能，简明扼要（如"移动端底部导航"、"图标库展示"）

## 3. 执行范围

- **新建文件**：全部遵循以上规则
- **修改旧文件**：改动部分遵循以上规则；旧代码中已有的其他前缀（如 `yao_`）不做全量重命名，除非用户明确要求
- **第三方库原生类名**（如 `fa-solid`、`iconfont`、`data-lucide`）保持原样，不加前缀

## 4. 技术栈约束

- 只使用**原生 HTML、CSS、JavaScript** 实现，不引入任何第三方库和框架（如 jQuery、Vue、React、Bootstrap 等）
- **图标**：统一使用 `font/` 目录下的 iconfont 字体图标——

  ```html
  <link rel="stylesheet" href="font/iconfont.css">
  ```

  用法：

  ```html
  <i class="iconfont icon-<font_class>"></i>
  ```

  可用图标名在 `font/iconfont.json` 的 `glyphs[].font_class` 中查询
- **禁止 CDN**：所有资源必须本地存在；`cdn/` 目录下的 Font Awesome / Lucide 仅限历史页面（13-1、13-2）使用，新页面不得引用
- **服务器端数据**：一律使用原生 `fetch` API 获取，不使用 XMLHttpRequest 或第三方 ajax 库；本地 JSON 数据需通过 http:// 访问（如 VS Code Live Server），直接双击以 file:// 打开会受 CORS 限制
