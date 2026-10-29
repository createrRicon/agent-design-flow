---
name: design-s3-explore
description: 设计流水线 S3——在 Claude Design 画布上把 decisions.md 里的呈现分歧拓展成多个方案并排比较，设计师选定后回写 decisions.md。当用户要出几个方案、比较布局或入口位置、发散方案，或提到 S3、Claude Design、方案画布、方案比较时使用。
---

# S3 方案发散

## 定位
- 输入：decisions.md（规则已定；"呈现"段列出的分歧）+ brief.md 里的现有页面参考（Figma 走查页、截图）
- 输出：一张 Claude Design 方案比较画布；decisions.md 各分歧的"选定"
- 下游：S4 原型只做选定的方案
- 分工：AI 出方案、写优缺点；选哪个由设计师看过画布后决定

## 开工前
先读 Storybook 交互与文案规范（路径见 `Design-Pipeline/PIPELINE.md`「全局规范位置」）。每个方案都要守硬规则（每区最多 1 个 Primary、按钮 Title Case 等），不能靠违反规范来制造差别。

## 流程
1. 从 decisions.md 收集所有"呈现"分歧。没有分歧的决策不画方案，直接按规则进原型。
2. 用 Artifact 工具建画布：`quickstart` intent "design" → 用返回的 Design 类型创建，设计系统选 团队的设计系统。标题写 `<需求名> 方案比较`。
3. 每个分歧点一组，2–4 个方案并排：
   - 用同一份真实感数据（字段、样例取自 brief / BRD），只改要比较的那一处，其他保持一致，差别才看得出来
   - 画面贴近现有产品：沿用现有页面结构、组件和英文文案；不追求像素级，token 留到 S5 对齐
   - Detail 页先用 `Design-Pipeline/detail-page-template.md` 筛一遍：违反固定结构的方案（例如把回显、状态放进 Section 标题行右侧）不画，或画出来时在优缺点里写明"违反 Detail 页结构"
   - 每个方案下写两三行大白话：好在哪、问题在哪
   - 几个方案能互补时，再加一个"结合版"
4. 输出最后附判断清单（格式同 design-s2-decisions），设计师回"编号 + 选 X / 结合 X+Y / 再出"。
5. 选定后回写 decisions.md 对应条目的"呈现"段：选定、理由、画布链接；没选的方案保留一行说明，以后被问到有据可查。画布链接同步到 STATUS.md。

关卡：每个呈现分歧都有"选定"，才进入 S4。

## 报告
画布链接；每个分歧画了哪几个方案、各自一句话差别。
