# feed-cards

App 信息流基础卡片设计系统

包含多种卡片类型，共享同一套视觉语言（字体、颜色、标签系统、间距），适用于 App 首页 / 推荐流 / 活动页等需要混合排列多种信息单元的场景。

**当前包含的卡片类型**：
- ① **商品卡片 product-card** 
- ② **原创卡片 article-card** 
- ③ **视频卡片**（article-card 的 .deal-card--video 变体）

## 快速预览

直接打开 [reference.html](reference.html) 查看渲染效果。

## 主要特性

- **两种布局** (v1.1)：
  - 左图右文（信息流单条）
  - 上图下文（两列瀑布流，高度随内容）

- **自适应布局**：图片固定 115×115（水平版）/ 100%(上图下文)

## 版本历史

### v2.3 (latest)
- **视频变体** `.deal-card--video`：在 article-card 上叠加修饰类，封面叠加播放按钮
- 横版：图片正中央 45px 圆 + 21px 图标
- 竖版：右上角 18px 圆 + 10px 图标，距上/距右各 9px
- 黑底 40% 透明 + 4px 玻璃模糊（`backdrop-filter: blur`）
- 资源：`icons/play-fill.svg`
- 与所有现有变体兼容：常规标签 / 商品大标签 / 话题入口 / 图片比例

### v2.2
- **深色模式支持**：
- **瀑布流改为 JS 重排**：替代 v2.1 的 CSS Grid 方案，实现真正的紧密堆叠 + 重要性逐行视觉。无空白
- **话题入口** `.deal-card__topic`：竖版卡片标题上方的分类入口，13px topic-star 图标 + 11px `#999` 文字

### v2.1
- **商品大标签** `.deal-tag--product`：原创卡片专用标签，承载关联商品
- **竖版原创卡片**：`.deal-card.deal-card--vertical.deal-card--article` 三类叠加，瀑布流场景的原创卡
- **商品大标签置顶变体** `.deal-card__tags--top`：在竖版原创卡中将商品大标签置于标题上方
- **图片比例修饰类**：`.deal-card__image-wrap--3-4` / `--4-3`（默认 1:1），仅竖版原创卡可用
- **修复 .deal-grid `align-items: start`**：v1.1 隐藏问题，左右两列卡片此前被强行等高拉伸；现按内容独立计算
- **一级标签语义切换**：原创卡片现支持一级标签，但语义改为内容信号（精华/热门/编辑推荐/年度精选），颜色含义不变
- **竖版原创卡 footer 简化**：右侧仅保留点赞（隐藏评论），左侧作者区保留

### v2.0 — 信息流卡片设计系统
- **架构升级**：从单一卡片组件 → 多卡片设计系统，新增「卡片类型」分类
- Skill name: `good-deal-card` → `feed-cards`（**breaking change**：老名字不再触发）
- GitHub repo: `product-skill` → `feed-cards`
- 现有卡片归类为 **商品卡片 product-card**（CSS 类名 `.deal-card` 保留兼容）
- 新增 **原创卡片 article-card**（`.deal-card.deal-card--article`）：
  - 复用商品卡片骨架，价格行 `display: none` 隐藏
  - 标签仅二级灰（原创内容用中性分类）
  - Footer 左侧：18px 圆头像 + 用户名 + 12px 认证角标
  - Footer 右侧：thumb-up 图标替代值图标，显示点赞数
  - 资源：`icons/thumb-up.svg`、`icons/yellow-v.png`、`image/user.png`、`image/article.png`

### v1.3
- 新增**售罄状态** `.deal-card--soldout`：价格行加 `售罄 |` 前缀，整行变灰 `#999999`
- 标签形态收敛为 **2 种**：纯描边 / 描边+填充+图标（去掉中间形态 B）
- 标签行与价格行**单行溢出处理**：`flex-wrap: nowrap; overflow: hidden`
- 新增 30 行零依赖 JS（`fitOneLine`）：从右到左隐藏溢出整段，HTML 顺序即优先级
- SKILL.md 新增 6.6 标签行溢出 / 7.4 售罄态 / 7.5 价格行溢出 三节

