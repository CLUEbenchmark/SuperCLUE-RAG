# SuperCLUE-RAG
中文原生检索增强测评基准

## 1. 介绍

随着人工智能技术的快速发展，大型语言模型在处理复杂、开放领域的问题时，常常面临知识获取和更新的挑战。它们所依赖的训练数据可能有限且过时，无法覆盖所有领域的知识，导致生成的内容缺乏准确性和时效性。
同时，在现实世界的应用场景中，用户期望获得最新、最准确的信息。

正是在这样的背景下，RAG（检索增强生成）技术结合了检索和生成两种方法的优势应运而生。RAG通过利用外部知识库中的信息，为语言模型提供了更全面、准确且最新的背景知识，使其在生成回答或文本时能够参考更多、更可靠的信息。
这不仅提高了模型的准确性，也使其更加实用和可信。同时，RAG方法还避免了昂贵的模型微调，允许模型在运行时动态地访问和更新知识库，从而提高了效率。

为了对国内外大语言模型的RAG技术发展水平进行评估并据此提出改进建议，我们发布了SuperCLUE-RAG（SC-RAG）中文原生检索增强测评基准，采用了不同于以往SuperCLUE评估方法的对比式测评模型，
依据不同的任务类型，全方位、多角度地对RAG技术水平进行测评。

#### 文章地址：https://www.cluebenchmarks.com/superclue_rag.html

#### 项目地址：https://github.com/CLUEbenchmark/SuperCLUE-RAG

## 2. SuperCLUE-RAG

### 2.1 特点

#### 1. 中文原生RAG应用综合能力评估

立足于为通用人工智能时代提供中文世界基础设施，文字输入或prompt提示词都是中文原生的；并充分考虑国内RAG技术的发展状况与应用场景，从国内RAG应用实际问题出发，致力于打造适合中国语义环境的RAG技术测评指标。

#### 2.多任务类型问答模型：综合考量RAG技术成熟度

基于RAG技术应用的主要场景，该测评指标设置了三种主要问答模式：

1. 无文档问答：考察系统对网页信息的捕获能力。

2. 单文档问答：主要考察系统对于敏感信息、错误信息、空缺信息的检索与反应能力。

3. 多文档问答：主要考察系统的信息整合能力。

#### 3.采用对比式评估方法：凸显RAG技术应用对系统整体能力的影响

不同于以往的测评体系，SuperCLUE-RAG还采用了对比式问答模式。除无文档问答类任务以外，针对同一问题进行先后两次提问，第一次不提供任何外部文档信息，第二次人为提供预设文档，对比两次答案的差异，并依据对比下暴漏的具体问题对系统RAG应用能力进行评估。

[图片]

### 2.2 评估方法与思路

参考SuperCLUE一贯的细粒度评估方式，构建专用测评集，每个维度进行细粒度的评估并可以提供详细的反馈信息。

#### 测评集构建
中文prompt构建流程：1.参考现有prompt--->2.中文prompt撰写--->3.测试--->4.修改并确定中文prompt

参考国际标准和当前已有工作，针对每一个维度构建专用的测评集。

#### 测评方法

评估流程：1.获得<中文prompt>-->2.依据评估标准-->3.使用评分规则-->4.进行细粒度打分

结合超级模型，在定义的指标体系里明确每一个维度的评估标准。结合评估流程、评估标准、评分规则，将文本输入送入超级模型进行评估，并获得每一个维度的评估结果。

进行评估与人类一致性分析，并报告一致性表现。

### 2.3 评价体系

测评体系分为评分标准与任务方向。

