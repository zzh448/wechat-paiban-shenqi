# 微信公众号排版神器 · 网页版

纯前端单文件工具，把任意精排 HTML（含 CSS 变量、渐变、flex/grid、SVG、Chart.js 图表等）一键转换成**微信公众号编辑器可直接粘贴**的兼容格式。

> 移植自 WorkBuddy 的 `wechat-html-adapter`（Python 版），无需安装任何依赖，浏览器打开即用。

## 在线使用

将 HTML 源码 / Markdown 粘贴进左侧，点击「转换」，右侧实时预览 + 生成兼容代码，点「复制到公众号」后去微信编辑器 Ctrl+V 即可。

本地图片可直接拖入，会自动内嵌为 data URI。

## 它能解决什么

| 问题 | 本工具处理 |
|------|------|
| 微信粘贴后「样式全丢、布局塌陷」 | 用浏览器 `getComputedStyle` 把全部样式内联，级联/变量/媒体查询精确解析 |
| SVG 变空白 | Canvas 栅格化为 PNG |
| Chart.js 等 `<canvas>` 图表不显示 / 数据不对 | 等动画渲染完成后 `toDataURL` 栅格化为 PNG |
| 金色强调变黑色 | 计算 `var(--brand)` 等变量为实色 |
| flex/grid 多列布局错乱 | 卡片网格→`<table>`；行内横向流→`inline-block`；徽章标题头→2 列表格 |
| 中文乱码 | 非 ASCII 转 HTML 数字实体 |

## 核心能力

- 渐变 → 平均纯色
- `flex` / `grid` → 兼容表格或行内流
- `div` 背景/边框/居中 → `section`
- 伪元素 `::before/::after` → `<span>`
- 表格边框中和（避免微信重叠双边框）
- 中文实体编码

## 本地运行

直接用浏览器打开 `index.html` 即可，无需服务器、无需联网（图表类需能加载对应 CDN）。

## 技术说明

完全运行在浏览器端：借用浏览器的 CSS 计算引擎替代 Python 的 `tinycss2` + `soupsieve` 手写解析；用 Canvas 替代 `cairosvg` 做栅格化。所有转换在沙箱 iframe 内完成，不向任何服务器发送数据。
