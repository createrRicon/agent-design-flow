# 设计流水线

Agent 每次开工前先读本文件，再读对应需求目录下的 STATUS.md。

## 目录结构
```
Design-Pipeline/          全局层：规则与模板，写一次、少改动
  PIPELINE.md
  templates/STATUS.md
Design-Projects/          单需求层：每个需求一个文件夹
  YYYY-MM-<需求名>/
    STATUS.md             当前阶段 + 待确认汇总 + 各阶段链接
    brief.md              S1
    decisions.md          S2（主干，S3 选定方案、评审后回写）
    prototype/            S4（可选：本地原型；发布成 Artifact 的不用建）
    review-log.md         评审
    handoff.md            S6
    qa-checklist.md       S7
```
走查截图和高保真稿留在 Figma，方案画布和原型留在 claude.ai，文档里只放链接。

## 顺序
先在 Claude Design 里把方案铺开比较 → 选定后做原型，亲手点一遍交互 → 交互确认后才落到 Figma。
Figma 只画已经确认过的方案，不再在 Figma 里出变体。

## 阶段
| 阶段 | skill | 输入 | 产出 | 关卡 |
|---|---|---|---|---|
| S1 背景理解 | design-s1-brief | BRD/PRD（md/html/pdf）+ Figma 走查页（改版必填，新建可无） | brief.md | 设计师确认所有 [推断] 条目；决策点 ≤8 条 |
| S2 方案决策 | design-s2-decisions | brief.md + design system 规范 | decisions.md（规则写定；呈现分歧只列方向，不在文字里选） | 设计师逐条判断规则 |
| S3 方案发散 | design-s3-explore | decisions.md 的呈现分歧 + 现有页面参考 | Claude Design 画布：每个分歧 2–4 个方案并排，可加结合版 | 每个分歧都已选定，回写 decisions.md |
| S4 交互原型 | design-s4-prototype | 选定方案 + decisions.md 规则 | 可交互 HTML 原型（Artifact 或 prototype/） | 设计师亲自体验确认；评审意见已处理 |
| S5 Figma 落稿 | design-s5-canvas | 确认后的原型 + decisions.md + 组件库 | Figma User Flow（只落定稿方案） | 审计三项清零 |
| S6 交付 | design-s6-handoff | decisions.md + 定稿 Figma + component-usage.md | handoff.md + Figma Design Doc（中文 Function description，不用 Dev Mode） | 所有待确认项已关闭（试跑除外，需标注） |
| S7 设计验收 | design-s7-qa | handoff.md 验收点 + STATUS.md 交互验收点 + 测试环境 | qa-checklist.md + 问题清单 | 设计师签收 |
| 评审（随时） | design-review | 评审意见（任意渠道）+ decisions.md + 画布 / 原型 / Figma 链接 | review-log.md，回写 decisions.md | 设计师逐条决定采纳与否 |

评审不是固定的一站：主要在 S4 拿原型给大家点，S3 选方案时、S5 之后也可能有意见进来，都走 design-review。

## 规则
1. 每个阶段只读上一阶段产出 + 全局规范，不从聊天记录里补上下文。
2. 任何阶段产生的【需确认】都要同步到 STATUS.md 的待确认列表。
3. 待确认列表没清空前，不能进入 S6。
4. 阶段完成后更新 STATUS.md 的当前阶段。
5. decisions.md 被改时，原条目状态改为"评审后改"或"原型后改"并写原因；评审引起的在 review-log.md 记录。
6. 试跑模式：被待确认项阻塞的决策，按推荐写入并标【假设】，状态记"试跑"。含【假设】的产出只用于验证流程，不可交付。
7. 原型里不放验收清单；能在原型里点出来的验收点记到 STATUS.md「留给验收的交互验收点」。
8. 进入 S5 后以 Figma 为准，原型不再跟着改；只有要重新评审交互时才回头改原型。
9. **S2–S5 动手设计前，先读 Storybook 交互与文案规范**（见下方"全局规范位置"）。规则、提示方式、弹窗、按钮、文案都按它来；和本目录的文件冲突时以 Storybook 为准，并报告给设计师。
10. 2026-10-20 前开的需求沿用旧编号（S3 高保真 / S4 评审 / S5 交付 / S6 验收），以各自 STATUS.md 为准，不强制迁移。

## 全局规范位置

### Storybook 交互与文案规范（设计前必读）
正本在代码仓库 `<前端仓库>/src/library/<组件库>/`，流程里只引用、不复制。只读设计相关的部分，props、scss、CONVENTIONS.md 这类写代码的规则不用看。
| 文件 | 管什么 | 主要用在 |
|---|---|---|
| `docs/START_HERE.md` | 四条硬规则：不用 el-button、每区最多 1 个 Primary、按钮 Title Case、只用 token | S2–S7 |
| `docs/patterns/FEEDBACK.md` | 四种提示怎么选（顶部 Message / 字段红字 / 页面提示条 / 确认弹窗）+ 提示文案写法 | S2 反馈类边界、S4–S6 文案 |
| `docs/patterns/CONFIRM_DIALOG.md` | 确认弹窗的标题、正文、Yes / No 和标准文案库 | S4–S6 |
| `packages/<组件>/README.md` | 只看"什么时候用 / 文案规范 / 不要这样用"几节（button、text-button、icon-button、tag、status 等） | S5 选组件、S6 写组件名 |

弹窗按钮分两种：**确认操作**（删除、撤销、提交前确认）用 Yes / No，按 CONFIRM_DIALOG.md；**配置类、提交类表单弹窗**（填内容、改设置）用 Cancel / Confirm。

### Figma 与流程模板
- Figma token 用途：Design-Pipeline/figma-token-usage.md
- User Flow 展示模板：Design-Pipeline/user-flow-template.md（团队统一，出 flow 时必须按此）
- Design Doc 展示模板：Design-Pipeline/design-doc-template.md（团队统一，出 Design Doc 时必须按此）
- 组件 usage guideline：Design-Pipeline/component-usage.md
- library 问题清单：Design-Pipeline/library-issues.md（S5 开工前查、发现新问题追加）
- Figma 文件页面：Design 页只放交付物（User Flow、Design Doc），过程稿和方案对比放 Playground 页
- Detail 页面固定结构：Design-Pipeline/detail-page-template.md（所有 Detail 页的画布、原型、Figma 都按此；Section 标题行右侧只放 CTA）
- Claude Design 设计系统：团队的设计系统（<设计系统 Artifact 链接>）
