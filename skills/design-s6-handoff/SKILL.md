---
name: design-s6-handoff
description: 设计流水线 S6——从 decisions.md 和定稿 Figma 生成交付说明：先写 handoff.md，再按 Design Doc 模板在 Figma 画布上用 Function description 组件写中文说明交给前端。当用户要交付设计稿、写标注、给开发写说明、出 Design Doc，或提到 S6、handoff 时使用。
---

# S6 交付

## 定位
- 输入：decisions.md（评审后版本）、定稿 Figma 链接、component-usage.md；STATUS.md 里的原型链接写在 handoff.md 顶部，方便开发对照交互
- 输出：`handoff.md`（源文件）+ Figma 里的 Design Doc section（开发实际看的地方）
- 前端主要看 Figma 上的 Design Doc，所以 handoff.md 是母本，Design Doc 从它生成，两者内容必须一致
- **不用 Dev Mode annotations**：研发没有 Dev Mode 席位，看不到

## 开工前检查（关卡）
- 读 STATUS.md：待确认列表不为空时，停下来列出未关闭的项，问设计师是否继续
- 试跑模式下可以继续，但 handoff.md 顶部和每条相关说明都必须写明"【试跑·含假设，不可开发】"
- decisions.md 里还有【假设】的条目，同样逐条列出

## handoff.md 结构
按**画面**组织，而不是按决策组织。开发是一屏一屏实现的，决策只是来源：
```
## <画面名>（Figma: <frame 链接>）
来源决策：D-x, D-y
状态：<这一屏有哪些状态，各自对应哪个 frame>
交互规则：<逐条写，从 decisions 的"规则"翻译成开发能直接实现的话>
边界情况：<空、错误、权限、切换……>
组件：<用到的组件 + 变体，对照 component-usage.md；同时写出代码里对应的写法（ui-button / ui-text-button / ui-icon-button / ui-tag、$toast、$confirm、el-alert），按 Storybook 规范；自建控件单独标出>
验收点：<从 decisions 复制，S7 会逐条检查>
```

## 写 Figma Design Doc
- 写画布前先加载 figma-use skill，并读 `Design-Pipeline/design-doc-template.md`，按其中的坐标、组件 key、命名搭建
- 结构：Part name 横幅（status=Design Doc）+ 左列 Page1…PageN 页面（每页一个 Click marker 框出重点区域）+ 右列一个 Function description 面板
- 页面直接复用本需求 User Flow 的页面 frame（见 `Design-Pipeline/user-flow-template.md`）
- 面板条目标题写 `{页面/功能名} — Page n`，编号与左列 Page 一一对应；正文用**中文**，内容取自 handoff.md，界面文案逐字给出
- 写完列出：每个 Page 写了哪些小节，以及有哪些规则没找到合适的 Page 承载

## 写法要求
- 对象是开发：写"点击 X 后 Y 出现"，不写"我们认为应该……"
- 不复述设计理由，理由留在 decisions.md
- 文案逐字给出（中英文以界面实际语言为准）
