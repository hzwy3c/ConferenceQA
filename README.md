# ConferenceQA

ConferenceQA是一个为学术会议问答任务而构建的数据集，它包含了7个不同会议信息并以半结构化的JSON格式组织，每个会议配有近100个人工注释的问答对，每个问答对根据回答的类型分成了四类。此数据集的目的是为了增强大型语言模型处理学术会议相关查询的能力，尤其是在知识准确性方面，以便研究人员和开发者更好地在这个方向上进行研究。

```
project-name/
│
├── src/                       # 源代码目录
│   ├── main.py                # 主程序文件
│   ├── module1.py             # 模块1
│   └── module2.py             # 模块2
│
├── data/                      # 数据目录
│   ├── train_data.csv         # 训练数据集
│   └── test_data.csv          # 测试数据集
│
├── notebooks/                 # Jupyter笔记本目录
│   ├── exploration.ipynb      # 数据探索笔记本
│   └── experiments.ipynb      # 实验笔记本
│
├── docs/                      # 文档目录
│   ├── README.md              # 项目README文件
│   └── CONTRIBUTING.md        # 贡献指南
│
├── tests/                     # 测试目录
│   ├── test_module1.py        # 模块1测试
│   └── test_module2.py        # 模块2测试
│
├── requirements.txt           # 项目依赖文件
└── setup.py                   # 安装脚本
```

## 数据集的收集方法

### 构建半结构化数据
为了构建ConferenceQA数据集，我们采取了手动和自动化的方法，将官方学术会议网站上的数据转换为半结构化的JSON格式。每个页面的标题作为JSON数据中的键或值的一部分，形成一种树状结构，以反映页面间的嵌套和并行关系。对于页面上的非结构化内容，如纯文本和小标题，我们将小标题作为路径提取，并将对应的内容作为值；同时为了增加粒度多样性，我们还对纯文本进行了更细致的分割。会议中结构化内容，如表格信息，我们通过网络爬虫获取并转换为对应页面路径下的半结构化数据。这样，我们最终得到了7个以半结构化JSON形式组织的会议数据集，它们可以作为准确可靠的知识库使用。

### 构建问答对
在创建ConferenceQA数据集的问答对时，我们结合了人工和自动化的方法，以确保每个问题都能反映人们在现实场景中可能提出的疑问。我们首先利用ChatGPT生成20个虚构研究人员的人物画像，包括年龄、研究方向、职位、历史发表论文和会议参与经验等细节。然后，使用prompt让ChatGPT代入每个角色并就每个会议提出五个不同粒度的问题，覆盖不同背景的角色对会议的兴趣或不确定性。最后，通过手动审核过滤重复或过于复杂的问题，并添加更具广泛性和多样性的问题。接着，我们根据半结构化的JSON数据手动标注答案，并为问答对中的每个答案注明来源，即答案在学术会议JSON数据中的位置，以确保数据集的可靠性。

### 问答对分类
为了评估模型回答不同难度问题的能力，我们设计了一个分类方案来区分问答对。这个分类主要基于两个方面：生成答案的过程以及生成正确答案所涉及的条目数量。第一个维度是"则归为extraction或reasoning"，它考虑生成答案的过程，如果答案可以直接从数据集中提取，即答案是数据集中的文本片段，则归为extraction；如果模型需要先进行推理然后生成答案，即相应答案不是数据集中的文本，则归为reasoning。第二个维度是"atomic或complex"，主要考虑生成正确答案所涉及的条目数量，如果答案的生成仅需要来自单个条目的信息，则归为atomic；如果答案的生成需要多个条目的信息，则归为complex。以上两个维度结合形成从简单到困难的四个类别：extraction atomic、extraction complex、reasoning atomic、reasoning complex。这一分类用于测试模型在不同复杂度和推理要求下的问答能力。

- 数据集的大小和结构（例如，行数、特征数）
- 每个特征的简要描述
- 数据集的预处理步骤
- 数据集的潜在用途和限制
- 数据集的许可和使用条款

## 结构感知方法
描述您设计的方法或相关的研究工作，并提供相关论文的引用和链接。

- 方法简介
- 方法的主要贡献
- 如何使用该方法处理/生成数据集

## 联系方式
如果有问题，请通过以下方式联系项目维护者：

邮箱: huangzww@zju.edu.cn

## 论文引用

如果您的数据集与一篇论文相关，请提供引用信息，例如：

```bibtex
@article{huang2023reliable,
  title={Reliable Academic Conference Question Answering: A Study Based on Large Language Model},
  author={Huang, Zhiwei and Jin, Long and Wang, Junjie and Tu, Mingchen and Hua, Yin and Liu, Zhiqiang and Meng, Jiawei and Chen, Huajun and Zhang, Wen},
  journal={arXiv preprint arXiv:2310.13028},
  year={2023}
}
