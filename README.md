# 设计流水线 Skills

这是我给 Claude Code 写的一套设计工作流。目的很简单：一个需求从拿到 BRD 到开发上线验收，每一步让 AI 帮我干活，但关键判断留给设计师自己。

## 这套东西解决什么问题

以前让 AI 帮忙做设计，常见的问题有这几个：

- 它会自己编业务规则，编得还挺像回事
- 聊着聊着上下文就丢了，下一步不知道上一步定了什么
- 画 Figma 的时候乱写颜色和字号，不用组件库
- 交付给开发的说明写在开发看不到的地方

所以我把流程拆成几个阶段，每个阶段只干一件事，产出一个文件，下一步只看这个文件。

## 流程一览

顺序是：先在 Claude Design 里把方案铺开比 → 选定后做原型，自己点一遍交互 → 交互没问题了才画 Figma。Figma 里只画定下来的那一版，不在 Figma 里出好几个变体。

| 阶段 | 用哪个 skill | 干什么 | 产出 |
|---|---|---|---|
| S1 背景理解 | `design-s1-brief` | 读 BRD 和走查截图，把需求背景理清楚，不懂的地方追问 | `brief.md` |
| S2 方案决策 | `design-s2-decisions` | 把交互规则和边界情况一条条过完，AI 给推荐，设计师拍板；布局这类"长什么样"的分歧先只列出来，不在文字里选 | `decisions.md` |
| S3 方案发散 | `design-s3-explore` | 在 Claude Design 画布上，每个分歧画 2–4 个方案并排比，设计师选一个 | 方案比较画布 |
| S4 交互原型 | `design-s4-prototype` | 把选定方案做成能点的 HTML 原型，自己体验、拿去评审 | 可交互原型 |
| S5 Figma 落稿 | `design-s5-canvas` | 按确认过的原型在 Figma 里出 User Flow，只用组件库的组件和 token | Figma User Flow |
| S6 交付 | `design-s6-handoff` | 按画面写交付说明，再在 Figma 上按 Design Doc 模板写成中文说明给前端 | `handoff.md` + Figma Design Doc |
| S7 设计验收 | `design-s7-qa` | 从验收点生成检查表，设计师在测试环境点一遍，问题整理成能直接转给前端的清单 | `qa-checklist.md` |
| 评审（随时） | `design-review` | 把会议、评论、聊天里的评审意见整理归类，设计师决定采不采纳，再回写决策。最常用在原型阶段 | `review-log.md` |

## 几个我比较在意的规矩

- **AI 猜的东西要标出来。** S1 里 AI 推断的内容一律标 [推断]，设计师确认过才算数。
- **回一个 "ok" 不等于全部确认。** 用户只回 ok 时，AI 要把没确认的条目单独拎出来再问。
- **每条决策都要能验收。** 写不出"怎么检查它做对了"，说明规则本身没想清楚。
- **判断清单用大白话。** 让设计师一眼扫完，回一行"1 同意、3 改成 X"就能结束。
- **方案在画布上比，交互在原型里试。** 文字描述比不出哪个布局好，静态图也看不出交互顺不顺，所以先画、再点、最后才进 Figma。
- **原型里不放验收清单。** 方案还在变，清单会反复失效；能点出来的验收点先记在 STATUS.md，留到最后验收用。
- **Figma 里不许写死数值。** 颜色、字号、间距全部绑定组件库的 style 和 variable，画完还要自查一遍。
- **业务问题不由设计来定。** 设计定不了的，写清楚要找谁问。
- **评审意见采不采纳，设计师说了算。** AI 只负责归类、找冲突、提醒连锁影响；独立审查（不带上下文的子代理按清单审原型）的意见不能当真用来关闭待确认项。
- **交付说明写在开发看得到的地方。** 我们研发没有 Figma Dev Mode 席位，所以不用 Dev Mode 标注，改成画布上的中文 Design Doc。
- **Flow 和 Design Doc 用团队统一模板。** 不再各画各的，坐标、组件、命名都写在模板文件里。

## 目录

```
skills/
  design-s1-brief/       S1：需求背景梳理
  design-s2-decisions/   S2：交互规则与方案决策（references/ 里有一个完整示例）
  design-s3-explore/     S3：Claude Design 方案发散
  design-s4-prototype/   S4：交互原型
  design-s5-canvas/      S5：Figma 落稿
  design-s6-handoff/     S6：交付说明 + Figma Design Doc
  design-s7-qa/          S7：设计验收
  design-review/         评审意见整理与回写（随时用）
Design-Pipeline/
  PIPELINE.md              整个流程的总说明，AI 每次开工先读它
  figma-token-usage.md     什么场景用哪个 token
  component-usage.md       什么场景用哪个组件、选哪个变体
  user-flow-template.md    User Flow 统一展示模板（横幅 + Step 标签 + 页面横向一行）
  design-doc-template.md   Design Doc 统一展示模板（横幅 + 左列页面 + 右列中文功能说明）
  detail-page-template.md  Detail 页固定结构
  library-issues.md        组件库问题清单（S5 开工前查，发现新问题追加）
  templates/STATUS.md      每个需求的进度表模板
```

## 怎么用

1. 把 `skills/` 下面八个文件夹复制到 `~/.claude/skills/`
2. 把 `Design-Pipeline/` 放到你的工作目录（我用的是 Obsidian vault 根目录）
3. 在 Claude Code 里说"我拿到一个新需求"，或者直接说"跑一下 S1"，它就会开始

S3 要用 claude.ai 的 Claude Design（画布 + 你们自己的设计系统）。S5 和 S6 需要连上 Figma MCP，并且能访问你们的组件库和 Design Handoff 库。

## 用之前要自己补的

这份是公开版，公司相关的内容已经去掉了：

- 组件 key、variable id、Figma 节点和链接都换成了 `<key>`、`<node id>`、`<Figma 链接>` 这类占位符，要换成你们 library 里的真实值
- token 表、组件表、展示模板里的 token 名和组件名是我们 library 的命名，换成别的设计系统要自己重新整理
- `PIPELINE.md` 里引用的 Storybook 交互与文案规范在我们前端仓库里，没有放进来；你可以换成自己团队的规范文件
- `library-issues.md` 只留了格式和示例行
