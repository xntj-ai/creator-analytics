# creator-analytics

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-d97757.svg)](https://claude.com/claude-code)
[![Output: static HTML](https://img.shields.io/badge/output-static%20HTML-2ea44f.svg)](#examples--示例)

**Multi-platform creator analytics → one self-contained, presentable HTML report page.**

**多平台自媒体数据复盘 → 一张自包含、可公开展示的 HTML 报告页。**

creator-analytics is a [Claude Code](https://claude.com/claude-code) / Agent skill. Once your cross-platform numbers are crunched, it locks down the *last mile* — turning scattered conversation output into a single static HTML page with a clear point of view: a finding-card lede, a heat-mapped master table, topic-by-topic charts, a per-platform iteration panel, and a tickable execution checklist. Pure Python standard library generates the HTML, charts are inline SVG, and zero JS libraries are loaded — open the file in any browser.

creator-analytics 是一个 [Claude Code](https://claude.com/claude-code) / Agent 技能。当你把跨平台数据算完之后,它把**最后一公里**固化下来 —— 把散落的对话输出收成一张静态 HTML 页,有明确判断:核心结论卡先行、热力总表、专题图表、分平台迭代面板、可勾掉的执行 Checklist。纯 Python 标准库生成 HTML,图表用内联 SVG,零 JS 库 —— 任何浏览器打开即可。

## Why creator-analytics · 为什么用 creator-analytics

The hard part of a data review isn't generating the page — it's making sure the *insight* survives the presentation layer, and that the disciplined steps don't get skipped. Most analyses end as a wall of chat messages and a few stray screenshots; the judgment, the calibrations, and the actionable next steps evaporate the moment the session ends. creator-analytics takes the opposite stance: the deliverable is a real, reviewable artifact — one HTML page that leads with judgment, colors every magnitude column on a log scale so one viral hit doesn't whitewash the rest, flags anomalies honestly, and ends with a checklist you can actually tick off.

数据复盘真正难的不是把页面画出来 —— 是让**洞察**别在呈现层掉价,以及那些有纪律的步骤别被跳过。多数分析最后只剩一墙对话和几张零散截图;判断、口径校正、可落地的下一步,会话一结束就蒸发了。creator-analytics 反其道而行:交付物是一个真正可复审的成品 —— 一张 HTML 页,开篇就给判断、每一列量级指标按对数刻度上色(一个爆款不会把整列洗白)、异常如实标注,最后是一份你真能勾掉的清单。

## Features · 功能特性

- **Insight methodology baked in** — the `references/insight-playbook.md` 0→9 analysis path: read magnitude and rate separately, locate the funnel stage before judging content, use natural-experiment control groups to nail causation, check concentration (75% of new fans from one post ≠ a stable engine), compute correlations on your own data to test platform folklore, and attribute findings down to a changeable action. **内置洞察方法论** —— `references/insight-playbook.md` 的 0→9 分析路径:量级与率分开读、先定位漏斗层级再评判内容、用自然实验对照组坐实因果、查集中度(75% 涨粉来自一篇 ≠ 稳定引擎)、用自己的数据现算相关性验平台传言、把结论归因到可改的动作。
- **A presentable page skeleton** — finding cards (judgment first) → master heat table (many entities × many metrics, colored per-column on a log scale) → topic sections (bar chart / SVG scatter + threshold line) → per-platform iteration panel (P0 in red) → execution checklist → footer cadence statement. **可展示的页面骨架** —— 核心结论卡(先给判断)→ 热力总表(多实体 × 多指标,列内对数刻度上色)→ 专题分析(条形图 / SVG 散点 + 阈值线)→ 分平台迭代建议(P0 标红)→ 执行 Checklist → footer 口径声明。
- **The authoritative PPVI light design system** — `assets/template.html` is the single style source of truth: zero dependencies, no JS library, opens straight in a browser. This repo is itself the origin of the PPVI light style, so its own pages are meant to be exemplary. **权威 PPVI 浅色设计系统** —— `assets/template.html` 是唯一样式权威源:零依赖、无 JS 库、浏览器直接打开。本仓库就是 PPVI 浅色样式的源头,它自己的页面应当是范例级。
- **Copy-ready component helpers** — heat coloring, bar chart, scatter plot, four-semantic badges, iteration panel, and checklist renderer; the generation script copies them straight from `references/components.md`. **可直接抄的组件 helper** —— 热力上色、条形图、散点图、四语义徽标、迭代面板、Checklist 渲染器,生成脚本直接抄 `references/components.md`。
- **Analysis discipline as guardrails** — single source of numbers (read only the parsed JSON, never hand-type), anomalies with no visible cause marked *待核实* (to verify) rather than guessed at, research conclusions tagged and kept apart from your own measured data, and iteration advice re-researched every cycle instead of reusing the old. **纪律即护栏** —— 数字单一来源(只读解析后的 JSON,不手填)、无可见原因的异常标「待核实」不臆断、调研结论加标注并与实测分开、迭代建议每周期重新调研不沿用存量。

## When to use · 适用场景

**What it locks in · 它固化了什么** — Reach for creator-analytics when the analysis is done and the conclusions need to land as a presentable, reusable HTML page — the report-page step of a cross-platform review. It fixes the things that should never be improvised: the page structure, the design tokens, the chart helpers, and the review discipline (judgment-first cards, honest anomaly handling, footer cadence statement). 当分析做完、结论需要落成一张可展示可复用的 HTML 页时就用它 —— 即跨平台复盘的报告页步骤。它固定那些不该即兴发挥的东西:页面结构、设计 token、图表 helper、审查纪律(结论先行卡、诚实的异常处理、footer 口径声明)。

**What it does not do for you · 它不替你做什么** — The depth still comes from each cycle's real work: reading the data, computing correlations, doing attribution, researching the latest platform playbooks. It is not the blog-article page, not the publish copy, and not the raw data parsing (an upstream script handles that). It is not a generic charting library either. 分析的深度仍然来自每个周期的真实劳动:读数据、算相关性、做归因、调研平台最新打法。它不是博客文章页、不是发布文案、也不是原始数据解析(上游脚本的事),更不是一个通用图表库。

## Install · 安装

Clone this repository into your Claude Code skills directory — project-level (recommended) or user-level:

把本仓库克隆到你的 Claude Code 技能目录 —— 项目级(推荐)或用户级:

```bash
# Project-level (recommended) · 项目级(推荐)
git clone https://github.com/xntj-ai/creator-analytics .claude/skills/creator-analytics

# User-level, available to every project · 用户级,所有项目可用
git clone https://github.com/xntj-ai/creator-analytics ~/.claude/skills/creator-analytics
```

That is the only setup. The generated report is pure static HTML — no runtime dependency, no CDN, nothing installed locally. The one tool the generation script uses is Python (3.8+, standard library only); a PEP 723 header lets you run it with `uv run --script`.

只需这一步。生成的报告是纯静态 HTML —— 无运行时依赖、无 CDN、本地不装任何东西。生成脚本唯一用到的工具是 Python(3.8+,仅标准库);加个 PEP 723 头就能用 `uv run --script` 跑。

## Usage · 用法

**With Claude · 对 Claude 说** — first parse each platform's backstage export into one aligned JSON (one piece of content = one entity, metrics grouped by platform), then say *"build a review report page from this data."* Claude follows the page structure and the hard rules in `SKILL.md`, generates a `build_<topic>_html.py` script, and produces the static HTML you open locally.

先把各平台后台导出解析成一份对齐的 JSON(一期内容一个实体,按平台分组指标),然后说"基于这份数据出一张复盘报告页"。Claude 会按 `SKILL.md` 的页面结构和铁律,生成一个 `build_<topic>_html.py` 脚本,产出你本地打开的静态 HTML。

```bash
# After Claude writes the script · Claude 写好脚本后
uv run --script scripts/build_<topic>_html.py
# → out: <topic>_review.html  (open in any browser · 浏览器直接打开)
```

**Standalone, without Claude · 独立使用,不经 Claude** — the design system lives entirely inside `assets/template.html`, so a model is optional:

设计系统完全封装在 `assets/template.html` 里,大模型可有可无:

1. Copy `assets/template.html`. · 复制 `assets/template.html`。
2. Fill the sections with your own findings and numbers (helper code in `references/components.md`). · 用你自己的结论和数字填进各区块(helper 代码见 `references/components.md`)。
3. Open it in a browser. · 浏览器打开即可。

## Report structure · 报告结构

The page is assembled in a fixed order; drop any section whose data you don't have, but the **finding cards** and the **footer cadence statement** stay in every version.

页面按固定顺序组装;没有对应数据的区块可删,但**核心结论卡**和 **footer 口径声明**任何版本都保留。

| Section 区块 | Component 组件 | Purpose 作用 |
|---|---|---|
| Kicker + H1 + lead | — | Account / range / north-star question. 账号 / 区间 / 北极星问题。 |
| Stat strip | `.stats` | 3–4 global summary numbers. 3–4 个全局汇总数。 |
| Finding cards | `.card` (gold / `j` jade / `t` terra) | 4–6 judgment-first cards — the whole page in a glance. 4–6 张结论先行卡 —— 全页判断的浓缩。 |
| Master heat table | `.tablecard` | Many entities × many metrics, per-column log-scale coloring + anomaly badges. 多实体 × 多指标,列内对数刻度上色 + 异常徽标。 |
| Topic sections × N | `.bars` / `.scatter` + `.two` | One-line stance + finding-first lede + chart + paired panels. 一句话立场 + 结论先行 + 图 + 成对解读。 |
| Iteration panel | `.iter` (⚑→P0) | Per-platform multi-dimension advice, P0 in red. 分平台多维建议,P0 标红。 |
| ★ Strategy cards | `.card` | The cycle's key corrections (calibrations). 本轮校正后的关键修正。 |
| Execution checklist | `.ckl` | Tickable, verb-first actions (P0 first). 可勾掉的动作清单(动词开头,P0 优先)。 |
| Footer | `footer` | Data source + review-discipline statement + generation script. 数据来源 + 审查纪律声明 + 生成脚本名。 |

Color semantics run through the whole page: **gold `#C8922E`** neutral emphasis · **jade `#1f6f5c`** positive / confirmed · **terra `#B4502E`** warning / correction / P0.

色彩语义贯穿全页:**gold `#C8922E`** 中性强调 · **jade `#1f6f5c`** 正面/坐实 · **terra `#B4502E`** 警示/校正/P0。

## Examples · 示例

The skill was distilled from a real 32-episode × 5-platform review on the「张拼拼·XNTJ」account — the same review is written up in the [EP0034 article](https://xntj.tv/ep/live-ep0034/), where Claude Code acts as the operations director, merges every platform's backstage export, and produces the optimization checklist. The skill's own page (`docs/`) is built on this very `assets/template.html`, so it doubles as a live style reference.

本技能蒸馏自「张拼拼·XNTJ」账号一次真实的 32 期 × 5 平台复盘 —— 同一次复盘写在 [EP0034 那篇文章](https://xntj.tv/ep/live-ep0034/)里:Claude Code 当运营总监,合并各平台后台导出,产出优化清单。技能自己的页面(`docs/`)就是用这套 `assets/template.html` 搭的,可当作活的样式参考。

## How it works · 技术原理

The generation script is plain Python standard library: it reads one aligned `_<topic>_data.json`, formats numbers, colors each magnitude column on a log scale (so a long-tail viral hit doesn't flatten the column to white), draws bar charts and scatter plots as inline SVG strings, and renders iteration panels and checklists from small data-driven helpers — then drops it all into the `assets/template.html` skeleton. The output is one static `.html`: no bundler, no backend, no CDN, no JS chart library. When the CSS goes into an f-string, the braces are doubled (`{{`); that is the only build wrinkle.

生成脚本就是纯 Python 标准库:读一份对齐的 `_<topic>_data.json`,格式化数字,把每一列量级指标按对数刻度上色(长尾爆款不会把整列压成白),用内联 SVG 字符串画条形图和散点图,用小巧的数据驱动 helper 渲染迭代面板和 Checklist —— 再全部填进 `assets/template.html` 骨架。产物是一个静态 `.html`:无打包、无后端、无 CDN、无 JS 图表库。CSS 放进 f-string 时花括号写成 `{{`,这是唯一的构建小坑。

## FAQ · 常见问题

**Does the report need an internet connection?** No. Unlike many "HTML report" tools, the output is pure static HTML with inline SVG charts and zero CDN calls — it opens offline and renders identically.

**报告需要联网吗?** 不需要。和很多"HTML 报告"工具不同,产物是纯静态 HTML,图表是内联 SVG,零 CDN 调用 —— 离线打开,渲染一致。

**Can I use it without Claude?** Yes. Copy `assets/template.html`, fill the sections with your own findings, and open it. The model is a convenience for turning analysis into a built page, not a dependency.

**不用 Claude 能用吗?** 可以。复制 `assets/template.html`,把各区块换成你自己的结论,打开即可。大模型只是帮你把分析变成成品页的便利,不是必需品。

**Does the skill parse my raw exports?** No — that's the upstream step. The hard rule is "single source of numbers": you parse each platform's export into one aligned JSON first, and the generation script only reads that JSON and computes, never hand-types a number. The footer states this explicitly.

**技能会解析我的原始导出吗?** 不会 —— 那是上游步骤。铁律是"数字单一来源":你先把各平台导出解析成一份对齐 JSON,生成脚本只读这份 JSON 算数,绝不手填任何数字。footer 会明确声明这一点。

**Why log-scale coloring on the magnitude columns?** Play counts and impressions are long-tailed; on a linear scale one viral entry pushes every other cell to near-white. Per-column log coloring keeps the within-column contrast readable. Magnitude metrics are colored per-column only — they are not comparable across platforms.

**为什么量级列用对数刻度上色?** 播放和曝光是长尾分布;线性刻度下一个爆款会把其它单元格压到接近白色。列内对数上色让列内对比依然可读。量级指标只做列内对照 —— 跨平台不可横比。

**Public or private report?** The page defaults to "will be shown publicly": monetization strategy (hooks / funnels / pricing) stays out of the public page; emit a separate private file when you need it.

**报告公开还是私有?** 页面默认按"会公开展示"对待:变现策略(承接钩子/漏斗/定价)不进公开页;需要时另出私有文件。

## Related · 相关

- [ppvi](https://github.com/xntj-ai/ppvi) — the PinPin Visual Identity methodology; creator-analytics is one of its light-mode origins. PinPin 视觉识别方法论,creator-analytics 是其浅色样式的源头之一。
- [flowmaker](https://github.com/xntj-ai/flowmaker) — a sibling Claude Code skill that turns a process into a single-HTML editable flowchart, on the same light glass aesthetic. 同体系的 Claude Code 技能:把流程变成单 HTML 可编辑流程图,同款浅色玻璃风。
- [cross-review](https://github.com/xntj-ai/cross-review) — multi-model adversarial review; pairs well when a strategy call needs a second opinion. 多模型交叉审查,做策略决策时拉来交叉验证。
- [xntj.tv](https://xntj.tv) — more Claude Code workflows and skills from 张拼拼 · XNTJ. 更多来自张拼拼·XNTJ 的 Claude Code 工作流与技能。

## License · 许可证

[MIT](./LICENSE) © [张拼拼 · XNTJ](https://xntj.tv)
