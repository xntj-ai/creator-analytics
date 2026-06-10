# 组件 helper 代码（生成脚本直接抄用）

所有 helper 假定数据已读入：`DATA = json.load(open("_<topic>_data.json", encoding="utf-8"))`，
`EPS` 为实体列表（一期视频/一篇笔记为一个实体），`g(e, p, k)` 为安全取值：

```python
def g(e, p, k, d=None):
    return e.get(p, {}).get(k, d) if e.get(p) else d
```

## 目录

1. [数字格式化 fnum](#1-数字格式化-fnum)
2. [列内热力上色 heat](#2-列内热力上色-heat)
3. [条形图 bars](#3-条形图-bars)
4. [SVG 散点图 scatter](#4-svg-散点图-scatter)
5. [徽标 badge](#5-徽标-badge)
6. [热力总表（分组表头 + sticky 首列）](#6-热力总表)
7. [迭代建议面板 ITER](#7-迭代建议面板-iter)
8. [执行 Checklist](#8-执行-checklist)

## 1. 数字格式化 fnum

```python
def fnum(v):
    if v is None: return "—"
    if isinstance(v, float) and v != int(v): return f"{v:,.2f}" if v >= 1 else f"{v:.2f}"
    return f"{int(v):,}"
```

缺数显示 `—`，不显示 0（0 和缺数是两回事）。

## 2. 列内热力上色 heat

长尾分布的量级指标（播放/曝光）必须用对数刻度，否则一个爆款把整列压白：

```python
import math

def col_vals(p, k):
    return [g(e, p, k) for e in EPS if isinstance(g(e, p, k), (int, float))]

def heat(v, p, k, reverse=False):
    if not isinstance(v, (int, float)): return ""
    vals = col_vals(p, k)
    if not vals: return ""
    lo, hi = min(vals), max(vals)
    if hi <= lo: return ""
    def t(x): return math.log10(x + 1)
    frac = (t(v) - t(lo)) / (t(hi) - t(lo))
    if reverse: frac = 1 - frac
    frac = max(0, min(1, frac))
    a = 0.06 + frac * 0.62          # 透明度区间，浅色主题
    return f"background:rgba(200,146,46,{a:.2f});"
```

用法：`f'<td class="num" style="{heat(v,"平台","指标")}">{fnum(v)}</td>'`。
颜色只跟**列内**比（每列自己的 min/max），跨列不可比时正合适。

## 3. 条形图 bars

单指标 TopN 排名，放进 `.panel` 内：

```python
def bars(items, unit="", maxw=None, fmt=fnum):
    mx = maxw or max(v for _, v in items)
    out = []
    for label, v in items:
        w = (v / mx * 100) if mx else 0
        out.append(f'<div class="barrow"><span class="bl">{label}</span>'
                   f'<span class="bt"><span class="bf" style="width:{w:.1f}%"></span></span>'
                   f'<span class="bv">{fmt(v)}{unit}</span></div>')
    return "".join(out)
```

固定满分指标（如 5 星制）传 `maxw=5`，别让最高者占满 100%。

## 4. SVG 散点图 scatter

两指标关系 + 异常点标注 + 阈值参考线。无 JS、无外部库，纯 SVG 字符串。
X 轴是量级指标（播放/曝光）时用对数刻度：

```python
pts = [(e, x_val(e), y_val(e)) for e in EPS
       if isinstance(x_val(e), (int, float)) and isinstance(y_val(e), (int, float)) and x_val(e) > 0]
SW, SH, PAD = 680, 380, 46
xs = [math.log10(p[1]) for p in pts]; ys = [p[2] for p in pts]
xmin, xmax = min(xs), max(xs); ymin, ymax = 0, max(ys) * 1.1
def sx(x): return PAD + (math.log10(x) - xmin) / (xmax - xmin) * (SW - 2*PAD)
def sy(y): return SH - PAD - (y - ymin) / (ymax - ymin) * (SH - 2*PAD)

dots = []
for e, xv, yv in pts:
    cx, cy = sx(xv), sy(yv)
    flagged = is_anomaly(e)                      # 警示点放大并换 terra 色
    fill = "#B4502E" if flagged else "#C8922E"
    r = 7 if flagged else 5
    dots.append(f'<circle cx="{cx:.0f}" cy="{cy:.0f}" r="{r}" fill="{fill}" fill-opacity="0.85"/>')
    if flagged or yv >= 标注阈值:                 # 只标注异常/极值，全标会糊
        dots.append(f'<text x="{cx:.0f}" y="{cy-10:.0f}" class="sct">{短名[:6]}</text>')

gx = []                                          # X 轴网格：对数轴挑整数刻度
for xt in (1000, 5000, 20000, 80000):
    if xmin <= math.log10(xt) <= xmax:
        X = sx(xt)
        gx.append(f'<line x1="{X:.0f}" y1="{PAD}" x2="{X:.0f}" y2="{SH-PAD}" class="grid"/>'
                  f'<text x="{X:.0f}" y="{SH-PAD+18}" class="axt" text-anchor="middle">{xt//1000}k</text>')
gy = []                                          # Y 轴网格
for yt in (2, 4, 6, 8, 10):
    Y = sy(yt)
    gy.append(f'<line x1="{PAD}" y1="{Y:.0f}" x2="{SW-PAD}" y2="{Y:.0f}" class="grid"/>'
              f'<text x="{PAD-8}" y="{Y+4:.0f}" class="axt" text-anchor="end">{yt}%</text>')

yref = sy(3.5)                                   # 阈值参考线（虚线 terra）+ 说明
scatter = f'''<svg viewBox="0 0 {SW} {SH}" class="scatter">
<rect x="{PAD}" y="{PAD}" width="{SW-2*PAD}" height="{SH-2*PAD}" fill="none" stroke="#ECE6DC"/>
{''.join(gy)}{''.join(gx)}
<line x1="{PAD}" y1="{yref:.0f}" x2="{SW-PAD}" y2="{yref:.0f}" stroke="#B4502E" stroke-dasharray="4 4" stroke-width="1"/>
<text x="{SW-PAD-4}" y="{yref-6:.0f}" class="axt" text-anchor="end" fill="#B4502E">阈值含义说明</text>
{''.join(dots)}
<text x="{SW/2:.0f}" y="{SH-8}" class="axl" text-anchor="middle">X 轴标题（对数）→</text>
<text x="14" y="{SH/2:.0f}" class="axl" transform="rotate(-90 14 {SH/2:.0f})" text-anchor="middle">Y 轴标题 →</text>
</svg>'''
```

## 5. 徽标 badge

异常/状态标注，四个语义类：

```html
<span class="b b-red">限流</span>      <!-- 红：功能性问题，已坐实 -->
<span class="b b-amber">需优化</span>  <!-- 琥珀：待改进/部分问题 -->
<span class="b b-jade">4★</span>      <!-- 玉：正面/达标 -->
<span class="b b-grey">0播·待核实</span> <!-- 灰：原因不明，不臆断 -->
```

判定函数写成显式规则（阈值/集合），别在渲染处即兴判断；判定口径必须进表格下方的 `.legend`。
**原因不明的异常一律 b-grey + 「待核实」**，不准升红（审查纪律）。

## 6. 热力总表

分组表头（第一行平台/分组色条，第二行具体指标）+ sticky 首列：

```html
<div class="tablecard"><div class="tscroll"><table>
<thead>
<tr>
  <th rowspan="2" class="ep" style="position:sticky;left:0;top:0;z-index:3;background:#F3EEE5">实体 · 日期 · 标签</th>
  <th colspan="2" class="grp" style="background:#5B8F7E">平台A</th>
  <th colspan="4" class="grp" style="background:#C44E5A">平台B</th>
</tr>
<tr><th class="num">指标1</th><th class="num">指标2</th>…</tr>
</thead>
<tbody>{rows_html}</tbody>
</table></div></div>
```

行渲染：首列 `<td class="ep"><b>{短名}</b><span class="meta">{日期} · {标签}</span></td>`，
数值列 `<td class="num" style="{heat(...)}">{fnum(v)}{badge}</td>`。
表后必跟 `.legend`（色阶 + 各徽标口径）和 `.note`（哪些数不可横比、缺数原因）。
实体名维护一份 `NAME = {id: 短钩子名}` 字典，表里用短名不用全标题。

## 7. 迭代建议面板 ITER

分平台多维建议，数据驱动渲染：

```python
ITER = [
 {"plat": "平台名", "role": "一句话定位", "c": "#5B8F7E",   # 平台主题色
  "ess": "该平台的机制本质一句话（含「(调研)」标注外部来源）",
  "dims": [
   ("维度名", "建议正文，关键动作加 <b></b>"),
   ("维度名 ⚑", "带 ⚑ 的渲染成 P0 红标"),
  ]},
]

def render_iter():
    out = []
    for it in ITER:
        dims = "".join(
            f'<div class="idim"><div class="idl">{d[0].replace(" ⚑", " <span class=&apos;p0&apos;>P0</span>")}</div>'
            f'<p>{d[1]}</p></div>' for d in it["dims"])
        out.append(
            f'<div class="iter" style="--c:{it["c"]}">'
            f'<div class="ihead"><span class="ipl">{it["plat"]}</span><span class="irole">{it["role"]}</span></div>'
            f'<p class="iess">{it["ess"]}</p>'
            f'<div class="idims">{dims}</div></div>')
    return "\n".join(out)
iter_html = render_iter().replace("&apos;", "'")
```

## 8. 执行 Checklist

```python
CHECK = [
 ("平台名", "一句话定位", "pr-1",          # pr-1 玉 / pr-2 琥珀 / pr-3 灰，按优先级
  [("动作描述，关键词加 <b></b>", 1),       # 1 = P0
   ("普通动作", 0)]),
]

def render_check():
    out = []
    for plat, role, pr, items in CHECK:
        lis = "".join(
            f'<li class="{"ck-p0" if p0 else ""}"><span class="ckbox"></span><span>{t}'
            + (' <span class="p0">P0</span>' if p0 else '') + '</span></li>'
            for t, p0 in items)
        out.append(
            f'<div class="ckl"><div class="cklh"><span class="ckpl">{plat}</span>'
            f'<span class="pr {pr}">{role}</span></div><ul class="cklist">{lis}</ul></div>')
    return "\n".join(out)
check_html = render_check()
```

每条 Checklist 必须是「可勾掉」的动作（动词开头、可观测完成态），不是方向描述。
