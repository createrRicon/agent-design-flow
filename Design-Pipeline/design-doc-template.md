# Design Doc 展示模板（团队统一样式）

> 来源：设计师 2026-10-15 指定的参考（PO-Overview 文件 `FDz68bhPgjzl9tsbJIncRn` · section `<node id>`）
> 样板：样板需求文件 · Design 页 · section `<node id>`
> 与 User Flow 模板（user-flow-template.md）配套：Flow 讲"怎么一步步走"，Design Doc 讲"每一页的规则"

## 结构
```
Section（底色 background/subtle/info-subtle，key <key>）
├─ Part name 横幅（status=Design Doc）   x=71, y=86, w=2546, h=280
├─ 左列：页面 frame × N（1440×800）       x=71, y=466 起，步长 900（间距 100），命名 Page1…PageN
│    └─ 每页一个 Click marker：被说明区域外扩 4px 的矩形，只描边 2px、不加填充（设计师 2026-10-27 定，填充会盖住被说明的内容），
│       描边颜色绑定 background/functional/accent/accent（key <key>），圆角 4
└─ 右列：一个 Function description 面板  x=1612, y=466, w=1005
```

## Function description 面板
- 用 📚Design Handoff 的 Function description 组件（key `<key>`）detach 后改造
- 面板：padding 32 / 39 / 60 / 39，条目间距 8
- 每个条目：纵向，paddingTop 16，间距 8
  - 标题行：32px 橙色圆形编号（组件自带 Frame 595）+ 标题，间距 16；标题格式 `{页面/功能名} — Page n`（Inter Semi Bold 20，text/default/primary）
  - 正文：Figtree Regular 14，行高 180%，颜色 text/default/secondary（key `<key>`）
  - 正文写法：小标题单独一行（入口 / 列表 / 操作 / 筛选 / 表格 / 校验 / 结果…），下面用 `- ` 列点；小节之间空一行；跨页引用写 `— Page n`
- 编号与左列 Page 一一对应

## 页面
- 直接复用本需求 User Flow 的页面 frame（完整页面 + 浮层 / 弹窗 + 遮罩），每个 Page 只标一个重点区域
- 内容取自 handoff.md（中文），界面文案逐字给出
