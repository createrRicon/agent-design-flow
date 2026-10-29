# Detail 页面固定结构（团队统一样式）

> 来源：设计师维护的样板 `Design Guidelines` 文件 · Detail Page · 样板 Detail 页（node `<node id>`）
> <Figma 链接>
> "分组标题右侧只放 CTA"一条：设计师 2026-10-20 确认（某需求比较方案 A / B 时提出）
> 适用：所有 Detail 页（各业务对象的 Detail 页）的设计稿、原型和方案画布

## 什么时候用
做任何 Detail 页时都按这个结构搭：S3 方案画布、S4 原型、S5 Figma 落稿都一样。方案之间只能改需求要比较的那一处，外壳不动。

## 结构
```
Page（1440×800）
├─ Sidebar（240，UI Kit Sidebar · Theme=Dark）
├─ Header（56，"面包屑+消息+头像" 组件：左面包屑，右通知 + 头像）
└─ Content（距侧栏、顶栏、右边、底边各 16；卡片之间间距 12）
   ├─ 信息卡（Info card）
   │   ├─ 第 1 行：返回箭头 + 标题（记录 ID）+ 状态 Tag …………… 右侧：页面主 CTA
   │   └─ 第 2 行：字段网格（等宽多列，label 在上、值在下）
   └─ 详情卡（Detail card）
       ├─ Tab 行
       └─ Section × N
           ├─ 标题行：左「标题 + (数量) + 状态」…………… 右「CTA」
           └─ 内容区：表格 / 列表 / 回显 / 空态 "No Data"
```

## 各区域规则

### 信息卡
- 第 1 行左侧：返回箭头、记录 ID（页面标题）、状态 Tag。
- 第 1 行右侧：**只放整页级的主操作**（如 Submit），用 Primary Button。整页只有一个主 CTA 时放这里，不放进 Tab 内容。
- 第 2 行：只读字段，等宽多列（样板为 4 列）；label 用 Tiny-regular / text/default/tertiary，值用 Body-regular / text/default/primary，label 与值间距 4。

### 详情卡
- Tab：文字 Tab，选中项下方 40×4 的品牌色指示条（圆角 2）；Tab 之间间距 8。
- Tab 内的批量操作（如 Add Row、Excel Upload）放在 Tab 内容顶部的工具栏，左对齐。（来源：现有 Detail 页设计）
- 校验提示条（如 "N validation errors remaining · View"）放在工具栏上方。（来源：现有 Detail 页设计）

### Section 标题行（重点）
- **左侧**：标题（Large-medium）、数量 "(n)"（text/default/tertiary）、状态（绿勾，或带图标的警示文字如 "Image required"）。
- **右侧只放 CTA**：Text Button（size=small · border=off，可带图标），多个 CTA 右对齐并排。
- **状态和回显不放右侧**：文件名 chip、计数（如 "1/1"）、缩略图、说明文字等属于数据，一律放在内容区。理由：右侧是操作区，混进数据后，以后增加 CTA 时回显会夹在按钮中间，看起来像按钮，也对不齐；同一位置在不同状态下一会儿是按钮、一会儿是数据，用户会困惑。
- 可折叠的 Section：箭头放在标题前；**展开 / 收起的点击区只包括箭头和标题**，不包括整行，避免和右侧 CTA 冲突。
- 标题行下方有一条分隔线（border/default）。

### Section 内容区
- 表格、列表、上传回显（如 "Photo 1/1" + 文件名 chip）、空态都放这里。
- 空态文字用 Body-regular / text/default/tertiary，如 "No Data"。

## 尺寸与 token
| 项 | 值 | token |
|---|---|---|
| 页面底色 | 白色，与样板一致（设计师 2026-10-20 定；卡片靠 1px 描边区分）。设计系统 README 写的是 gray-50 内容区，Detail 页以本文件为准，README 暂不改 | background/default/White |
| 卡片 | 白底、1px 描边、圆角 | background/default/card · border/default · Radius/Base |
| 卡片内边距 | 上下 12、左右 16 | Spacing/12 · Spacing/16 |
| 卡片之间 | 12 | Spacing/12 |
| Section 之间 | 16 | Spacing/16 |
| 信息卡两行之间 | 8 | Spacing/8 |
| 字段列间距 | 16（样板是 10，不在 token 档位里） | Spacing/16 |

## 样板目前不合规的地方（交给样板维护者修）
2026-10-20 检查 样板 Detail 页 发现：页面底色（白）和卡片圆角是裸值，应分别绑 background/default/White、Radius/Base；部分 gap 没绑 token（字段列间距 10 不在档位）；侧栏是 detach 的 frame 且底色绑了 text/color/brand；Tab 是自建的（library 有 Tabs-top）；根层有两个空矩形和一个隐藏的 12 列栅格残留。这些不影响上面的结构规则。