### v1.2
- 标签系统重构：CSS 变量驱动（`--tag-color` / `--tag-border` / `--tag-bg`），新增色系仅需 3 行 CSS
- 新增 4 个色系：`--emphasis`(红 #E62828 限量抢&低价) / `--green`(绿 #10B354 国家补贴) / `--navy`(深蓝 #23386E 新品) / `--orange`(橙 #FFA129 限时抢)
- 新增 2 个修饰类：`--filled` 同色填充底（5%/10%）/ `__icon` 标签内 15px 图标
- 新增 4 套配套图标：`flash.png`（限量抢&低价）/ `coupon.png`（国家补贴）/ `new.png`（新品）/ `time.png`（限时）
- 横版示例从 1 张扩展到 3 张
- SKILL.md 第 6 章重写为 5 小节系统化规范

### v1.1
- 新增上图下文卡片 `.deal-card--vertical`（两列瀑布流，高度随内容，3px 圆角）
- 新增 `.deal-grid` 两列等宽容器（gap 5px）
- 完全复用 v1.0 的标题/标签/价格/footer 组件
- 页面 body padding 12px → 5px

### v1.0
- 好价卡片基础组件首个稳定版本
- 左图右文布局，固定 139px 高度
- 标题图文环绕（徽标 float + 2 行硬截断）
- 一/二级标签（红 / 灰）+ Retina 0.5px 描边
- 价格特殊字体（PriceFont/DIN）+ 整数突出
- Footer 防重叠（渠道名截断，时间/meta 不收缩）

## 文件结构

```
.
├── SKILL.md              # Claude Code Skill 规范文档
├── reference.html        # 标准实现示例
├── image/
│   ├── badge.png         # 徽标图（如"绝对值"）
│   ├── shoe.png          # 商品图占位
│   ├── article.png       # 原创卡片封面占位 (v2.0)
│   └── user.png          # 用户头像占位 (v2.0)
├── fonts/
│   ├── price.ttf             # 价格主字体（PriceFont，工业风加粗）
│   └── zhi-number-thin.ttf   # 价格细体（商品大标签用，v2.1）
└── icons/
    ├── comment.svg       # 评论图标
    ├── zhi.svg           # "值"图标（商品卡推荐率）
    ├── thumb-up.svg      # 点赞图标 (v2.0 原创卡)
    ├── topic-star.svg    # 话题入口图标 (v2.2)
    ├── play-fill.svg     # 视频播放图标 (v2.3)
    ├── yellow-v.png      # 黄 V 认证角标 (v2.0)
    ├── flash.png         # 闪电（限量抢/秒杀，配红色标签）
    ├── coupon.png        # 优惠券（国家补贴/领券，配绿色标签）
    ├── new.png           # 新品（新品上市，配深蓝色标签）
    └── time.png          # 时钟（限时/倒计时，配橙色标签）
```

## 在 Claude Code 中使用

把整个仓库放到 `~/.claude/skills/feed-cards/`，然后在 Claude Code 里说：

> 用 feed-cards 生成一个国家补贴（标签带图标）、限时抢（不带图标）的横版商品卡片

Claude 会读取 SKILL.md 与 reference.html，按规范生成卡片代码。

## 设计规范摘要

| 维度 | 值 |
|---|---|
| 卡片高度 | 139px |
| 卡片圆角 | 6px |
| 内边距 | 12px |
| 商品图 | 115×115px / 圆角 3px |
| 标签高度 | 15px / 圆角 3px / 边框 0.5px |
| 一级红 | `#E62828` |
| 二级灰 | `#666666` |
| 标题色 | `#333333` |
| 辅助文字 | `#999999` |

完整规范见 [SKILL.md](SKILL.md)。

## License

仅供个人/团队内部使用，请勿将 `fonts/price.ttf` 二次分发。
