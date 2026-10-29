---
name: design-s5-canvas
description: 设计流水线 S5——把原型确认过的方案落到 Figma，用 公司远程 library 的 style 和 variable 搭 User Flow。凡是要在 Figma 画布上生成或修改 产品界面、把原型或决策转成设计稿，或提到 S5、Figma 落稿时使用。要比较多个方案时先走 design-s3-explore（S3），不在 Figma 里出变体。
---

# S5 Figma 落稿

## 定位
- 输入：S4 确认后的原型链接 + decisions.md + 参考模板页的 Figma 链接
- 输出：Figma User Flow，只落已确认的那一个方案；覆盖决策中列出的所有状态
- 不出变体：方案比较在 S3 的 Claude Design 画布上已经做过
- 布局、状态、文案以原型为准；原型和 decisions.md 对不上时停下来问设计师，不自己挑
- 开工前检查 STATUS.md：S4 没确认就先问设计师是否继续
- 开工前先读 Storybook 交互与文案规范（路径见 `Design-Pipeline/PIPELINE.md`「全局规范位置」）；原型里的文案或按钮用法不符合规范时，列出来问设计师，不要照搬
- 写画布前先加载 figma-use skill

## 开工前：组件清点（先于画稿）
1. 按原型列出这次要用到的所有控件（按钮、输入、下拉、标签、提示、弹窗……），逐个对照 `component-usage.md` 和 `search_design_system`
2. 输出一张表：控件 · library 组件 + 变体 · 缺口（没有 / 有但不适用 / 属性不全）
3. 缺口先查 `Design-Pipeline/library-issues.md`：已有记录的照旧处理方式；新缺口追加进去，再报告给设计师，等回复后再画。不要画到一半才逐个打补丁

## Figma 页面怎么放
- **Design 页只放交付物**：User Flow section、S6 的 Design Doc section
- **Playground 页放过程稿**：方案对比、废弃方案、参考截图、评审前后对比
- 动手前先 `get_metadata` 列出文件里的页面和 section；设计师可能挪过 section，按实际位置找，不按上次记的 node id 写

## 铁律：不写任何硬编码值
颜色、字号、行高、间距、圆角、描边粗细，全部绑定 公司 library 的 style 或 variable。
用途与 token 的对应关系见 `Design-Pipeline/figma-token-usage.md`。

## 开工前：从模板读取 token（不要查 local）
组件库的 style 和 variable 都在**远程 library** 里。查 local styles/variables 会返回空，这不代表没有。
1. 打开用户给的模板节点，遍历子节点，读取实际绑定的 variable（boundVariables）和 style ID
2. 汇总成 token 表，与 figma-token-usage.md 对照；有出入的地方报告给用户
3. 获取 token 的顺序：
   a. importVariableByKeyAsync / importStyleByKeyAsync
   b. 导入失败（未发布）→ 直接复用模板节点上已解析的 style ID 或 variable
   c. 模板上也是裸值 → **保留模板的数值**，不要擅自换成其他 token，并列入报告

## 生成时
- createAutoLayout 会默认加白色 fill。布局容器（Row / Cell / Toolbar 等 wrapper）一律清空 fill；只有真正的表面（卡片、输入框、表头底纹）才带 fill
- **画任何控件前，必须先用 `search_design_system` 查 library 组件，并按 `Design-Pipeline/component-usage.md` 选择变体和属性。**不许凭印象决定用哪个组件、哪个 size
- **自建控件必须在报告里单独列出，并说明原因**（library 里没有 / 有但不适用，后者要说明哪里不适用）。自建不是默认选项，是需要交代的例外
- 同一组控件的 size 必须一致（例如弹窗底部的两个按钮）
- 每个区域最多 1 个 Primary；弹窗按钮：确认弹窗 No + Yes，配置、提交类表单弹窗 Cancel + Confirm
- UI 文案用英文，按 Storybook 规范（Title Case、确认弹窗文案库、提示文案写法）
- Detail 页的页面 frame 按 `Design-Pipeline/detail-page-template.md` 的固定结构搭（信息卡右上角主 CTA；Section 标题行右侧只放 CTA，状态和回显放内容区）
- 需要展示 User Flow 时，先读 `Design-Pipeline/user-flow-template.md`，按团队统一模板搭（Part name 横幅 + Step 标签 + 1440×800 页面横向一行），不加说明文字和箭头；规则说明留到 S6 的 Design Doc

## 生成后：必须审计
对所有新建节点检查，以下三项必须为空：
```
textWithoutStyle:    []
fillsWithoutToken:   []
strokesWithoutToken: []
```
不为空就修正，修不了的列入报告。

**再检查组件实例的属性**：遍历所有 INSTANCE，逐个核对变体、尺寸、图标开关是否符合 `component-usage.md`。
```
instancePropMismatches: []   // 每条写：实例名 · 当前属性 · 应为
```
不为空就修正，修不了的列入报告。同一组控件 size 不一致，算 mismatch。

然后用 decisions.md 对应条目的**验收点**逐条自检，说明每条在哪个 frame 里体现。

## 报告格式
1. 页面清单：frame 名称 + 覆盖了哪些状态；原型里有、静态稿没画的状态单独列出（S6 Design Doc 里补文字）
2. 验收点自检：逐条标注 ✓ / ✗ / 无法在静态稿中体现
3. 自建控件清单：哪些没用 library 组件、为什么
4. Design system 问题：未发布的 token、模板上的裸值、library 缺失的组件（交给设计师到 library 里修，不属于本需求的待确认项）；同步追加到 `Design-Pipeline/library-issues.md`
