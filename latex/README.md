# Report 源码导航

`report.tex` 是编译入口，只负责加载配置和组织章节。正文按主题拆分，修改内容时直接打开下表对应的文件。

## 编译

在仓库根目录执行：

```bash
latexmk -cd -pdf -interaction=nonstopmode -halt-on-error latex/report.tex
```

或在 `latex/` 目录执行：

```bash
latexmk -pdf report.tex
```

两种方式均生成 `latex/report.pdf`。编辑子文件后，仍编译 `report.tex`，不单独编译子文件。

## 全局设置

| 文件 | 修改内容 |
| --- | --- |
| [report.tex](report.tex) | 文稿入口、章节顺序 |
| [config/preamble.tex](config/preamble.tex) | 宏包、主题、字体设置、图注、页脚、来源标注、水印、图片路径 |
| [config/metadata.tex](config/metadata.tex) | 标题、副标题、作者、单位、日期、PDF 元信息 |
| [sections/literature.tex](sections/literature.tex) | Reference 两部分的顺序 |
| [sections/literature/ai-research.tex](sections/literature/ai-research.tex) | Reference Part I 分隔页、各论文的顺序 |
| [sections/proposal.tex](sections/proposal.tex) | Proposal 各模块的顺序 |
| `figures/` | 图片和标志 |

## 正文索引

页码对应当前的 54 页 PDF；新增、删除或移动幻灯片后，后续页码会改变。

| 当前页码 | 内容 | 文件 |
| --- | --- | --- |
| 1 | 封面 | [frontmatter/cover.tex](frontmatter/cover.tex) |
| 2 | 目录 | [frontmatter/contents.tex](frontmatter/contents.tex) |
| 3–4 | Introduction | [sections/introduction.tex](sections/introduction.tex) |
| 5 | Reference Part I 分隔页 | [sections/literature/ai-research.tex](sections/literature/ai-research.tex) |
| 6–7 | Robin | [sections/literature/robin.tex](sections/literature/robin.tex) |
| 8–13 | Co-Scientist、scaling、Open Question、Scientific Exploration | [sections/literature/co-scientist.tex](sections/literature/co-scientist.tex) |
| 14–16 | XunZi | [sections/literature/xunzi.tex](sections/literature/xunzi.tex) |
| 17–19 | IdeaScientist | [sections/literature/idea-scientist.tex](sections/literature/idea-scientist.tex) |
| 20 | Other papers | [sections/literature/other-studies.tex](sections/literature/other-studies.tex) |
| 21–25 | Reference Part II、SECAR 实验优化 | [sections/literature/nuclear-astrophysics.tex](sections/literature/nuclear-astrophysics.tex) |
| 26–29 | Deepfield 分隔页、命名、Vision、验收标准 | [sections/proposal/overview.tex](sections/proposal/overview.tex) |
| 30–31 | Example Scientific Loop、MAS 角色与假设细化 | [sections/proposal/architecture.tex](sections/proposal/architecture.tex) |
| 32–38 | Data、Harness、Prompt、Context、Tools、Fusion、Validation | [sections/proposal/system-components.tex](sections/proposal/system-components.tex) |
| 39–44 | Research Directions | [sections/proposal/research-directions.tex](sections/proposal/research-directions.tex) |
| 45–46 | Benchmarks、独立评价 | [sections/proposal/evaluation.tex](sections/proposal/evaluation.tex) |
| 47–48 | Roadmap and Budget | [sections/proposal/roadmap-budget.tex](sections/proposal/roadmap-budget.tex) |
| 49 | Q&A / Thank You | [sections/qa.tex](sections/qa.tex) |
| 50–54 | 全文参考文献 | [sections/references.tex](sections/references.tex) |

## 定位和修改一页

每页保留原来的 `label`，用它定位比页码更稳定。例如：

- 第 12 页 Scientific Exploration：`sections/literature/co-scientist.tex` 中的 `label=md-10`。
- 第 27 页 Why Deepfield：`sections/proposal/overview.tex` 中的 `label=md-3`。
- Q&A：`sections/qa.tex` 中的 `label=qa`。

在 `latex/` 目录可按标签或标题搜索：

```bash
rg -n 'label=md-10|Scientific Exploration' sections/
```

一页由 `\begin{frame}` 到 `\end{frame}` 组成；前面的 `\setsource{...}` 设置该页的来源链接。移动页面时，将对应的来源命令一同移动；没有来源时用 `\setsource{}` 清空上一页的来源。

保留已有的 frame label 和参考文献 key，以维持目录跳转和引用。文件中的 `Source page: Deepfield.md Page ...` 注释是原始材料页码，不是当前 PDF 页码。

文件通过 `\input` 加载，不额外引入作用域或分页。新增主题文件时，在相应的章节入口添加 `\input{...}`；路径相对于 `latex/`。`template/` 保存早期模板，当前 report 不依赖它。

## 排版约定

通用样式集中在 `config/preamble.tex`：正文通常使用 `\small`，表格密集页使用 `\footnotesize`，图注使用 `\scriptsize`。正文中的蓝色小标题使用 `\slideheading{...}`；底部说明使用 `\slidenote{...}`，需要额外间隔时可写 `\slidenote[0.35cm]{...}`。这能避免小字号段落意外继承正文行距。

分隔页使用 `\sectiondivider{主标题}{副标题}`，统一粗体主标题和一行间隔。表格使用铺满正文宽度的 `tabular*`，普通表格行距系数为 `1.15`；内容较密或需要强调时可在单页微调。第 12 页保留简洁的大字布局，第 24 页的符号说明保留单行。
