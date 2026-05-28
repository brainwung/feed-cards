---
name: good-deal-card
description: 生成"好价卡片"基础组件 — 电商比价/优惠信息卡片，支持两种布局：① 左图右文（信息流单条，固定 139px 高度）② 上图下文（两列瀑布流，高度随内容）。含商品图、徽标图、标题（图文环绕，2 行截断）、一/二级标签、价格区（特殊字体 + 整数突出）、来源信息行（渠道截断 + meta 不收缩）。当用户提到"好价卡片"、"商品优惠卡片"、"比价卡片"、"deal card"、"product card"时使用。
---

# 好价卡片 (Good Deal Card) v1.1

电商比价/优惠类信息卡片基础组件。设计稿基准 375px 视口移动端，**已实现自适应**。

## 0. 两种布局

| 布局 | 修饰类 | 高度 | 适用场景 |
|---|---|---|---|
| **左图右文（默认）** | `.deal-card` | 固定 139px | 信息流单条推送，纵向滚动列表 |
| **上图下文（v1.1 新增）** | `.deal-card.deal-card--vertical` | 高度随内容 | 两列瀑布流（推荐页/活动页/分类页） |

两种布局**共享所有内部组件样式**（标题/标签/价格/footer），只在容器层面用修饰类切换布局方向与尺寸。

## 1. 设计目标

- **价格优先**：当前价用特殊字体（PriceFont/DIN 工业风）+ 品牌红 `#E62828`，整数字号 16px 突出
- **可信背书**：通过一级红色标签（历史低价/百日新低/折扣）建立购买动机；二级灰色标签作中性补充
- **轻量信息**：来源、时间、评论数、推荐率以灰色辅助文字呈现，不抢主视觉
- **图文比例**：图片正方形 115×115 固定左对齐，信息区自适应

## 2. 整体结构

```
┌──────────────────────────────────────────────────────────────┐
│  ┌──────────┐                                                │
│  │          │  [徽标img] SK-II 神仙水 230ml 大瓶装           │
│  │  商品图   │  PITERA 精华                                   │
│  │  115x115 │                                                │
│  │          │  [历史低价] [PLUS专享] [跨店满减]              │
│  │          │                                                │
│  └──────────┘  ¥666.5 需用券 [6.5折] ¥̶1̶6̶9̶0̶                  │
│                                                              │
│                京东国际 │ 10:42         💬 8.8k  值 100%      │
└──────────────────────────────────────────────────────────────┘
```

## 3. 卡片容器规范

| 属性 | 值 |
|---|---|
| 高度 | **固定 139px** |
| 背景色 | `#FFFFFF` |
| 圆角 | 6px |
| 描边 | 无 |
| 内边距 | 12px |
| 图文间距 (gap) | 9px |
| 信息区上偏移 | `margin-top: -3px`（视觉顶对齐） |
| 页面背景 | `#F5F5F5` |
| 页面 padding | 12px |
| body max-width | 750px（PC 居中） |

## 4. 商品图 (左)

| 属性 | 值 |
|---|---|
| 容器尺寸 | 115×115px (`flex: 0 0 115px`) |
| 比例 | `aspect-ratio: 1 / 1` |
| 背景 | `#F5F5F5` |
| 圆角 | 3px |
| 图片填充 | `width:100% height:100% object-fit: cover` |

## 5. 标题区（图文环绕方案）

**关键技巧**：徽标 `<img>` 直接放在 `<h3>` 内首位，`float: left`，文字自动环绕。

```html
<h3 class="deal-card__title">
  <img class="deal-card__badge" src="./badge.png" alt="绝对值" />标题文字...
</h3>
```

| 元素 | 值 |
|---|---|
| 徽标 | `<img>` 替代文字标签，**高度固定 15px，宽度自适应** |
| 徽标 margin | `4px 4px 0 0` |
| 标题颜色 | `#333333` |
| 标题字号 | 14px |
| 标题字重 | 600 |
| 标题行高 | 1.66 |
| 标题截断 | `height: calc(14px * 1.66 * 2)` + `overflow: hidden` 硬截断 2 行（不用 ellipsis，不破坏 float） |

> ⚠️ 不能用 `-webkit-line-clamp`，会破坏 float 环绕。

## 6. 标签行

