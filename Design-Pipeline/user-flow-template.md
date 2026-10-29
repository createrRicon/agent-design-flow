# User Flow 展示模板（团队统一样式）

> 来源：设计师 2026-10-12 确认的团队统一模板
> 样板：样板需求文件 · section `<node id>`（User Flow · 保存与使用筛选组）；原始参考 section `<node id>`

## 什么时候用
需求要在 Figma 里展示"用户一步步怎么操作"时，一律用这个模板，不再用自建的标题文字 + 箭头。

## 结构（全部横向一行）
```
Section（底色 background/subtle/accent-subtle）
├─ Part name 横幅（status=User Flow）      x=150, y=168, h=280，宽度 = 覆盖全部步骤
├─ Step 标签 × N                         y=513, h=48
└─ 页面 frame × N（1440×800）              y=626
```

| 项 | 规格 |
|---|---|
| 左右边距 | 150 |
| 步骤间距（x 步长） | 1567（= 1440 页面 + 127 间隔） |
| 标签 → 页面 | 标签 y=513，页面 y=626（间距 113 − 标签高 48 = 65） |
| Step 标签宽度 | 前 N−1 个 = 1567（线延伸到下一步），最后一个 = 1425 |
| 横幅宽度 | (N−1) × 1567 + 1440 |
| Section 尺寸 | 宽 = 150 + (N−1)×1567 + 1440 + 150；高 = 1583 |
| 排版 | **只横向一行**，不换行 |

## 组件（远程 library，按 key 导入）
| 用途 | 组件 | key | 用法 |
|---|---|---|---|
| 顶部横幅 | Part name（component set） | set `<key>` | 变体 `status=User Flow`；另有 `status=Design Doc` |
| 步骤标签 | Step（component） | `<key>` | 无属性，直接改文字；`resize` 宽度 |
| Section 底色 | variable `background/subtle/accent-subtle` | in-file id `<variable id>` | 绑定到 section fill |

## 步骤命名
`Step{n}-{英文短名}`，例如 `Step1-Default applied`、`Step4-Save as new filter`。分支步骤写成 `Step{n}-Branch · {说明}`。
Step 标签文字与页面 frame 名称一致。

## 页面 frame
- 用完整页面（侧栏 + 面包屑头部 + 内容），1440×800；以对应模块的列表页为底（例：现有列表页 `<node id>` 改造）
- 浮层 / 弹窗直接叠在页面上；弹窗背后加遮罩，fill 绑定 `background/default/mask`（key `<key>`）
- 全局提示用 **Message** 组件（set key `<key>`），白底 `Plain=True`、`Close=Off`，内容区顶部居中
- 不加步骤说明文字、不画箭头；规则说明放在中文 Design File（Function description）里
