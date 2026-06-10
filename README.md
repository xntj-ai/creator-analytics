# creator-analytics

自媒体数据分析 → 一张可公开展示的 HTML 报告页。Claude Code skill。

把多平台（视频号 / 小红书 / 抖音 / B站 / YouTube …）运营数据复盘的**最后一公里**固化下来：分析做完之后，结论怎么落成一张有判断、有口径、有落地动作的报告页——而不是一堆散落的对话输出。

来自「张拼拼·XNTJ」账号日更实战：32 期视频 × 5 平台的完整数据复盘跑通后蒸馏成此 skill（[复盘那期视频的文章](https://xntj.tv/ep/live-ep0034/)）。

## 它固化了什么

- **页面骨架**：核心结论卡（先给判断）→ 热力总表（多实体×多指标，列内对数刻度上色）→ 专题分析（条形图 / SVG 散点 + 阈值线）→ 分平台迭代建议（P0 标红）→ 执行 Checklist → footer 口径声明
- **一套完整 CSS 设计系统**（PPVI 浅色）：`assets/template.html` 是样式权威源，零依赖、无 JS 库，浏览器直接打开
- **组件 helper 代码**：热力上色、条形图、散点图、四语义徽标、迭代面板、Checklist 渲染器，生成脚本直接抄（`references/components.md`）
- **分析纪律**：数字单一来源（只读解析后的 JSON，不手填）、异常无因标「待核实」不臆断、调研结论与实测分开标注、迭代建议每周期重新调研不沿用存量

## 它不替你做什么

分析的深度仍然来自每个周期的真实劳动：读数据、算相关性、做归因验证、调研平台最新打法。skill 保证的是这些劳动的产出**不会在呈现层掉价**，以及纪律性的步骤不会被跳过。

## 安装

```bash
# 项目级（推荐）
git clone https://github.com/xntj-ai/creator-analytics .claude/skills/creator-analytics

# 或用户级（所有项目可用）
git clone https://github.com/xntj-ai/creator-analytics ~/.claude/skills/creator-analytics
```

## 使用

1. 先把各平台后台导出解析成一份对齐的 JSON（一期内容一个实体，按平台分组指标）
2. 对 Claude Code 说：「基于这份数据出一张复盘报告页」
3. Claude 会按 SKILL.md 的页面结构和铁律生成 `build_<topic>_html.py`，产出静态 HTML

## License

MIT