### 评分标准

  #### 1. 答案规整度：
     
     考察生成答案是否按照需求标准回答，强调要求文本具有清晰的因果关系、逻辑关系、时间顺序等，同时具有总结、分析的过程体现。
     
  - 例如：在“整理美国大选2024年以来各位候选人的动态数据并分析谁的当选概率更大”问题中，应该分别描述各位候选人的情况，且需要理清时间线和变化趋势，同时在分析谁更有可能当选的问题时，应该以描述内容为依据进行分析，而不是独立生成答案。
  
  #### 2. 答案准确度：
     
    考察生成答案的内容是否与提问的需求高度一致，确保答案直接且专注地解决问题的核心点。该指标不仅衡量答案的事实准确性，还包括其对问题的直接响应程度，并剔除与问题核心无关的信息，确保信息的紧凑性和目标导向性，答案文本中冗余信息的比例较低。
    
  - 例如：“请告诉我2023年诺贝尔文学奖获得者是谁”，应当准确回答2023年的诺贝尔文学奖获得者名单，而不包括其他年份的获奖者。
  
  #### 3. 信息提炼度：
     
    考察在描述性需求下，模型能够做到在相似文档或噪声文档中精准提取出补充信息，在全面、综合提炼文档信息的基础上，要求生成文本应当能够做到完善回答问题涉及的各个方面。
    
  - 例如：给定一系列企业的财报业绩文档如苹果、谷歌、华为、小米等，针对“请告诉我华为最新季度的收入增长以及各个分支盈亏的具体信息并反馈在文档中所体现的关键业务进展。”等问题，生成文本应当能够实现详细的反馈和整理。
  
  #### 4. 文本对齐度：
     
    考察生成文本与提供文档内容之间的关联性，应当能够呈现高相关度，内容紧密结合。
    
  - 例如：结合以上文档内容，梳理今年两会的最新进展，要求完全按照文档内容进行整理、归纳，则生成文本中的各项方针政策应当围绕今年两会的详细情况而展开叙述。

### 任务方向：RAG关键技术检测

  #### 1. 答案即时性：
  在最新的时间标准下准确回答问题，重点考察系统对最新信息的捕获能力。（该部分不提供文档，考察系统对于网页信息的捕获能力。）
  
  - 例如：假设今天是2024年3月6日，请系统回答2024年3月6日最新的A股指数。
    
  #### 2. 拒答能力：
  对于敏感问题或文档中不存在相关信息的问题应拒绝回答而非给出模糊答案。（单文档问题）
  
  - 例如：给定一系列企业的财报业绩文档如苹果、谷歌、华为、小米等，针对“请给出美国大选中拜登为什么退出竞选的理由”，应当以信息不完备而拒绝回答。
    
  #### 3. 检错&纠错能力：
  对于问题中有未更新的错误信息的，应当指出错误信息，修改后再返回。（单文档问题）
  
  - 例如：针对“目前已知最大的星系是距离银河系的IC 1101，直径约为400万光年，对吗？”，应当根据最新的文档信息生成“不对，目前已知的最大星系是距离银河系约30亿光年的阿尔库俄纽斯星系，直径约为1630万光年”。
  - 
  #### 2. 信息整合能力：
  根据提供的多条文档内容，能够具有多文档的检索记忆能力，并且根据检索的内容进行多步推理与整合，最终给出精确、完整的答案。（多文档问题）
  
  - 例如：根据提供的多段文档信息，整理归纳中亚五国在过去两个月内的动态与重大事件发展趋势。

### 2.4 问题设置与赋分说明

基于SuperCLUE-RAG测评体系的特殊性（对比式问答模式），全部题目基于RAG关键技术检测的四个任务方向设置问题，分为无文档问答、单文档问答、多文档问答三种形式，灵活采用评分标准进行赋分，进而得出多维评估结果。
[图片]

[图片]

## 3. 测评邀请

### 3.1 时间规划

报名：3月11日----3月22日

参测模型确认：3月22日

测评执行：3月15日--3月下旬

测评报告发布：3月下旬

### 3.2 测评流程

1. 邮件申请
   
3. 意向沟通
   
5. 参测确认与协议流程
   
7. 提供测评API接口或大模型
   
9. 获得测评报告
   
### 3.3 申请评测地址

邮件标题：SuperCLUE-RAG检索增强测评申请，发送到contact@superclue.ai

请使用单位邮箱，邮件内容包括：单位信息、文生视频大模型简介、联系人和所属部门、联系方式
[图片]
[图片]

