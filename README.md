# biblatex-solution-to-latex-bibliography

latex中文参考文献的biblatex解决方案

针对\LaTeX 文档的中文参考文献问题，对其需求展开分析，提出利用biblatex宏包的解决思路，并针对性地给出了各项需求的解决方案(包括示例代码)，为\LaTeX 文档参考文献生成提供便利。

%% history:

%% 2016-12-07 v1.0  完成基本内容

%% 2017-01-05 v1.0a beamer类下的内容完善

%% 2017-01-18 v1.0b 修正一些错别字，加了后记

%% 2017-01-19 v1.0b 更正biblatex-gb7714-2015未考虑texlive2015兼容性所带来的问题，主要是修改gb7714-2015样式文件，具体可以参考biblatex-gb7714-2015。

%% 2017-03-15 v1.0c 增加了关于参考文献表文本转换成bib文件的内容，增加了nocite命令的介绍，增加了文献著录表中行溢出问题的解决方法，增加了关于&等特殊字符处理的介绍。

---------------------------------------------------------------

0 引言 1
1 需求分析 1
2 解决方案 2
2.1 数据源文件准备 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 2
2.1.1 手动文本文件生成 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3
2.1.2 利用 winedt 生成 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3
2.1.3 利用 texstudio 生成 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4
2.1.4 利用 Jabref 软件生成 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4
2.1.5 随LATEX文档生成 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4
2.1.6 获取标准 bib 文件 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 6
2.1.7 从 pdf 或文本文件转换 bib 文件 . . . . . . . . . . . . . . . . . . . . . . . . . 6
2.2 文档的组成 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 6
2.2.1 文档源文件基本结构 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 6
2.2.2 biblatex 宏包、参考文献数据源和样式的加载 . . . . . . . . . . . . . . . . . . 7
2.2.3 文献的引用命令 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 7
2.2.4 文献表打印命令 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 8
2.2.5 参考文献正反超链接 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 8
2.3 文档的编译 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 8
2.3.1 利用 winedt . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 8
2.3.2 利用 texstudio . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 9
2.3.3 命令行或脚本 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 9
2.4 分章参考文献和书后参考文献 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 11
2.4.1 利用 refsection 环境分章 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 11
2.4.2 利用宏包 refsection 选项分章 . . . . . . . . . . . . . . . . . . . . . . . . . . . 12
2.4.3 统一的全局参考文献 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 13
2.5 参考文献标题格式 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15
2.5.1 加入目录链接 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15
2.5.2 重定义 heading . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 16
2.5.3 利用 titlesec . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 21
2.6 参考文献内容格式 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22
2.6.1 一般设置方法 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22
2.6.2 自定义环境和局部修改 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22
2.6.3 自定义环境举例 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 24
2.6.4 参考文献表中的行溢出问题 . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27
2.7 参考文献著录和标注样式 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27
2.7.1 标准样式 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 28
2.7.2 gb7714-2015 样式 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 28
2.7.3 其他定制样式 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 29
2.7.4 快速定制/临时定制 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 29
2.8 多语言文献 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 31
2.8.1 动态方法 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 31
2.8.2 静态方法 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 32
2.9 脚注题注小页环境中的引用 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 33
2.9.1 脚注中的引用 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 33
2.9.2 题注中的引用 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 34
2.9.3 小页环境中的引用 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 34
2.10 脚注旁注中的文献表 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 35
2.10.1 使用 biblatex 命令实现脚注文献表 . . . . . . . . . . . . . . . . . . . . . . . . 35
2.10.2 使用 biblatex 命令和 footmisc 实现旁注文献表 . . . . . . . . . . . . . . . . . 37
2.10.3 使用自定义环境实现旁注文献表 . . . . . . . . . . . . . . . . . . . . . . . . . . 37
2.11 文献表分类筛选打印 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 39
2.12 beamer 类中的参考文献 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 40
2.12.1 文末文献表和脚注文献表 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 40
2.12.2 文献表中的条目序号 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 42
2.12.3 beamer 中 biblatex 参考文献表内容格式 . . . . . . . . . . . . . . . . . . . . . 45
3 结论 46
4 后记 46
目录 47
插图 48
示例 49