容器 `display: flex; flex-wrap: wrap; margin-top: 7px;`
标签间距用 `margin-right: 6px`（不用 `gap`，更可控），最后一个 `margin-right: 0`。

**两级标签共享尺寸**：

| 属性 | 值 |
|---|---|
| 高度 | 15px |
| 圆角 | 3px |
| 内边距 | `0 3px` 左右各 3px，宽度自适应 |
| 字号 | 11px |
| 行高 | 1（用 `inline-flex + align-items: center` 居中） |
| 边框 | **0.5px**（Retina 实显半像素） |

**一二级差异**：

| 类型 | 文字色 | 边框色 |
|---|---|---|
| 一级 (`.deal-tag--emphasis`) | `#E62828` | `rgba(230, 40, 40, 0.4)` |
| 二级 (`.deal-tag`) | `#666666` | `rgba(102, 102, 102, 0.4)` |

折扣徽标（如 `6.5折`）也归类为一级标签，复用相同样式。

## 7. 价格区

容器 `display: flex; align-items: baseline; flex-wrap: wrap; margin-top: 4px;`
段间距用 `margin-right: 3px`。

> ⚠️ `margin-top: 4px` 是为了抵消整数 `line-height: 20px` 上方约 4px 的 line-box 余白，**实际视觉间距 ≈ 8px**。

### 7.1 当前价（特殊字体 + 三段拼接）

```html
<span class="deal-card__price-current">
  <span class="symbol">¥</span><span class="integer">666.</span><span class="decimal">5</span>
</span>
```

字体链（PriceFont 自定义 + DIN/Impact fallback）：
```css
font-family: 'PriceFont', 'DIN Alternate', 'DIN Condensed', 'Oswald',
             'Bebas Neue', 'Impact', 'Helvetica Neue', sans-serif;
font-feature-settings: 'tnum';
font-variant-numeric: tabular-nums;
letter-spacing: 0.01em;
```

| 段 | 字号 | 行高 | 备注 |
|---|---|---|---|
| `¥` symbol | 13px | 15px | 字重 600 |
| `666.` integer | 16px | 20px | **小数点归整数部分** |
| `5` decimal | 13px | 15px | 仅小数数字 |

颜色 `#E62828`，字重 700，**baseline 对齐**（数字踩在同一基线上，整数向上突出）。

自定义字体 `@font-face`：
```css
@font-face {
  font-family: 'PriceFont';
  src: url('./fonts/price.ttf') format('truetype');
  font-display: swap;
}
```

### 7.2 凑单/用券提示

| 属性 | 值 |
|---|---|
| 颜色 | `#E62828` |
| 字号 | 13px |
| 行高 | 15px |

### 7.3 划线原价

| 属性 | 值 |
|---|---|
| 颜色 | `#999999` |
| 字号 | 13px |
| 行高 | 15px |
| 装饰 | `text-decoration: line-through` |
| 包裹标签 | `<s>` + `aria-label="原价 X 元"`（防 SR 误读） |

## 8. Footer 信息行（自适应防重叠）

容器：
```css
.deal-card__footer {
  margin-top: 7px;
  margin-right: -3px;     /* 让右侧 meta 距卡片边 9px */
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 9px;               /* 左右两部分最小间距 */
  color: #999;
  font-size: 12px;
}
```

### 8.1 左侧 source（可截断）

```html
<span class="deal-card__source">
  <span class="deal-card__source-channel">京东国际</span>
  <span class="deal-card__divider">│</span>
  <span class="deal-card__source-time">10:42</span>
</span>
```

- `.deal-card__source` 加 `min-width: 0; flex: 0 1 auto;` — **允许收缩**
- `.deal-card__source-channel` 渠道名加 `overflow: hidden; text-overflow: ellipsis; white-space: nowrap;` — **空间不足时截断为 …**
- `.deal-card__divider` + `.deal-card__source-time` 加 `flex: 0 0 auto;` — **始终完整显示**
- 行高 15px，分隔符颜色 `#D9D9D9`

### 8.2 右侧 meta（不收缩）

```css
.deal-card__meta {
  display: inline-flex;
  align-items: center;
  gap: 0;             /* 评论与值无间距 */
  line-height: 1.2;
  flex: 0 0 auto;     /* 不收缩 */
}
```

