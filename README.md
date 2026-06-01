# product-skill

好价卡片（Good Deal Card）基础组件 — Claude Code Skill。

电商比价/优惠类信息卡片组件，适用于"什么值得买"的商品推送场景。

## 快速预览

直接打开 [reference.html](reference.html) 查看渲染效果。

## 主要特性

- **两种布局** (v1.1)：
  - 左图右文（信息流单条）
  - 上图下文（两列瀑布流，高度随内容）
- **5 色标签系统** (v1.2)：CSS 变量驱动，每色支持 描边 / 填充 / 填充+图标 三形态
- **自适应布局**：图片固定 115×115（水平版）/ 100%(上图下文)

## 版本历史

### v1.3 (latest)
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
├── reference.html        # 标准实现示例（含 3 张横版 + 2 张瀑布流）
├── image/
│   ├── badge.png         # 徽标图（如"绝对值"）
│   └── shoe.png          # 商品图占位
├── fonts/
│   └── price.ttf         # 价格特殊字体
└── icons/
    ├── comment.svg       # 评论图标
    ├── zhi.svg           # "值"图标
    ├── flash.png         # 闪电（限量抢/秒杀，配红色标签）
    ├── coupon.png        # 优惠券（国家补贴/领券，配绿色标签）
    ├── new.png           # 新品（新品上市，配深蓝色标签）
    └── time.png          # 时钟（限时/倒计时，配橙色标签）
```

## 在 Claude Code 中使用

把整个仓库放到 `~/.claude/skills/good-deal-card/`，然后在 Claude Code 里说：

> 用 good-deal-card 生成一个国家补贴（标签带图标）、限时抢（不带图标）的横版商品卡片

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
