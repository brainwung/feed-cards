# product-skill

好价卡片（Good Deal Card）基础组件 — Claude Code Skill。

电商比价/优惠类信息卡片组件，适用于"什么值得买"的商品推送场景。

## 快速预览

直接打开 [reference.html](reference.html) 查看渲染效果。

## 主要特性

- **两种布局** (v1.1)：
  - 左图右文（信息流单条）
  - 上图下文（两列瀑布流，高度随内容）
- **自适应布局**：图片固定 115×115（水平版）/ 100%(上图下文)
- **价格特殊字体**：zhi字体
- **图文环绕标题**：标签 `<img>` float left，标题 2 行硬截断
- **一/二级标签系统**：红色强调（历史低价/折扣）+ 灰色中性（PLUS/补贴）
- **防重叠 footer**：渠道名 ellipsis 截断，时间/评论/推荐率始终完整显示
- **Retina 半像素描边**：标签使用 0.5px border

## 文件结构

```
.
├── SKILL.md              # Claude Code Skill 规范文档（15 节）
├── reference.html        # 标准实现示例
├── badge.png             # 徽标图（如"绝对值"）
├── shoe.png              # 商品图占位
├── fonts/
│   └── price.ttf         # 价格特殊字体
└── icons/
    ├── comment.svg       # 评论图标
    └── zhi.svg           # "值"图标
```

## 在 Claude Code 中使用

把整个仓库放到 `~/.claude/skills/good-deal-card/`，然后在 Claude Code 里说：

> 用 good-deal-card 生成一个 XXX 商品卡片

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
