# Markdown 中文报告转 Word（docx）构建规则

配合 [`SKILL.md`](SKILL.md) 与 [`structure-and-figures.md`](structure-and-figures.md) 使用。源文件用 Markdown 管理正文，交付用 docx；转换用 pandoc＋模板 reference-doc，再对生成的 XML 做少量后处理。一条可复用脚本完成全部步骤（参见项目内 `build_docx.ps1`）。

## 1. 转换链与分工

| 层面 | 做法 |
| --- | --- |
| 段落首行缩进 | 源文件段首两个英文空格标记段落；docx 由段落样式实现（`w:ind w:firstLineChars="200"`，即 2 字符），**不使用全角空格占位** |
| 标题样式 | 常用中文格式由样式定义实现：题目黑体二号居中；一级标题黑体三号加粗；二级黑体四号加粗；三级黑体小四加粗 |
| 中西文字体 | 中文宋体（正文）／黑体（标题），西文一律 Times New Roman（`w:rFonts w:ascii/w:hAnsi/w:cs`）；标题 eastAsia 仍为黑体 |
| 表格列宽 | pandoc 按表头分隔线短横线数量比例分配列宽；转换前按各列内容宽度（中文按 2 字符宽计）自动改写分隔线，序号等窄列自动收窄，无需手工调 |
| 图题表题 | docx 写入器会**丢弃原生 HTML**，图题表题须写成 Markdown 加粗文本行；转换后对含「图 N-x／表 N-x」的段落做居中＋缩进清零后处理 |
| 图片图注 | pandoc 会按图片 alt 文本自动生成第二行图注，与自编号图题重复；转换前把 `![alt](` 替换为 `![](` 清空 alt，只保留图题行 |

## 2. 后处理清单（对 docx 内 XML）

1. `word/document.xml`：图题表题段落（首 run 为「图／表」且次 run 为「N-M」）加 `<w:jc w:val="center"/>` 与 `<w:ind w:firstLineChars="0"/>`；注意图题文字会被拆成多个 run，不能按整串匹配。
2. `word/styles.xml`：按需补 `BodyText`／`FirstParagraph`（首行缩进 2 字符）与 `Heading1..4`（中文字号字体）；pandoc 引用的样式 ID 在模板中可能不存在（缺省回落 Normal，表现为「标题没有样式」）。
3. 表格列宽若需覆盖，改 Markdown 分隔线横线数，不改 XML。

## 3. 操作约定

- 源 Markdown 是唯一真源；docx 每次由脚本整卷重建，不在 Word 里手工改版式后反向保存。
- 重建前确认目标 docx 未被 Word 占用（占用时先关文件或输出到临时名再替换）。
- 转换后用 LibreOffice headless 渲染 PDF 抽查版式（图宽、居中、缩进、表格）。
- 表格内单元格、项目符号列表、标题、图题表题不缩进；正文段落才缩进。
