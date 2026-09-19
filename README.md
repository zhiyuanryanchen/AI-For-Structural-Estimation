# AI 辅助的结构模型研究

以 Empirical IO 为核心的中文 Quarto 图书项目，面向高年级硕士研究生、博士研究生和研究者。

当前是**定位计划、23 章大纲与首个样章模板**，不是完成的教材或估计程序库。已经确定计算基础前置，生产函数、需求与市场势力、动态决策和战略互动等核心模块，以及贯穿式 AI 协作；前沿实验、真实数据案例及 Skills／Agents 尚处于规划阶段。

## 内容入口

- [全书定位与编写计划](planning/book-plan.qmd)：定位、案例、纸电分工、研究议程和 P0—P5 编写顺序。
- [首页](index.qmd)：读者定位与学习路径。
- [电子实验台](companion/lab-map.qmd)：实验编号、协作资源、权限与验收。
- [写作规范](companion/writing-guide.qmd)：章节、交叉引用、代码与多格式约定。
- [素材与参考资源](references.qmd)：已有讲义及技术依据。
- [Logit／Nested Logit 样章大纲](chapter-template/demand-model-sample-outline.qmd)：首个样章的理论、坑点、交互和验收框架。
- [协作指南](collaboration-guide/index.md)：GitHub 协作与 Quarto 写作入口。

## 首个样章与分工

首个样章从 Logit 需求模型出发，最终落到 Nested Logit，用于验证全书统一范式；BLP 留给后续正式章节。

- **罗茸**：负责理论主线、推导、参数约定与理论边界。
- **李法强**：负责 AI 交互问题设计、前置诊断、常见问题初步梳理和反馈路径。
- **陈志远**：负责项目整合、Quarto 载体和 GitHub 协作环境。
- **三人共同完成**：常见问题的理论原因、纸电取舍、联合审阅和最终成书。

## 预览与构建

在本目录运行，需要 Quarto。网页和目前的非执行书稿不依赖 Python 或 R。

```bash
quarto render --to html
quarto preview --to html
```

网页入口：`_book/index.html`。编辑 `.qmd` 源文件，不编辑生成的 HTML。

## 导出

```bash
quarto render --profile export --to docx --no-clean
quarto render --profile export --to epub --no-clean
quarto render --profile export --to pdf --no-clean
```

输出位于 `_exports/ai-structural-research.docx`、同名 `.epub` 和 `.pdf`。PDF 需要 XeLaTeX、ctex 与 Fandol 字体；禁用构建时自动安装 TeX 包。Word 和 EPUB 不保留“篇”层级，出版排版需另审。

常规配置只构建 HTML。导出配置不会覆盖 `_book/`；`--no-clean` 保留先前导出的其他格式，否则下一次构建会清理导出目录。导出当前包含策划页，仍为内部审阅稿。

## 目录职责

```text
_quarto.yml            网页配置与唯一章节注册表
_quarto-export.yml     Word、EPUB、PDF 导出配置
index.qmd              首页与读者路径
planning/              全书策划
chapters/              六篇二十三章，一章一文件
chapter-template/       未进入正式目录的样章模板
companion/             电子实验与写作规范
collaboration-guide/    GitHub 与 Quarto 初学者指南
references.qmd         本轮素材和官方技术入口
_book/                 生成的网页，忽略入库
_exports/              生成的文档，忽略入库
```

## 下一步编写与协作

优先确认并完成 [Logit／Nested Logit 样章大纲](chapter-template/demand-model-sample-outline.qmd)，收集理论 Slides、模拟数据、变量说明和既有作业，再实现分阶段 DeepSeek 交互与预设坑点。样章通过三人联合审阅和独立读者复现后，再规模化扩写其他章节。

GitHub 将作为正式协作工具，源码仓库为 <https://github.com/zhiyuanryanchen/AI-For-Structural-Estimation>，默认分支为 `main`。推荐流程为 Issue → 独立分支 → Commit → Push → Pull Request → 审阅合并。请先阅读 [Git 与 GitHub 协作指南](collaboration-guide/github-collaboration-guide.md)并完成一次练习。

项目已确定与中国人民大学出版社采用纸质书加配套网站的混合出版方向。具体访问权限、二维码、公开时间和许可尚未实施，待作者团队与出版社进一步确认。
