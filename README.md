# -interview

面试官你好，我叫xxx，拥有5年软件工程师从业经验，本人熟练掌握Java相关生态技术，熟练运用Spring，Spring Boot，Spring Cloud以及Spring AI等框架搭建高可用系统.同时我对数据库优化，Redis缓存使用以及python脚本开发有丰富的实战经验，能够独立完成从需求分析到系统上线的全流程开发工作。本人目前在新加坡某IT公司担任Java高级开发工程师，主要参与了IT Service management(ITSM)，Fund transform workflow这2个系统的开发，期间完成了动态接口对接功能将项目对接开发周期缩短至3天，还基于Spring AI搭建了LLM智能识别模块，实现了Swift报文的自动解析与校验。个人感觉我始终保持着对技术的热情，在了解到贵公司在招募AI RAG(Retrieval-Augmented Generation)方向的开发人才。我渴望能凭借自身在RAG技术架构设计、知识库构建等方面的积累，深度融入团队，为贵司AI产品的效能提升与生态建设贡献核心力量。


是什么
RAG（Retrieval-Augmented Generation，检索增强生成）是通过调用外部知识库信息辅助大语言模型生成回答的技术，
为什么要用RAG
1 减少幻觉问题
	基于真实检索信息生成内容，大幅降低大模型编造错误信息的概率；
突破知识局限
	实时获取外部知识库的最新信息，弥补大模型知识训练截止期的缺陷；
适配专业场景
	外挂垂直领域知识库，让通用模型具备深度专业知识；
控制成本
	无需重新训练模型，仅通过更新知识库就能实现能力升级，性价比更高。
三、RAG的原理
分为两大阶段：
将多源非结构化数据（PDF、文档等）清洗后拆分为语义完整的文本块，通过嵌入模型转换为向量，存入向量数据库构建可检索知识库；
‌用户提问后，先将问题向量化，在向量数据库中通过相似度搜索召回Top3 - 5(Top K)相关文本块，最后将问题与检索结果一同输入大模型，生成有依据的回答。


RAG常见问题
低召回率，上下文幻觉

上下文幻觉
指大语言模型（LLM）生成的内容与之前对话的上下文相矛盾的现象，常见于多轮对话中。例如，模型在前一轮承认“不知道某信息”，但在后续却自信地给出具体答案，造成逻辑冲突 。这类问题会严重损害用户体验和系统可信度 。

‌RAG召回率

是衡量检索增强生成（RAG）系统在检索阶段性能的核心指标，表示从知识库中成功检索到的相关文档数量占所有相关文档总数的比例。召回率越高，说明系统越能全面覆盖关键信息，减少遗漏，从而为生成阶段提供更完整的上下文支持 。公式为：
召回率 = 检索到的相关文档数 / 知识库中所有相关文档数‌ 。

简单来说，上下文幻觉是生成阶段的“说错话”问题，而RAG召回率是检索阶段的“没找全”问题‌。低召回率可能导致模型因缺乏足够依据而“编造”答案，间接加剧幻觉现象 。


‌BM25全文检索是RAG系统中一种基于关键词匹配的高效检索方法‌，它通过计算查询词与文档之间的相关性得分，对候选文档进行排序，尤其擅长精确召回包含特定术语、实体或专业词汇的文档 。在混合检索架构中，BM25常与向量语义检索结合使用，弥补纯语义模型对“字面匹配”不敏感的短板 。

如何有效提高RAG系统的召回率？ 采用混合检索（BM25 + 向量检索）


LLM Client（语言模型客户端）

作为与大模型交互的统一接口，支持 OpenAI、通义千问、ChatGLM、Azure AI、百度文心等主流模型 。
提供同步与流式调用能力，屏蔽不同厂商API差异，实现无缝切换 。
‌ChatMemory（对话记忆）

管理多轮对话上下文，支持会话状态持久化。
可结合 Redis 或数据库实现跨会话记忆存储，适用于客服、助手类场景 。
‌Tool / Function Calling（工具调用）

允许大模型调用外部Java方法或API，如查询订单、执行计算、调用支付接口等。
实现“AI决策 + 业务执行”的闭环，是构建智能代理的关键模块 。
‌Prompt Template（提示词模板）

支持动态变量注入的可复用提示词管理。
可通过配置文件定义模板，提升提示工程的可维护性与一致性 。
‌Retriever & RAG 组件‌

支持从向量数据库（如Milvus、Pinecone）中检索相关知识片段。
内置 Easy RAG、Native RAG、Advanced RAG 三种模式，适配从原型验证到生产部署的不同阶段 。
‌Agent（智能代理）‌

基于ReAct框架，让模型能自主规划任务、调用工具、迭代推理。
适用于复杂任务编排，如“分析用户投诉 → 查询订单 → 生成补偿方案 → 发送邮件”全流程自动化 。



AI 回答统计聚合问题:

LLM擅长语义理解，不擅长统计计算聚合，strawberry有多少个r

姓张的员工有多少个

1.噪音问题: 知识库有多个文件，姓张的员工，姓张的学生(学生文件)

2.TopK限制问题 ， 大模型只找k个最相似数据





LLM： 大模型, 本质文字接龙

Token：大模型处理数据的最基本单元

Context：大模型每次处理任务时接收到的信息总和，塞入历史聊天使LLM有记忆

Context Window：大模型的Context最多能够存储的Token量

Prompt：用户或系统当前给大模型下达的具体指令或问题

Tool：大模型用来感知和影响外部环境的函数

MCP: 同意了工具接入格式的标准协议(claude code, GPT) model context pretool

Agent: 能自主规划和调用工具，直至解决用户问题的程序 （告诉我天气，如果下雨告诉我最近的雨伞店。先查天气工具--->查雨伞店位置工具）

Agent Skill: 给Agent看的说明文档

RAG: Retrieval-Augmented Generation 检索增强生成



public ChatMemoryController (ChatModel chatModel) {

this. chatClient = ChatClient
.builder (chatModel)
.defaultSystem(text:“你是一个旅游规划师,请根据用户的需求提供旅游规划建议。”)
.defaultAdvisors (new MessageChatMemoryAdvisor (new InMemoryChatMemory () ) )
defaultAdvisors (new MessageChatMemoryAdvisor (new RedisChatMemory (
host: "127.0.0.1",
port: 6379,
password: null

.build () ;



spring事务：





线程池的几种拒绝策略
AbortPolicy（默认策略）：直接抛出RejectedExecutionException异常，阻止系统正常运行。
CallerRunsPolicy：调用执行任务的线程（即提交任务的线程）来执行该任务，通常用于小批量任务的场景。
DiscardPolicy：直接丢弃无法处理的任务，不抛出任何异常。
DiscardOldestPolicy：丢弃线程池中最早的任务，然后重新尝试执行当前任务。
AbortPolicyWithReport：与AbortPolicy类似，但会打印出拒绝任务的详细信息，包括线程池的运行状态。