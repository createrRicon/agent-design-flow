# 组件选用表

来源：🧩UI Kit library，2026-09-24 用 `search_design_system` + 组件属性实测。
S5 在 Figma 里画任何控件前先查这张表；表里没有的，先搜 library，仍然没有才自建，并在报告里单列。

## 按钮

| 用途 | 组件名 | 推荐变体 | 属性设置 |
|---|---|---|---|
| 主操作（表单弹窗 Confirm；确认弹窗 Yes） | Primary Button | status=default · size=small（弹窗内）| **icon=off**（纯文字操作不带图标） |
| 主操作禁用 | Primary Button | status=disabled · size 与同组按钮一致（弹窗内 small） | icon=off |
| 主操作提交中 | Primary Button | status=loading · size 与同组按钮一致（弹窗内 small） | icon=off |
| 次操作（表单弹窗 Cancel；确认弹窗 No） | **Secondary Button** | status=default · size=small（弹窗内）| **icon=off** |
| 弱操作 / 内联动作（Re-upload file） | Text Button | status=default · size=medium · border=off | ⚠️ 见下方图标说明 |
| 图标操作（关闭、更多） | Icon Button | status=default · size=small · border=off | 通过 INSTANCE_SWAP 换图标 |

**弹窗按钮文案分两种**：确认操作（删除、撤销、提交前确认）用 **No + Yes**，标题和正文按 Storybook `CONFIRM_DIALOG.md`；配置类、提交类表单弹窗用 **Cancel + Confirm**。每个区域最多 1 个 Primary，按钮文案 Title Case（见 Storybook `START_HERE.md`）。

**同一组按钮 size 必须一致。** 弹窗底部两个按钮统一 **size=small**（2026-10-08 设计师在某个需求中定为 small，原为 medium）。弹窗 Body 内容间距统一 Spacing/8。

⚠️ **Text Button 的变体里焊死了一个 `🧬 circle-plus` 图标**，属性只有 status / size / border，没有 icon 开关。纯文字动作只能覆盖图层可见性（把那个 INSTANCE 子层设为不可见）。建议给 library 的 Text Button 补一个 `icon` 变体属性，与 Primary / Secondary Button 对齐。

## 输入

| 用途 | 组件名 | 推荐变体 | 属性设置 |
|---|---|---|---|
| 单行输入（编号类字段） | Input | State=Default · Size=Default · Input=True | Show-prompt=false；Prefix-icon / Suffix-icon 按需 |
| 输入错误态（编号不匹配） | Input | **State=Error** · Size=Default | **Show-prompt=true**，Prompt-text 写错误说明 |
| 输入聚焦 | Input | State=Focus | — |
| 输入禁用 / 只读 | Input | State=Disable | — |
| 多行输入（技术文档描述） | Input | State=Default · Size=Default | **拉高 Input 实例**到所需高度，不新增 Textarea 组件。内容顶端对齐 |

Input 自带 `Show-prompt` + `Prompt-text`，字段级错误说明应该用它，不要另外堆文本节点。

⚠️ **Input 的 message 图层是绝对定位在 y=32 的**，不占布局高度。开了 `Show-prompt` 之后，外层字段容器必须留出底部空间（`paddingBottom` 绑 `Spacing/24`），否则提示会被下一个字段盖住——看起来像没生效。

## 选择

| 用途 | 组件名 | 推荐变体 | 属性设置 |
|---|---|---|---|
| 单选未选（Art. 条款） | Radio | State=Default · Size=Default · Border=False | Text 留空，列头已说明 Yes/No/N.A. |
| 单选已选 | Radio | State=Selected · Size=Default · Border=False | Text 留空 |
| 单选禁用 | Radio | State=Disable(Default) / Disable(Selected) | — |

Radio 的 `Border=True` 是带边框的卡片式选项，本弹窗的表格矩阵用 False。

## 反馈与容器

| 用途 | 组件名 | 推荐变体 | 属性设置 |
|---|---|---|---|
| 短时加载指示 | Loading | status=middle · Text=Off | 仅用于 spinner 场景 |
| 全局提示（保存成功、操作失败） | **Message**（set key `<key>`） | Type=Success / Danger 等 | **Plain=True（白底）· Close=Off**；放内容区顶部居中。2026-10-12 起替代旧 Alarm，不再自建 toast |
| 弹窗容器 | Dialog | 无变体属性 | **用 Dialog 保证外框与容器间距**（圆角、内边距、分区节奏），**内部信息自定义**填充 |
| 气泡提示 | Popover | Layout 按位置选 | 气泡指向型提示，**不能**当横幅用 |
| 文字提示 | Tooltip | — | hover 提示 |
| 标签 | Tag | Close=False · Style=Default · Size=Small · Topic=Light | component set key 导不进来，复用文件里已有实例的 main component。品牌色标签（如 Default）：底 brand-subtle、描边 brand-disabled、字 text/color/brand |

**选中态统一用系统品牌高亮**（下拉选中项、已选标签）：底色 `background/functional/brand/brand-subtle`，文字 `text/color/brand`。不要用灰底或自定颜色（2026-10-12 设计师指定）。

## library 里没有的（必须自建，报告里单列）

| 用途 | 现状 | 建议 |
|---|---|---|
| **骨架屏**（识别中） | 无组件。`Loading` 是 spinner，不是骨架 | 【需确认】自建骨架条（矩形 + background/default/subtle + Radius/small），还是给 library 加 Skeleton 组件 |
| **阻断条 / 通栏提示**（无签名、条款不合规） | 无 Alert / Banner 组件。Popover 是气泡提示，语义不符 | 【需确认】自建通栏条，还是给 library 加 Alert 组件。这是 D-D 三档提示的主要载体，建议补进 library |
| 文档预览容器 | 无组件 | 自建，属业务专用，不必进 library |

## Dialog 容器的用法

Dialog 组件没有任何 slot 或属性，实例的子层不能增删。要放自定义内容，做法是：实例化 → `detachInstance()` → 三个分区（Header / Body / Footer）也各自 detach。detach 后 padding、gap、圆角的变量绑定全部保留，容器间距即继承自组件。

分区间距（来自组件本身）：Header `16/24/0/24` · Body `16/24/16/24` gap `16` · Footer `12/24/12/24` + 顶部分隔线。

建议给 library 的 Dialog 加 INSTANCE_SWAP 插槽，这样就不必 detach。

## 建议补进 library

| 组件 | 理由 |
|---|---|
| Alert / Banner | D-D 三档提示的主要载体，目前只能自建 |
| Skeleton | 识别中等待态，`Loading` 只是 spinner |
| Text Button 的 `icon` 属性 | 与 Primary / Secondary Button 对齐 |
| Dialog 的内容插槽 | 避免每次 detach |
