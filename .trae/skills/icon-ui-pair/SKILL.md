---
name: icon-ui-pair
description: 成对生成字体图标与 SVG 图标两种方式的网页 UI 组件，自动本地化 CDN 资源。Use when user asks for icon showcases, mobile tab bars, or any icon-driven page needing both font and SVG variants. Do not use for single-method icon pages or non-icon UI.
---

# Icon UI Pair Generator

基于项目内 `cdn/`、`font/` 本地资源和"字体图标 vs SVG 图标成对生成"的固定套路，产出两个 HTML 文件。

## 触发前提

用户已说明需求场景（图标库展示、移动端底部导航等）和图标来源（Font Awesome、Lucide、项目 `font/` 目录下的 iconfont）。本 skill 不主动询问选型，按用户指定执行。

## 资源本地化

**Font Awesome**（字体图标）：
- 目标目录：`cdn/font-awesome/`
- 必须下载并保留：`css/all.min.css` 及对应的 `webfonts/` 目录下全部 `.woff2` / `.ttf` 文件
- HTML 引入：`<link rel="stylesheet" href="cdn/font-awesome/css/all.min.css">`
- 版本匹配：CSS 与 webfonts 必须来自同一次官方发布包，不能混搭

**Lucide**（SVG 图标）：
- 目标目录：`cdn/lucide/lucide.min.js`
- HTML 引入：`<script src="cdn/lucide/lucide.min.js"></script>` + `lucide.createIcons()`
- Lucide 使用 `stroke` 而非 `fill`，颜色通过 `stroke="currentColor"` 继承

**iconfont**（自定义项目字体，已在 `font/` 目录）：
- HTML 引入：`<link rel="stylesheet" href="font/iconfont.css">`
- 字体类命名规则：`iconfont icon-<font_class>`，其中 `<font_class>` 来自 `font/iconfont.json` 的 `glyphs[].font_class`

## 成对文件命名

| 后缀 | 方式 | 图标渲染手段 | 典型场景 |
|------|------|-------------|---------|
| `-1.html` | 字体图标 | `<i class="库前缀 图标类名">` | Font Awesome fa-solid/fa-regular/fa-brands、iconfont iconfont icon-xxx |
| `-2.html` | SVG | `<svg><use xlink:href="#symbol-id"></use></svg>` 或 Lucide `<i data-lucide="name">` | Lucide JS 运行时、iconfont SVG Symbol 雪碧图 |

文件名中的序号由用户指定的基数决定（如"第 13 组"→ `13-1.html`、`13-2.html`）。若出现导航或其他多组件，延续同一基数（`13-3.html`、`13-4.html`）。

## 两种方式的技术差异速记

**字体图标方式**：
- 使用 `<i>` 或 `<span>` 标签
- 大小由 `font-size` 控制，颜色继承 `color`
- CSS 伪元素渲染字体 glyph，支持 CSS `animation`
- 不需要额外 SVG 资源文件

**SVG 方式**：
- 使用原生 `<svg>` + `<symbol>` 雪碧图（iconfont）或 Lucide JS 运行时注入
- 大小由 `width` / `height` 控制，颜色继承 `currentColor` 或 stroke/fill 属性
- 可单独控制每个 path 的 stroke、fill、clip-path
- iconfont 的 SVG symbol 必须确保 path 的 viewBox 与字体原始 viewBox 一致，坐标才能对齐

## 移动端底部导航固定套路

当需求是移动端固定底部导航（tabbar）时，严格遵循以下模板。

**结构骨架**：
```html
<nav class="yao_tabbar">
  <a class="yao_item yao_item_active" href="#">
    <span class="yao_icon_wrap"><i class="iconfont icon-xxx"></i></span>
    <span class="yao_label">标签名</span>
  </a>
  <!-- 其他项 -->
</nav>
```

**核心样式**（字体图标版，SVG 版仅替换图标选择器）：
```css
.yao_tabbar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100%;
  display: flex;
  background: #fff;
  border-top: 1px solid #eceef1;
  box-shadow: 0 -2px 12px rgba(24, 36, 48, .06);
  user-select: none;
  padding-bottom: env(safe-area-inset-bottom); /* iOS 刘海屏适配 */
}
.yao_item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: .25rem;
  padding: .5rem 0 .45rem;
  color: #333;
  text-decoration: none;
}
.yao_item_active { color: #000; }
```

**角标模板**（放在 `.yao_icon_wrap` 内部，`position: absolute` 相对定位）：
```css
/* 数字徽章 */
.yao_badge {
  position: absolute; top: -6px; right: -12px;
  min-width: 16px; height: 16px; padding: 0 4px;
  border-radius: 8px;
  background: #ae2b2b; color: #fff;
  font-size: 10px; line-height: 16px; text-align: center;
  border: 1.5px solid #fff; box-sizing: border-box;
}
/* 红点 */
.yao_dot {
  position: absolute; top: -2px; right: -6px;
  width: 8px; height: 8px; border-radius: 50%;
  background: #ae2b2b;
  border: 1.5px solid #fff; box-sizing: border-box;
}
```

**内容区占位**：主体 `padding-bottom` 至少留 5.5rem，避免被 tabbar 遮挡。

## iconfont SVG 雪碧图生成

从 `font/iconfont.json` 的 `glyphs[]` 列表读取需要的 `font_class`，然后使用 iconfont 项目已导出的 SVG symbol（或从 `font/iconfont.js` / 在线工具导出）。不要尝试从 TTF/WOFF 二进制文件反推 path——坐标和 viewBox 容易对不上。

使用方式：
```html
<svg aria-hidden="true" style="position:absolute;width:0;height:0;overflow:hidden">
  <symbol id="icon-<font_class>" viewBox="0 0 <原始viewBox>">
    <path d="..."/>
  </symbol>
</svg>
<!-- 消费端 -->
<svg class="yao_icon" aria-hidden="true"><use xlink:href="#icon-<font_class>"></use></svg>
```

SVG 消费端样式：`.yao_icon { width: 1.45rem; height: 1.45rem; fill: currentColor; }`

## 类名与署名规范

- 所有自定义 CSS 类必须加 `yao_` 前缀（项目约定）
- 图标库原生类名（`fa-solid`、`iconfont`、`data-lucide`）保持原样不重命名
- 页面 header 区域保留"姚琳 · <序号>"署名格式
- `viewport` meta 必须存在：`<meta name="viewport" content="width=device-width, initial-scale=1.0">`
- `box-sizing: border-box;` 和 `-webkit-tap-highlight-color: transparent;` 作为全局重置

## 质量检查

- 字体图标 HTML 中，CDN 路径用相对路径，且 webfonts 文件在磁盘上确实存在
- SVG HTML 中，所有被 `<use>` 引用的 symbol id 在 sprite 里有定义
- 底部导航 `width: 100%` 生效，`env(safe-area-inset-bottom)` 正确声明
- 徽章/红点只在 `position: relative` 的容器内生效，检查 `.yao_icon_wrap` 是否遗漏
- 两个文件视觉上能一一对应，导航项数量、顺序、标签文案一致
