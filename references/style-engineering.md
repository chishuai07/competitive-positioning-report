# 苹果风格视觉令牌与工程红线

## 视觉令牌（设计系统）

```
背景：    --paper:#FFFFFF；灰白交替 section 底 #F5F5F7
文字：    --ink:#1D1D1F（标题）；--ink-soft:#6E6E73（正文）；--ink-faint:#6E6E73（小字，勿低于此对比度）
主色：    --accent:#0071E3（苹果蓝）；--accent-deep:#0077ED；--accent-tint:#EAF3FD
深色卡：  #1D1D1F 底 + #D2D2D7 正文 + #86868B 标签
卡片：    白底 + border-radius:18px + box-shadow:0 2px 16px rgba(0,0,0,.05)
字体：    系统栈 -apple-system / SF Pro / PingFang SC / Microsoft YaHei；不引外部 webfont
吸顶导航：rgba(22,22,23,.8) + backdrop-filter:blur(20px)，深色文字按钮
```

## 工程红线（评审已验证，逐条照做）

1. **锚点不被遮挡**：`html{scroll-behavior:smooth;scroll-padding-top:72px}`。
2. **导航滚动联动**：IntersectionObserver（rootMargin `-80px 0px -70% 0px`）给当前章节链接加 `.cur` 高亮，复用 hover 样式 `background:rgba(255,255,255,.14)`；同时加 `:focus-visible` 焦点态（无障碍）。
3. **对比度**：所有正文级小字 ≥#6E6E73（白底约 5.3:1）；#86868B 只留给深色卡上的装饰标签。
4. **标题层级**：h2 下直接用 h3（不要跳 h4）；卡片标题用 h3 并在 CSS 里覆盖字号。
5. **图片不抖动**：每张 `<img>` 补真实 width/height + `loading="lazy"`；产品图统一展示框（width:100%;height:固定;object-fit:contain）。
6. **KPI 条纯净**：只放我方占优指标，弱势指标不进 KPI。
7. **反向指标图表**：标题右侧加「越低越好」胶囊。
8. **我方列高亮**：td.ours 底色 rgba(0,113,227,.07)；表头 th.ours 加 2px 品牌色下边框。
9. **宽表滚动提示**：窄屏（<820px）在 .table-scroll 外包一层 .table-outer，右侧放 34px 渐隐阴影 `.fade-r`。
10. **打印降级**：`@media print` 隐藏 canvas 图表，显示预写好的 `.print-note` 数据文字；隐藏导航。
11. **深色模式**：`@media (prefers-color-scheme:dark)` 覆盖令牌（#000/#1D1D1F/#F5F5F7/#A1A1A6），产品图保持白底框。
12. **图表降级**：ECharts CDN 失败时显示「数据见第 02 章表格」文字分支。

## 操作坑（实测）

- 同一文件多处修改：≥10 处机械替换用一个带计数断言的 python 脚本一次完成；≤几处用 Edit，但**同一文件的多个 Edit 必须串行**，并行会被拒。
- Bash/python 写过文件后，下一次 Edit 前必须先 Read 刷新状态。
- 改产物前先备份到 `_backup/`。
- present_files 只交付一个 .html；提醒用户转发时连同 assets/ 目录（图片相对路径引用）。
