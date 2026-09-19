# Quarto 与 `.qmd` 写作指南 {.unnumbered}

本书使用 Quarto book。作者编辑 `.qmd` 源文件，Quarto 把同一套内容编译为网页，并在需要时导出 Word、EPUB 和 PDF。`.qmd` 是带有 Quarto 功能的 Markdown 文本，不是编译后的网页。

## 一、只编辑源文件

主要目录：

```text
chapters/              正式章节
chapter-template/      尚未进入正式目录的样章模板
planning/              全书计划
companion/             电子实验与出版规范
collaboration-guide/   协作与写作指南
_quarto.yml            网页目录和章节顺序
_quarto-export.yml     Word、EPUB、PDF 配置
```

不要编辑 `_book/`、`_exports/` 或 `.quarto/`。这些目录由构建生成，下次编译会被覆盖，也不应提交到 Git。

## 二、`.qmd` 的基本写法

### 标题与段落

正式章节以一级标题开始：

```markdown
# 从消费者选择到 Logit 需求 {#sec-logit-demand}

这里是正文段落。段落之间留一个空行。

## 随机效用

### 外部选项
```

`#` 是章标题，`##` 是一级小节，`###` 是二级小节。不要手写“第 11 章”；Quarto 按 `_quarto.yml` 中的顺序自动编号。

内部模板可以使用一级标题但不注册到 `_quarto.yml`。只有经过确认的正式章节才加入图书导航。

### 强调、列表与链接

```markdown
**重要判断**，*英文术语或适度强调*，`代码与文件名`。

- 第一项。
- 第二项。

1. 第一步。
2. 第二步。

[PyBLP 官方文档](https://pyblp.readthedocs.io/)
```

中文正文使用全角标点和弯引号。命令、代码、路径、URL 与英文程序输出保留半角形式。

## 三、公式

行内公式用一对美元符号：

```markdown
价格系数记为 $\alpha$，产品 $j$ 的份额记为 $s_j$。
```

独立公式用两对美元符号：

```markdown
$$
s_j = \frac{\exp(\delta_j)}{1 + \sum_{k=1}^{J}\exp(\delta_k)}
$$
```

需要编号和引用的公式：

```markdown
$$
\log s_j - \log s_0 = \delta_j
$$ {#eq-logit-inversion}

由 @eq-logit-inversion 可恢复平均效用。
```

首次出现的符号必须解释。不要把截图当作公式，也不要直接粘贴未经核验的 AI 推导。

## 四、表格、图片与交叉引用

简单表格：

```markdown
| 对象 | 含义 |
|---|---|
| $s_j$ | 产品份额 |
| $s_0$ | 外部选项份额 |

: Logit 模型符号 {#tbl-logit-symbols}
```

正文写“见 `@tbl-logit-symbols`”，不要手写“见表 3”。

图片放入项目内稳定目录，例如 `figures/`：

```markdown
![不同巢结构下的替代模式](../figures/nested-logit/substitution.png){#fig-nested-substitution width=85%}
```

正文用 `@fig-nested-substitution` 引用。图片需要说明来源；AI 生成的示意图也要标注生成与人工核验情况。

章节使用稳定标签，如 `{#sec-production-functions}`，正文用 `@sec-production-functions` 引用。移动文件后检查相对图片路径和普通 Markdown 链接。

## 五、提示框与人机边界

Quarto callout 适合提示关键边界：

```markdown
::: {.callout-warning}
## 研究者必须判断
巢划分需要产品相似性和制度依据，不能只按拟合优度自动选择。
:::
```

建议统一使用：

- `.callout-note`：概念和补充说明。
- `.callout-tip`：可操作建议。
- `.callout-warning`：常见错误和 AI 风险。
- `.callout-important`：识别、授权或结论边界。

提示框不应取代连续论述。正文先解释原因，再用提示框帮助回查。

## 六、代码块与可执行代码

只展示、不执行的代码：

````markdown
```python
shares = np.exp(delta) / (1 + np.exp(delta).sum())
```
````

需要执行的代码使用 Quarto 的 Python 单元。下面先以不会执行的 `.python` 展示；实际写入 `.qmd` 时去掉圆点，改成 `{python}`：

````markdown
```{.python}
#| label: cell-logit-shares
#| echo: true

import numpy as np
```
````

本项目当前章节大纲不自动执行 Python。加入可执行实验前，先确定环境、依赖、随机种子、运行时间和输出目录。付费 API、外部模型和受限数据不得在默认构建时自动调用。

