# Figma Token 用途表

S5 Figma 落稿时按用途选择 token。所有 token 都在远程 library，不在 local。
校验基准：样板需求文件 · 样板 Detail 页（节点 <node id>）· 2026-09-24 实测。

## 取用方式（重要）

library 的 token 分两类，取法不同：

| 类别 | 取法 |
|---|---|
| 可按 key 导入 | `importVariableByKeyAsync` / `importStyleByKeyAsync` 直接拿 |
| **别名变量，只能按 in-file ID 复用** | 先从模板节点读出已解析的 ID，再 `getVariableByIdAsync` |

属于第二类的：`Radius/Base`、`Border/1`、以及全部文字 style。它们**已发布且在模板上正常使用**，只是按 key 导不进来。旧表写的"未发布"不准确——是发布了但 key 不可导。

## 文字 style（全部只能按 in-file ID 复用）

| 用途 | style | 实测 |
|---|---|---|
| 页面标题 / 弹窗标题 | Title/H1-bold | Figtree SemiBold 24 |
| Tab / 区块标题 | Large-medium | Figtree Medium 16 |
| 表格正文 / 输入值 | Body-regular | Figtree Regular 14 |
| 链接文字 / 强调值 | Body-medium | Figtree Medium 14 |
| 字段标签 / 表头 / 辅助说明 | Tiny-regular | Figtree Regular 12 |
| 记录数 | Content/Middle-regular | Figtree Regular 14 |

模板上另有两个未登记的 style，可用：`Large-regular`（16 Regular）、`Content/Small-regular`（13 Regular）。

## 颜色 variable（可按 key 导入）

| 用途 | variable |
|---|---|
| 主文字 | text/default/primary |
| 次要文字 / 标签 | text/default/tertiary |
| 占位符 | text/default/placeholder |
| 链接 | text/color/brand |
| 卡片底色 | background/default/card |
| 页面底色 / 骨架屏 | background/default/subtle |
| 描边 | border/default |
| 错误文字 / 错误描边 | text/color/error · border/error |
| 错误底色 | background/functional/error/error-subtle |
| 警示文字 / 底色 | text/color/warning · background/functional/warning/warn-subtle |
| 品牌底色（主按钮） | background/functional/brand/brand |
| 选中项高亮（下拉选中项、已选标签） | 底 background/functional/brand/brand-subtle · 字 text/color/brand · 描边 brand-disabled |

## 尺寸 variable

| 用途 | variable |
|---|---|
| 间距 | Spacing/2 · 4 · 8 · 12 · 16 · 24 |
| 容器圆角 | **Radius/Base**（模板所有容器绑的是它，不是裸值 6——旧表那条【需确认】已关闭） |
| 控件圆角（输入框、表格、骨架条） | Radius/small = 4 |
| 全圆（头像、徽标） | Radius/full = 9999 |
| 描边粗细 | Border/1 |

## 已知问题（library 维护，交给设计师到 library 里修）

1. **命名重复**：`Radius/16`、`Radius/24`、`Radius/4` 与 `Spacing/16`、`Spacing/24`、`Spacing/4` 是同一个变量的不同名字（key 完全相同），而且在模板里被用作 padding。一个叫 Radius 的变量控制内边距，读代码和读稿都会误解。
2. **命名不一致**：`Space/6` 与其余 `Spacing/*` 不同前缀。
3. **同 key 双名**：`background/default/card` 与 `background/default/White` 指向同一个变量。语义上卡片底色和纯白应该可以分开演进，现在绑哪个都一样。
4. **`Color / Primitive` collection 为空**（0 个变量），语义色全部挂在 `Color / Semantic` 下，没有 primitive 层。
5. `Radius/Base`、`Border/1` 及全部文字 style 无法按 key 导入，每次生成都要先从一个模板节点里捞 ID。建议在 library 里确认这些是否应该独立发布。