每个 meta-item：
- 图标 13×13px，`<img src="./icons/comment.svg">` / `<img src="./icons/zhi.svg">`
- 图标与数字间距 2px (`gap: 2px`)
- 数字宽度固定（防止抖动）：
  - 评论数字 `.deal-card__meta-value`: width 29px
  - 推荐率数字 `.deal-card__meta-value--zhi`: width 32px

**优先级**：宽度不足时只截断渠道名，分隔符 / 时间 / 整个右侧 meta 始终完整可见。

## 9. 颜色 Token

```css
:root {
  --deal-red: #FF2741;          /* 备用品牌红 */
  --deal-text: #1F1F1F;
  --deal-text-secondary: #666;
  --deal-text-muted: #999;
  --deal-border: #D9D9D9;
  --deal-border-light: #EAEAEA;
  --deal-image-bg: #F5F5F5;
  --deal-card-bg: #FFFFFF;
}
```

**实际使用色（直接 hex，不走变量）**：
- 强调红：`#E62828`（价格、需用券、一级标签文字）
- 一级标签描边：`rgba(230, 40, 40, 0.4)`
- 二级标签描边：`rgba(102, 102, 102, 0.4)`
- 标题：`#333333`
- 二级标签文字：`#666666`
- 划线原价 / footer 文字：`#999999`
- 分隔符 │：`#D9D9D9`

## 10. 字体规范

```css
font-family: -apple-system, BlinkMacSystemFont, "PingFang SC",
             "Hiragino Sans GB", "Microsoft YaHei", "Helvetica Neue", sans-serif;
```

价格数字使用独立字体链（见 7.1）。

## 11. 自适应规则

- **viewport**：`width=device-width, initial-scale=1.0`
- **html, body**：`width: 100%`
- **body**：`max-width: 750px; margin: 0 auto;`（移动端撑满，PC 居中）
- **图片**：`flex: 0 0 115px` 固定不缩
- **信息区**：`flex: 1; min-width: 0` 自适应（`min-width: 0` 必须，否则子级 ellipsis 失效）
- **footer 内部**：渠道名可截断，分隔符 / 时间 / 右侧 meta 不收缩

## 12. 资源文件清单

| 文件 | 用途 |
|---|---|
| `fonts/price.ttf` | 价格特殊字体 |
| `icons/comment.svg` | 评论气泡 |
| `icons/zhi.svg` | "值"图标 |
| `badge.png` | 标题前徽标（如"绝对值"） |
| 商品图 | 项目自有，建议 PNG 透明底 |

## 13. 数据接口

```typescript
interface GoodDealCardProps {
  image: string;
  imageAlt?: string;
  badgeImage?: string;            // 徽标图 URL（如绝对值/神价格）
  badgeAlt?: string;
  title: string;
  tags: Array<{
    text: string;
    emphasis?: boolean;           // true = 一级红，false = 二级灰
  }>;
  price: {
    current: string;              // "666.5" — 字符串避免精度问题
    original?: number;
    discount?: string;            // "6.5折"
    note?: string;                // "需用券"
  };
  source: string;                 // "京东国际"
  postedAt: string;               // "10:42"
  commentCount: string;           // "8.8k"
  recommendRate: string;          // "100%"
  onClick?: () => void;
}
```

## 14. 反例（绝对禁止）

- 不要使用渐变背景、玻璃拟态、霓虹色
- 不要给标签或卡片加阴影
- 不要把所有标签都做成红色（一级标签是稀缺资源）
- 不要使用纯黑 `#000` 作正文
- 不要使用 emoji 替代评论 / 推荐图标，必须用 SVG
- 不要使用 `border-radius: 9999px` (pill 形)
- 不要在卡片内嵌套另一张卡片
- 不要用 `-webkit-line-clamp` 截断标题（会破坏徽标 float 环绕）

## 15. 参考实现