代码输出不是事实来源。公式、参数化和经济假设仍需独立核验；示例、伪代码和真实运行日志要明确区分。

## 七、纸质书与网页内容

核心理论、研究判断、坑点原因和结论边界必须在非交互格式中独立可读。仅对真正依赖浏览器的内容使用条件显示：

````markdown
::: {.content-visible when-format="html:js"}
打开交互实验 LAB-D01。
:::

::: {.content-visible unless-format="html:js"}
完整交互实验见配套网站 LAB-D01。
:::
````

`html:js` 指可运行 JavaScript 的网页，不包含 EPUB。条件隐藏只改变显示，不会阻止代码执行，不能用来保护敏感数据或避免 API 费用。

## 八、引用与参考文献

当前策划页使用普通链接。正式学术章节应逐步建立项目文献库和引用键，再用 Quarto／Pandoc 引用语法：

```markdown
Berry（1994）的份额反演可写为…… [@berry1994]
```

加入引用前核验作者、年份、题名、出处和引用论断。不要引用 AI 摘要代替原始文献，也不要保留无法核验的引用键。

## 九、本地预览与 HTML 编译

在项目根目录运行：

```bash
quarto preview --to html
```

终端会显示本地网址。保存源文件后，预览通常自动更新。结束预览可在终端按 `Ctrl+C`。

提交前运行完整 HTML 构建：

```bash
quarto render --to html
```

输出在 `_book/index.html`。至少检查：

- 新章节是否出现在正确位置。
- 章号和小节层级是否正确。
- 公式、表格、图片与中文是否正常。
- 交叉引用是否出现 `?@...` 等未解析标记。
- 手机宽度下表格和代码是否可读。

如果只是在 `chapter-template/` 编写未注册样章，完整 book 默认不会编译该模板。可以临时运行：

```bash
quarto render chapter-template/demand-model-sample-outline.qmd --to html --output-dir /tmp/quarto-template-preview
```

该命令只检查单个模板的基本语法；正式导航和跨章引用仍要等文件进入图书目录后验证。

## 十、导出 Word、EPUB 与 PDF

本项目使用独立 export profile：

```bash
quarto render --profile export --to docx --no-clean
quarto render --profile export --to epub --no-clean
quarto render --profile export --to pdf --no-clean
```

文件输出到 `_exports/`。`--no-clean` 很重要：省略时下一种格式会清理前一种导出结果。

PDF 需要 XeLaTeX、ctex 与 Fandol 字体。Word 和 EPUB 当前不保留 Quarto 的“篇”层级，但章节正文会保留。导出成功只证明文档构建链可用，不证明公式、代码或研究结论正确。

日常写作优先检查 HTML；样章定稿或提交出版社审阅前，再检查其他格式。

## 十一、常见构建问题

### 页面没有更新

确认保存的是 `.qmd` 源文件，而不是 `_book/` 下的 HTML。停止并重新运行 preview，查看终端是否报告错误。

### 章节没有出现在目录

确认文件已经加入 `_quarto.yml` 的 `book.chapters`，路径与大小写完全一致。模板文件未注册时不出现是正常行为。

### 交叉引用未解析

检查标签是否唯一、前缀是否正确，并使用 `@标签`。普通链接使用相对源路径，章节引用优先使用稳定标签。

### 图片找不到

图片路径相对于当前 `.qmd` 文件解析。确认文件已保存、扩展名和大小写一致，并已加入 Git。

### YAML 错误

YAML 依赖缩进，只用空格，不用 Tab。冒号后的值需要空格；包含特殊字符的标题用半角引号包围。

### PDF 失败但 HTML 正常

先看错误是否来自缺少 TeX、字体或 LaTeX 命令。不要为了让 PDF 通过而删除中文或公式。把完整错误信息交给维护者处理。

## 十二、一次章节修改的推荐流程

1. 在 GitHub Issue 明确章节任务。
2. 从最新 `main` 创建分支。
3. 修改一个 `.qmd` 源文件，小步保存。
4. 用 preview 查看当前页面。
5. 运行完整 HTML 构建。
6. 用 `git diff` 检查只改了预期内容。
7. Commit、push 并创建 PR。
8. 理论、交互或数据负责人完成对应审阅。
9. 根据意见修改，在同一 PR 更新。
10. 合并后同步 `main`。