参见 [reference.html](reference.html)，包含：
- 完整 CSS（含 `@font-face`、所有自适应规则、防重叠/截断）
- 三个示例（默认完整态、长标题截断、窄屏渠道名截断）
- 资源占位（badge.png / shoe.png / icons/*.svg / fonts/price.ttf）

资源文件位于同目录的 `assets/` 子目录下，使用时按需替换。

---

## 16. 上图下文布局 `.deal-card--vertical`（v1.1）

适用于**两列瀑布流**场景。所有内部元素（标题/标签/价格/footer）样式复用，仅容器与少数细节差异。

### 16.1 瀑布流容器

```css
.deal-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 5px;
}
```

- 两列等宽，列间距 5px
- 在 375 视口 + 5px body padding 下，每列约 180px

### 16.2 卡片容器差异

| 属性 | 水平版 (.deal-card) | 上图下文 (.deal-card--vertical) |
|---|---|---|
| flex-direction | row | column |
| height | 139px 固定 | auto 随内容 |
| padding | 12px | 0（图片撑满） |
| border-radius | 6px | **3px** |
| overflow | visible | hidden（保证图片裁圆） |

### 16.3 图片区

| 属性 | 值 |
|---|---|
| 宽度 | 100%（撑满列宽） |
| 比例 | `aspect-ratio: 1 / 1` |
| 圆角 | 0（继承卡片顶圆角 + overflow:hidden） |

### 16.4 信息区

| 属性 | 值 |
|---|---|
| padding | **9px**（四边一致） |
| margin-top | 0（覆盖水平版的 -3px） |

### 16.5 各区块垂直间距（覆盖水平版）

| 区块 | margin-top |
|---|---|
| 标签行 | 9px（水平版 7px） |
| 价格行 | 9px（水平版 4px） |
| Footer | 9px（水平版 7px） |

> 注：价格行因整数 `line-height: 20px` 有约 4px line-box 余白，实际视觉间距 ≈ 13px。

### 16.6 标题字号差异

| 属性 | 水平版 | 上图下文 |
|---|---|---|
| 字号 | 14px | 14px |
| 字重 | **600** | **400 常规** |
| 行高 | 1.66 (≈23px) | **20px 固定** |
| 截断 | `height` 硬定 2 行 | `max-height` 软截 2 行（短标题不留白） |

### 16.7 Footer 简化版（左侧只显商城）

```html
<div class="deal-card__footer">
  <span class="deal-card__source">
    <span class="deal-card__source-channel">天猫商城</span>
    <!-- 不再渲染 divider 和 source-time -->
  </span>
  <span class="deal-card__meta">...</span>
</div>
```

- 去掉 │ 和时间
- 右侧 meta（评论 + 推荐率）保留不变
- `margin-right: 0` 覆盖水平版的 -3px

### 16.8 单卡尺寸估算（375 视口下）

| 区块 | 高度 |
|---|---|
| 图片 | 180px |
| 信息区 padding-top | 9px |
| 标题（2 行满） | 40px |
| 标签行（9+15） | 24px |
| 价格行（9+20+余白） | 33px |
| Footer（9+15） | 24px |
| 信息区 padding-bottom | 9px |
| **合计** | **≈ 319px** |

短标题 1 行时少 20px → 约 299px。

### 16.9 完整示例

```html
<div class="deal-grid">
  <a class="deal-card deal-card--vertical" href="#" aria-label="...">
    <div class="deal-card__image-wrap">
      <img src="./img.png" alt="商品名" />
    </div>
    <div class="deal-card__info">
      <h3 class="deal-card__title">
        <img class="deal-card__badge" src="./badge.png" alt="绝对值" />商品标题
      </h3>
      <div class="deal-card__tags">
        <span class="deal-tag deal-tag--emphasis">历史低价</span>
        <span class="deal-tag">国家补贴</span>
      </div>
      <div class="deal-card__price-row">
        <span class="deal-card__price-current">
          <span class="symbol">¥</span><span class="integer">368.</span><span class="decimal">1</span>
        </span>
        <span class="deal-card__price-note">需用券</span>
        <s class="deal-card__price-original" aria-label="原价 699 元">¥699</s>
      </div>
      <div class="deal-card__footer">
        <span class="deal-card__source">
          <span class="deal-card__source-channel">天猫商城</span>
        </span>
        <span class="deal-card__meta">...</span>
      </div>
    </div>
  </a>
  <!-- 第二张卡片 -->
</div>
```

### 16.10 注意事项

- "高度随内容" 模式下，同一行两张卡片高度不齐平（标题字数/标签数量不同）
- 若需对齐，可改用 CSS Grid 的 `align-items: stretch`（默认）或为标题加固定高度
- 价格区在窄列宽下可能换行；如要避免，可减少同时显示的元素（如去掉 note 或 discount）
