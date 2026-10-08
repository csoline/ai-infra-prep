# AI Infra 招聘调研与个人转型建议

> 调研日期：2026-10-08（中国标准时间）  
> 适用对象：目前以 Java 后端为主、约 2.5 年经验，有 Agent 开发和部分评测经验，希望转 AI Infra 的工程师。  
> 结论先行：你最适合从 **Agent Runtime / AI 平台后端、推理平台、评测与研究基础设施** 切入；暂时不要把目标定成 CUDA 算子、编译器、RDMA/网络或大规模训练系统的纯底层岗位。你的后端、分布式系统、可靠性和 Agent/eval 经验可以迁移，但需要补齐 Python/PyTorch、Kubernetes、LLM serving、GPU 基础、可观测性和评测工程。

## 1. 口径与限制

AI Infra 不是单一岗位，而是从芯片/内核到模型服务、数据、调度和 Agent 运行时的一组岗位。不同公司对 “AI Infra”“ML Infrastructure”“Inference Platform”“Research Infrastructure” 的命名并不统一。

薪资分为两种口径：

- 中国岗位多数公开 JD 不写薪资，下面同时使用官方/招聘平台 JD、Levels.fyi 自报总包、猎头/招聘报告做三角参考。
- 美国岗位多为官方公开的 **base salary**，通常另有 equity/bonus；不能和中国“月薪×薪数”或 Levels.fyi 总包直接比较。
- 所有数字都不是个人 offer 预测，会随地点、职级、团队、股票、奖金、汇率和招聘时点变化。应以投递时的官方职位页面和书面 offer 为准。

证据优先级：企业官网 JD 用来判断岗位职责和硬技能；招聘平台/Levels.fyi 用来提供职位或总包样本；猎头、媒体和聚合页只用于观察市场宽区间。中国的月薪样本统一理解为税前，通常不含或不完整包含股票、奖金和补贴；美国公开数字通常是年 base，另有 equity/bonus。

## 2. AI Infra 岗位地图

| 岗位族群 | 主要产出 | 常见 JD 名称 | 高频技能 | 与你的距离 |
|---|---|---|---|---|
| 计算/集群/资源调度 | GPU 集群、作业调度、容量、故障自愈、bin-packing、资源隔离 | Compute Infra、Cluster Infra、Fleet、Scheduler、AI Platform | Linux、K8s、容器、调度算法、云/IaC、网络/存储、SRE | 中等；分布式后端可迁移 |
| 训练系统/分布式训练 | 数据加载、并行训练、checkpoint、容错、训练编排 | ML Systems、Training Infra、Pre-training Infra | Python/C++、PyTorch/JAX、Megatron/DeepSpeed、GPU/NCCL、分布式计算 | 偏远；需补 ML/GPU/训练经验 |
| 推理/Serving/性能 | 模型上线、路由、批处理、KV Cache、量化、低延迟高吞吐 | Inference Engineer、Inference Platform、Serving、Model Performance | vLLM/SGLang/TensorRT-LLM/Triton、GPU 显存、并行、profiling、autoscaling | 中等；最值得作为第二阶段主攻 |
| GPU/算子/编译器/运行时 | kernel、通信库、编译器、硬件适配、算子性能 | CUDA Kernel、Compiler、Runtime、ML Accelerator | C++/CUDA、Triton、CUTLASS/CuTe、LLVM/MLIR、NCCL、NUMA | 远；通常需要高性能计算经历 |
| ML/数据/研究平台 | 数据管道、实验编排、模型注册、训练到生产、实验追踪 | ML Platform、Research Infrastructure、Data Infrastructure | Python、Spark/Flink、Kafka、Airflow/Temporal、数据质量、权限、可观测性 | 中等；后端和评测经验有优势 |
| Agent Runtime/平台 | 沙箱、工具调用、状态/上下文、任务调度、长任务恢复、多租户、安全 | Agent Infra、Agent Runtime、Agent Platform、Harness | K8s/容器隔离、工作流、事件驱动、状态机、鉴权、trace、eval | **最近** |
| 评测/安全/质量基础设施 | benchmark、grader、回归、质量/安全指标、线上反馈闭环 | Model Evaluations、Eval Infra、Safety ML Infra | Python、实验设计、统计、数据/标注、评测服务、dashboard | **最近** |
| AI SRE/可靠性/容量 | SLO、容量规划、发布、故障演练、成本和可观测性 | AI Reliability、ML SRE、Capacity Engineering | Prometheus/OpenTelemetry、Linux、K8s、on-call、性能/成本建模 | 中等；传统后端运维能力可迁移 |

### 2.1 招聘方真正关心的能力层次

1. **通用工程底座**：数据结构与算法、并发、网络、存储、数据库、分布式一致性、API 设计、测试、发布、故障处理。
2. **云原生和平台工程**：Docker、Kubernetes、Operator/CRD、Helm、Terraform、工作流引擎、消息队列、权限和多租户。
3. **模型服务能力**：Transformer 基本结构、token/上下文、prefill/decode、batching、KV Cache、量化、流式输出、TTFT/TPOT/p95。
4. **GPU 与系统性能**：显存层次、GPU 利用率、tensor/pipeline/expert parallel、NCCL、profiling；更底层岗位再要求 CUDA/Triton/C++/RDMA。
5. **生产质量**：可观测性、回滚、幂等、容量、成本、数据/模型漂移、eval 回归和安全控制。
6. **协作方式**：能把研究原型变成可运行服务，能写清设计取舍、压测方法和事故复盘，而不是只会调用模型 API。

## 3. 公司与岗位调研

### 3.1 阿里巴巴/阿里云

**公开岗位信号**

- 阿里 HappyHorse/ROCK 的 AI Infra 团队说明覆盖“推理架构设计、全链路服务编排、性能优化”，列出多模态大模型推理系统、训练系统优化、机器学习系统、Agent Infra、搜索/数据库管控等岗位。
- 阿里 2027 校招的 **Agent Infra 工程师** 明确要求支撑 Agent 全生命周期：高并发 Agent 框架、沙箱执行、容器编排、任务调度、状态管理、训练优化、安全部署、检查点恢复和服务化交付。
- AI Infra 校招还分网络、存储等方向：面向千卡/万卡规模的计算基础设施，关注 GPU 间通信、远端 KV Cache、分布式存储和 PyTorch/Megatron/DeepSpeed/Ray/vLLM 的数据访问模式。

**典型要求**

- Agent/平台方向：Java/Go/Python 任一后端基础、分布式系统、K8s/容器、调度与状态管理、权限/沙箱、稳定性。
- 推理方向：C++/Python、CUDA/Triton、并行计算、vLLM/SGLang/TensorRT-LLM、KV Cache、批调度、量化和 profiling。
- 存储/网络方向：分布式存储、I/O 路径、RDMA/高性能网络、GPU 集群通信。

**薪资参考**

阿里及达摩院的 AI 岗位公开薪资不稳定。2025 年招聘报告列出的示例包括“大模型推理系统性能优化专家”约 **40–70k 元/月、13 薪**，机器学习系统/高性能岗位约 **30–60k 元/月**；这是第三方岗位样本，不是阿里统一薪级。北京市人社局 2025 薪酬报告中，AI 大模型架构师月薪中位值约 **46,142 元**，AI 算法工程师中位值超过 **30,000 元/月**，可作为城市市场锚点。

**适合你的切入点**

优先看 Agent Infra、机器学习系统/平台、推理服务控制面、评测与可观测性；先不把高性能算子/编译器作为第一份目标岗位。

来源：[ROCK/HappyHorse AI Infra 招聘](https://alibaba.github.io/ROCK/zh-Hans/careers/)、[阿里 Agent Infra 工程师](https://campus-talent.alibaba.com/campus/position/199907640058?deptCodes=UGWZCC%2C0NZ8T6%2CJOQMU5%2C23DWTR%2CAT1LW3%2C9SQM5Z)、[阿里 AI Infra 网络岗位](https://jobs.ultraai.site/jobs/alibaba_campus/campus%3A199907740014)、[阿里 AI Infra 存储岗位](https://jobs.ultraai.site/jobs/alibaba_campus/campus%3A199907640015)、[北京市人社局薪酬报告摘要](https://www.fxbaogao.com/detail/4785667)。

### 3.2 字节跳动 / Seed

Seed 官方招聘页显示，岗位主要覆盖基础大模型、机器学习系统、视觉/语音智能、大模型 AI 搜索、模型个性化和具身智能；官方介绍强调 AI Infra 基建、算力、数据和工程资源。公开校招信息对具体社招薪资披露较少。

**典型要求**

- ML systems/训练推理：Python/C++、PyTorch/JAX、分布式训练、数据处理、GPU 性能。
- AI 搜索/Agent：检索、排序、在线服务、工具调用、数据闭环和评测。
- 平台后端：高并发、低延迟、服务治理、容器/调度和可观测性。

**薪资参考**

Levels.fyi 中国区软件工程师数据的二手整理显示，字节整体软件工程师公开总包中位数约 **60.8 万元/年**，公开区间约 **47.1 万–227 万+元/年**；这不是 AI Infra 专项，也不是官方薪级，且混合了不同城市、职级和股票。它只能说明公司整体工程岗位的市场上沿。

**适合你的切入点**

字节的 Agent/AI 搜索平台、ML platform backend、推理服务和评测平台比底层 kernel 更匹配；若投 Seed 核心系统岗，需要明显补强 Python/PyTorch、C++/CUDA 和系统性能。

来源：[Seed 大模型人才校招](https://seed.bytedance.com/zh/seedearlycareer)、[Seed 招聘主页](https://seed.bytedance.com/zh/career?view_from=homepage_tab)、[Levels.fyi 数据的公开整理](https://interviewaibox.co/zh/blog/bytedance-software-engineer-salary-2026)。

### 3.3 腾讯 / 混元 / 腾讯云 TIONE

腾讯公开的 **TIONE-AI Infra 后台研发工程师** 岗位要求基于 Kubernetes 建设大模型推理云原生平台，涉及控制面/数据面、多租户、异构算力统一调度、Operator/CSI/CNI/设备插件、模型部署、弹性伸缩和调用全生命周期。这类岗位与 Java 后端的服务治理和平台经验迁移性很强。

腾讯美国公开的 **Sr. AI Inference Systems Engineer** 更偏深层推理优化：KV Cache、Router、异构加速器、调度/内存管理、CUDA/Triton、量化、并行计算和分布式系统；Palo Alto 年 base 为 **$124,800–$235,000**。

**薪资参考**

Levels.fyi（中国区、软件工程师全岗位）截至 2026-10-07 的数据为：T4 约 **27.7 万元总包**，T12 约 **229 万元总包**，中位数约 **46.4 万元/年**。这包含 base、股票和奖金，不等同于 TIONE 或 AI Infra 的报价。

**适合你的切入点**

腾讯云 TIONE、混元推理平台、模型服务治理、Agent/搜索平台和评测基础设施是优先方向；你需要把 K8s、GPU 资源调度和 vLLM 等 serving 技术补到能独立做项目的程度。

来源：[腾讯 TIONE-AI Infra 后台研发](https://careers.tencent.com/jobdesc.html?postId=2099328404741603328)、[腾讯 Sr. AI Inference Systems Engineer](https://tencent.wd1.myworkdayjobs.com/en-US/Tencent_Careers/job/Sr-AI-Inference-Systems-Engineer_R107335)、[腾讯 Levels.fyi 中国薪资](https://www.levels.fyi/companies/tencent/salaries/software-engineer/locations/china)。

### 3.4 DeepSeek

DeepSeek 官方招聘页的岗位拆分非常接近 AI Infra 的完整地图：

- **超算集群研发工程师**：集群、算力和任务基础设施。
- **高性能算子/通信/编译器工程师**：kernel、通信库、编译器和硬件性能。
- **大模型训练/推理框架工程师**：训练/推理框架和运行时。
- **研发基础设施工程师**：研发/实验/工程平台。
- **高性能分布式存储工程师**：训练和推理数据路径。
- **AI 平台运维工程师、IT 基础设施、IDC 数据中心**：运维、容量和数据中心。
- **Agent 弹性计算研发、Agent Harness、服务端开发**：Agent 运行时、沙箱/弹性资源和产品后端。
- 另有预训练数据、后训练、多模态和 Frontier/持续学习研究岗位。

**典型要求**

深度学习系统岗位通常看重 C++/Python、并行与分布式系统、GPU/通信、PyTorch/Megatron/DeepSpeed/vLLM 等；Agent/服务端岗位看重高并发、任务调度、状态、沙箱、安全和工程交付；研发基础设施岗看重 Linux、容器、工作流、数据/实验追踪和可靠性。

**薪资参考**

锐仕方达《2025 生成式人工智能薪酬报告》公开样本中，DeepSeek 核心系统研发约 **60–90k 元/月**，大模型全栈工程师约 **30–60k 元/月**，深度学习研究员约 **50–80k 元/月**。该报告是市场/猎头样本，不能视为 DeepSeek 官方承诺；DeepSeek 官网岗位页本身通常不公开薪资。

**适合你的切入点**

DeepSeek 的服务端开发、Agent 弹性计算、Agent Harness、研发基础设施和 AI 平台运维比超算/算子/编译器更现实。若要冲训练/推理框架，需要先做出 PyTorch/vLLM 级别的可复现实验或开源贡献。

来源：[DeepSeek 官方招聘](https://talent.deepseek.com/)、[2025 生成式 AI 薪酬报告](https://pdf.dfcfw.com/pdf/H3_AP202503031644010561_1.pdf?1741083136000.pdf=)。

### 3.5 百度（相近大厂对照）

百度官方 AI Infra 平台研发岗位把训练、后训练、推理和 Agent 放在同一平台：数据/代码/镜像/资源/产物可追踪，Kubernetes + Ray 异构调度，配额/抢占/容错/弹性/长任务恢复/灰度回滚，MLOps/LLMOps、模型中心、部署和评估，以及 Agent API、MCP/Skill、长期状态、回放和 Memory。岗位要求本科、3 年以上分布式/云原生/AI Infra，Go/Python/C++/Java 至少一门；推理方向还要求 CUDA/CUTLASS/Triton、vLLM/SGLang/TensorRT-LLM、KV Cache、PD 分离、量化和批调度。

这类“平台控制面 + Agent 评测”的 JD 与你的背景很接近，可以作为国内转型的重点样本；纯推理引擎/算子方向则仍需要 C++/CUDA 和 GPU 性能证据。

来源：[百度 AI Infra 平台研发](https://talent.baidu.com/jobs/detail/SOCIAL/2216e7ae-f75d-4e7c-b14d-669a4171a1ff)、[百度 AI Infra 推理工程师](https://talent.baidu.com/jobs/detail/GRADUATE/7753cdb4-70b0-462b-a634-ee53818c7c9a)。

### 3.6 OpenAI

OpenAI 的公开岗位展示了从基础设施到模型服务的完整链路：

- **Software Engineer, GPT Infrastructure**：长任务 API/control plane、调度、重试、checkpoint、资源预算、可观测性、加速器编译/执行/验证、端到端 benchmark；偏好 Python/C++/Go/Rust、Linux、网络、存储、容器、CUDA/ROCm/Triton、vLLM/SGLang。
- **Software Engineer, Compute Infrastructure**：训练系统、GPU/NIC/拓扑/固件/硬件健康、Kubernetes/调度、网络、存储、CaaS、Agent sandbox 和 profiling。
- **AI Infrastructure Engineer, pAGI**：大规模训练/评测基础设施、推理/评分平台、compute 调度、容量管理、GPU idle 和性能可见性。
- **Software Engineer, Agent Infrastructure**：为 Agent 训练和生产运行提供容器编排、API、云/IaC、虚拟化/沙箱和性能优化；公开 base 为 **$230k–$385k + equity**。这类岗位与“Agent 开发 + 后端”直接相连，但仍要求 K8s、Terraform、Python/FastAPI 和容器隔离。
- **Software Engineer, Productivity – Inference Runtime**：负责推理引擎发布门禁、正确性/性能回归、TTFT/TBT 指标、CI/CD、canary、失败分诊和可观测性，JD 明确部分岗位不要求已有 inference 经历，是从评测/发布质量切入推理系统的好样本。
- **Software Engineer, Inference – Multi Modal**：多模态模型的低延迟高吞吐服务、GPU 利用率、tensor parallel、硬件抽象、vLLM/TensorRT-LLM/自研并行系统。
- **Software Engineer, Data Infrastructure**：Spark、Iceberg/Delta、Kafka/Flink、Airflow、分布式存储和 ML feature tooling。

公开年 base 参考：

| 岗位 | 地点 | 官方 base |
|---|---|---:|
| GPT Infrastructure | SF/Seattle | $293k–$385k + equity |
| Compute Infrastructure | 多地 | $230k–$405k + equity |
| AI Infrastructure Engineer, pAGI | San Francisco | $266k–$500k + equity |
| Inference – Multi Modal | San Francisco | $295k–$555k + equity |
| Data Infrastructure | SF/NYC/Seattle/Mountain View | $266k–$445k + equity |
| Monetization ML Infrastructure | San Francisco | $347k–$445k + equity |

这些岗位普遍要求大规模生产系统经验；对 2.5 年经验者，偏 control plane、研究工具、评测平台的岗位比 GPU kernel/训练底座更可能作为跳板。

来源：[GPT Infrastructure](https://openai.com/careers/software-engineer-gpt-infrastructure-san-francisco/)、[Compute Infrastructure](https://openai.com/careers/software-engineer-compute-infrastructure-san-francisco/)、[pAGI AI Infrastructure](https://openai.com/careers/ai-infrastructure-engineer-pagi-san-francisco/)、[Inference – Multi Modal](https://openai.com/careers/software-engineer-inference-multi-modal-san-francisco/)、[Data Infrastructure](https://openai.com/careers/software-engineer-data-infrastructure-san-francisco/)、[Monetization ML Infrastructure](https://openai.com/careers/software-engineer-monetization-ml-infrastructure-san-francisco/)。

### 3.7 Anthropic

Anthropic 的职位页面把基础设施进一步拆成 Inference、Cloud Inference、Inference Velocity、GPU/Performance、ML Networking、Kubernetes/Cluster、Data Infrastructure、Research Tools、Model Evaluations、AI Reliability 和 Agent Runtime Platform 等。

**典型 JD 信号**

- Cloud Inference：多云、多区域路由、容量、负载均衡、batching/caching、Kubernetes/IaC、Python/Rust。
- Performance Engineer, Inference Systems：从 kernel、网络、框架到 autoscaler 建立 FLOPs/吞吐/延迟模型，定位数值正确性和性能差距。
- ML Networking：TCP/IP、XDP/eBPF/io_uring、DPDK/RDMA、collectives/RPC、NUMA、Rust/C/C++。
- Research Tools / ML Infrastructure：为研究人员提供实验编排、反馈收集、sandbox、数据和可视化；要求生产级 Python/服务开发，ML 知识部分岗位可入职后补。
- Model Evaluations：设计并运行大模型评测，建设 dashboard/alert/report，关注统计、实验设计、指标可靠性和生产回归。

公开年薪参考：

| 岗位 | 官方年薪 |
|---|---:|
| Staff+ / Senior Cloud Inference | $320k–$485k |
| Engineering Manager, Inference Infrastructure | $405k–$625k |
| Performance Engineer, Inference Systems | $350k–$850k |
| Staff+ ML Inference Path | $320k–$485k |
| Research Engineer, Post-Training Model Evaluations | $500k–$850k |
| Staff+ SRE, Safeguards ML Infra | $320k–$485k |

薪资区间很宽，原因是混合了 senior/staff/manager 和不同地点；它们是 annual salary，另有 equity/福利。

来源：[Anthropic 公开岗位列表](https://www.anthropic.com/careers/jobs)、[Cloud Inference](https://job-boards.greenhouse.io/anthropic/jobs/5436684008)、[Inference Infrastructure Manager](https://job-boards.greenhouse.io/anthropic/jobs/5411560008)、[Performance Engineer, Inference Systems](https://job-boards.greenhouse.io/anthropic/jobs/5224564008)、[Model Evaluations](https://job-boards.greenhouse.io/anthropic/jobs/5198255008)、[Safeguards ML Infra SRE](https://job-boards.greenhouse.io/anthropic/jobs/5416709008)。

### 3.8 对照样本：Together AI / xAI 等

美国 AI Infra 创业公司的 JD 对技能边界写得更直接：

- Together AI GPU/Infra：Python/Go/Rust、Linux/K8s/Terraform、GPU fleet、NCCL/NVLink/NVSwitch、InfiniBand/RoCE、裸机和自动化；公开 base 约 **$160k–$280k**，按岗位/级别变化。
- Together AI ML Engineer–Inference：PyTorch、OS/内存/网络/存储性能，vLLM/TGI/TensorRT-LLM、speculative decoding、CUDA/Triton；base 约 **$160k–$230k**。
- xAI Platform Infrastructure：Rust/C++、GPU/Linux kernel/网络/文件系统 profiling、K8s CNI/CSI/service mesh、Prometheus/Grafana/OpenTelemetry；base 约 **$180k–$440k**。
- Elorian Inference Infrastructure：3+ 年大模型 serving、量化、batching、speculative decoding、KV Cache、vLLM/TensorRT-LLM/Triton/SGLang、多 GPU/多节点、autoscaling；base **$200k–$400k**。

来源：[Together AI GPU/Infra](https://job-boards.greenhouse.io/togetherai/jobs/5138540007)、[Together AI Inference](https://job-boards.greenhouse.io/togetherai/jobs/4385540007?gh_src=4635cfa37us)、[xAI Platform Infrastructure](https://job-boards.greenhouse.io/xai/jobs/5191142007)、[Elorian Inference Infrastructure](https://jobs.ashbyhq.com/elorian-ai-inc/7836158c-eeb6-4d55-95ca-510b1007b41d)。

## 4. 薪资横向解读

### 中国大陆

按公开岗位样本归纳的岗位层级宽区间（税前月薪，通常为现金口径；年包还会受 13–16 薪、奖金、股票和城市影响）：

| 岗位族群 | 常见公开样本区间 | 进入门槛 |
|---|---:|---|
| AI 平台/控制面 | 30–60k × 14–16 | Java/Go/Python、K8s、API/调度/可观测；最适合你的第一跳 |
| Agent Runtime / 评测 / LLMOps | 30–70k × 14–16 | 工作流、沙箱、工具/MCP、回放/基准、CI；你的 Agent/eval 可形成差异 |
| 推理服务工程 | 35–70k × 14–16 | vLLM/SGLang、路由、batch、KV Cache、量化；需补 GPU |
| 训练/调度系统 | 40–90k × 14–16 | Ray/Megatron/DeepSpeed、3D 并行、checkpoint；常见 3+ 年/硕士 |
| Kernel/编译器/硬件协同 | 50–100k+ × 13–16 | C++/CUDA/Triton/RDMA；门槛最高 |

以上是市场宽区间，不能替代具体公司的书面报价。

| 参考来源 | 公开数字 | 口径 |
|---|---:|---|
| 腾讯 Levels.fyi 中国软件工程师 | T4 约 27.7 万；T12 约 229 万；中位约 46.4 万/年 | 自报总包，base+股票+奖金；不是 AI Infra 专项 |
| 字节 Levels.fyi 二手公开整理 | 中位约 60.8 万；区间约 47.1 万–227 万+ | 软件工程师全岗位；混合城市/职级 |
| DeepSeek 猎头报告 | 核心系统约 60–90k/月；全栈约 30–60k/月 | 第三方样本，偏热门/高端岗位 |
| 阿里/达摩院报告样例 | 推理系统性能优化约 40–70k/月、13 薪 | 第三方岗位样本 |
| 北京人社局 AI 岗位 | 大模型架构师中位约 46,142 元/月；算法工程师中位超过 30,000 元/月 | 城市市场统计，不是公司报价 |

对你而言，2.5 年经验更应该关注岗位级别和实际职责，而不是直接拿到“核心系统/研究员”的高端样本。若能把后端能力转成平台/推理/Agent 基础设施成果，国内一线城市的中级 AI 平台岗位通常比纯应用开发有更高上限；具体仍要以团队、业务和职级为准。

### 美国

OpenAI、Anthropic 的官方 base 通常在 **$230k–$555k** 的公开区间，Anthropic 的某些 inference/performance/eval 岗位因级别跨度达到 **$320k–$850k**。Levels.fyi 还显示湾区高等级 AI 实验室的总包会因 equity 显著高于 base：

- OpenAI 软件工程师湾区公开总包约 **$246k–$1.83M**，中位约 **$875k**。
- Anthropic 软件工程师湾区公开总包中位约 **$888k**；Senior/Lead/Staff 的公开样本约 **$628k/$779k/$1.13M**。

这些是自报、验证后的总包样本，且偏向高等级和湾区，不能拿来预测中国 offer，也不能把总包当年薪 base。

来源：[OpenAI Levels.fyi](https://www.levels.fyi/companies/openai/salaries/software-engineer/locations/san-francisco-bay-area)、[Anthropic Levels.fyi](https://www.levels.fyi/companies/anthropic/salaries/software-engineer/locations/san-francisco-bay-area)。

## 5. 你的匹配度判断

### 5.1 现有优势

- Java 后端 2.5 年：服务设计、并发、数据库、缓存、RPC、稳定性和工程协作可以迁移到平台控制面。
- Agent 开发：理解工具调用、上下文、工作流和产品落地，适合 Agent Runtime、Harness、工具执行和观测。
- 评测经验：如果能把评测变成可复用的任务编排、数据/trace schema、grader、回归和 dashboard，能对接 ML Infra/Eval Infra。
- 后端工程视角：很多研究型团队缺少生产系统工程师，你可以把“原型能跑”推进到“可部署、可回滚、可观测、可压测”。

### 5.2 主要缺口

按多数公开 JD 出现频率排序：

1. Python 与 PyTorch：至少能读模型/推理代码、写数据和评测 worker。
2. Kubernetes：Deployment/Job、GPU device plugin、调度、Operator/CRD、Helm。
3. LLM serving：vLLM 或 SGLang，理解 streaming、continuous batching、KV Cache、TTFT/TPOT/p95。
4. Linux、网络与可观测性：进程/内存/IO、Prometheus、OpenTelemetry、日志/trace、故障注入。
5. 工作流和事件系统：Temporal/Cadence、Kafka/NATS、重试/幂等/补偿/长任务 checkpoint。
6. GPU 基础：显存、CUDA 执行模型、tensor/pipeline parallel、NCCL、量化和 profiling。
7. 语言扩展：Go 是平台岗位的高性价比补充；C++/Rust/CUDA/Triton 是性能路线的后续投入。
8. 统计与实验设计：评测指标、置信区间、数据切分、grader 校准、漂移和回归。

### 5.3 岗位优先级

| 优先级 | 目标岗位 | 适配度 | 建议 |
|---|---|---:|---|
| A | Agent Runtime / Agent Infra 后端 | 高 | 近期直接投；重点补 K8s、沙箱、任务编排、状态/恢复、安全 |
| A | AI Platform / ML Platform Backend | 高 | 以 Java/Go 控制面 + Python worker 为切入 |
| A | Eval Infra / Research Infrastructure | 高 | 把现有评测经验产品化，补 Python、数据管道、实验追踪 |
| A- | Inference Platform Engineer（平台/服务治理侧） | 中高 | 先做 vLLM/K8s/弹性伸缩和压测 |
| B | Compute/Fleet/Scheduler | 中 | 先有集群/调度项目，再冲更大规模 |
| B- | Inference Performance Engineer | 中低到中 | 6–18 个月补 GPU、profiling、量化、并行 |
| C | CUDA Kernel / Compiler / ML Networking | 低（短期） | 需要 C++/CUDA/RDMA/HPC 深度，暂不作为第一跳 |
| C | Pre-training/Distributed Training 核心研发 | 低（短期） | 通常要求 PyTorch/JAX、Megatron/DeepSpeed、GPU 集群和研究/论文证据 |

### 5.4 推荐关注的公司/团队

- 国内第一梯队：阿里 Agent Infra/机器学习系统/推理平台，腾讯 TIONE/混元推理平台，DeepSeek 服务端/Agent 弹性计算/Agent Harness/研发基础设施，字节 Seed 的 ML systems/AI 搜索/Agent 平台。
- 海外可迁移岗位：Anthropic Research Tools、ML Infrastructure/Safeguards、Cloud Inference Launch；OpenAI Agent Infrastructure、Productivity–Inference Runtime、Compute/pAGI/Research Infrastructure 中偏平台和调度的岗位；Together AI Inference/Compute control plane。
- 暂缓作为第一目标：OpenAI/Anthropic 的 GPU kernel、ML Networking、超大规模训练系统，以及需要多年 HPC/CUDA/RDMA 的岗位。

## 6. 学习与求职路线

### 0–4 周：补齐共同语言

- Python：异步、类型、pytest、FastAPI、数据处理；能读 PyTorch 模型和 vLLM 代码。
- LLM serving：跑通一个开源模型，记录 prefill/decode、streaming、batching、KV Cache、TTFT/TPOT/p95。
- Linux/K8s：Docker、GPU 容器、Deployment/Job/Service、资源限制、日志和 Prometheus。
- 把 Java 后端项目经历改写为：吞吐、延迟、SLO、故障率、成本、容量和恢复时间，而不只写业务功能。

### 1–3 个月：做一个可展示的桥接项目

建议项目：**Agent Runtime + Eval Platform**。

架构可以是：

- Java 或 Go：API、租户、鉴权、任务控制面、状态机。
- Python：Agent worker、模型调用、评测 worker、grader。
- Temporal 或 Kafka：长任务、重试、幂等、补偿、checkpoint。
- Kubernetes：部署 worker 和 vLLM；用 HPA/KEDA 或自定义控制器做弹性。
- Prometheus + OpenTelemetry：记录 token、TTFT、TPOT、p95、GPU 利用率、失败原因、单位任务成本。
- 评测：离线任务集、工具调用成功率、轨迹质量、回归阈值、grader 校准和 dashboard。
- 安全：工具白名单、租户隔离、超时、资源预算、沙箱边界。

项目验收标准：

- 能在本地或云 GPU 上一键启动，README 有架构图和故障恢复说明。
- 有压测表：并发、吞吐、TTFT/TPOT/p95、GPU 利用率、成本。
- 注入模型超时、worker 崩溃、重复消息和依赖不可用，证明重试/幂等/恢复有效。
- 至少有一组评测回归：质量下降时 CI 或 dashboard 能阻断/报警。
- 说明一个明确优化：例如 continuous batching 使吞吐提高、KV Cache/缓存使成本下降、或 checkpoint 使长任务恢复时间下降。

### 3–6 个月：补到能面推理平台

- 深入 vLLM/SGLang/Triton Inference Server，理解 scheduler、continuous batching、KV Cache、prefix cache。
- 学 GPU 基础、NCCL、显存和 profiling；用 Nsight 或 PyTorch profiler 做一次端到端瓶颈分析。
- 学量化、speculative decoding、tensor/pipeline parallel；能解释质量、吞吐、显存和成本的取舍。
- 做一次开源贡献：文档、benchmark、bugfix、插件或可复现实验均可，重点是能说明问题、实验和结果。
- 补 Go；Java 继续保留作为成熟后端栈，不需要为了转型完全丢掉。

### 6–12 个月：分叉选择

- **平台路线**：Agent Runtime、AI Platform、Eval/Research Infra、SRE/Capacity，继续加强 K8s、工作流、云、可观测性和安全。
- **推理路线**：Inference Platform/Performance，继续加强 C++/CUDA/Triton、模型并行、量化、通信和性能工程。
- **训练路线**：PyTorch/JAX、Megatron/DeepSpeed、数据加载、checkpoint、容错和大规模实验；这条路线投入最大，适合在已有平台基础上再转。

## 7. 简历和面试怎么呈现

简历项目不要只写“开发 Agent、接入大模型、完成评测”。应改成可验证的基础设施成果，例如：

- “设计任务控制面和 worker 状态机，支持 X 个并发任务；通过幂等键、指数退避和 checkpoint 将重复执行率/恢复时间降低到……”
- “构建评测流水线，覆盖数据版本、grader、回归阈值和 trace；每次模型变更自动产出质量、延迟和成本报告。”
- “将模型服务部署到 K8s，增加弹性伸缩和 Prometheus 指标；在 N 并发下 TTFT/TPOT/p95 为……，GPU 利用率从……提升到……”
- “对工具调用、超时、重试、权限和 prompt injection 做故障注入，形成可复现的安全/可靠性测试。”

面试准备分两层：

1. 先把 Java 后端强项打透：分布式系统设计、并发、缓存、消息、数据库、限流、降级、SLO、事故复盘。
2. 再补 AI Infra 题：如何部署 vLLM、如何做动态批处理、KV Cache 如何影响显存和吞吐、GPU 任务如何调度、长任务如何恢复、评测指标如何防止 reward hacking/数据泄漏、如何定位 p99 尾延迟。

## 8. 最终建议

以你的背景，最合理的求职顺序是：

1. **Agent Runtime / AI Platform Backend**
2. **Eval Infra / Research Infrastructure**
3. **Inference Platform（控制面、服务治理、弹性、可观测性）**
4. **Inference Performance / Compute Scheduler**
5. **CUDA/编译器/ML Networking/核心训练系统**

先用 3 个月做出一个有压测、故障注入、评测回归和 K8s 部署的桥接项目，再针对 A 类岗位投递；等有 serving 和 GPU 性能证据后，再把目标上移到推理性能和调度。这样能最大化你现有 Java/Agent/eval 经验的复用率，同时避免把时间全部投入到短期内难以形成面试证据的底层方向。

## 9. 参考链接总表

- [Alibaba ROCK/HappyHorse AI Infra](https://alibaba.github.io/ROCK/zh-Hans/careers/)
- [Alibaba Agent Infra 工程师](https://campus-talent.alibaba.com/campus/position/199907640058?deptCodes=UGWZCC%2C0NZ8T6%2CJOQMU5%2C23DWTR%2CAT1LW3%2C9SQM5Z)
- [ByteDance Seed 招聘](https://seed.bytedance.com/zh/career?view_from=homepage_tab)
- [Tencent TIONE-AI Infra](https://careers.tencent.com/jobdesc.html?postId=2099328404741603328)
- [Tencent AI Inference Systems Engineer](https://tencent.wd1.myworkdayjobs.com/en-US/Tencent_Careers/job/Sr-AI-Inference-Systems-Engineer_R107335)
- [DeepSeek 官方招聘](https://talent.deepseek.com/)
- [OpenAI GPT Infrastructure](https://openai.com/careers/software-engineer-gpt-infrastructure-san-francisco/)
- [OpenAI Compute Infrastructure](https://openai.com/careers/software-engineer-compute-infrastructure-san-francisco/)
- [OpenAI pAGI AI Infrastructure](https://openai.com/careers/ai-infrastructure-engineer-pagi-san-francisco/)
- [OpenAI Agent Infrastructure](https://openai.com/careers/software-engineer-agent-infrastructure-san-francisco/)
- [OpenAI Productivity – Inference Runtime](https://openai.com/careers/software-engineer-productivity-inference-runtime-san-francisco/)
- [OpenAI Inference – Multi Modal](https://openai.com/careers/software-engineer-inference-multi-modal-san-francisco/)
- [OpenAI Data Infrastructure](https://openai.com/careers/software-engineer-data-infrastructure-san-francisco/)
- [Anthropic 岗位列表](https://www.anthropic.com/careers/jobs)
- [Anthropic Cloud Inference](https://job-boards.greenhouse.io/anthropic/jobs/5436684008)
- [Anthropic Performance Engineer, Inference Systems](https://job-boards.greenhouse.io/anthropic/jobs/5224564008)
- [Anthropic Model Evaluations](https://job-boards.greenhouse.io/anthropic/jobs/5198255008)
- [Tencent Levels.fyi 中国薪资](https://www.levels.fyi/companies/tencent/salaries/software-engineer/locations/china)
- [OpenAI Levels.fyi 湾区总包](https://www.levels.fyi/companies/openai/salaries/software-engineer/locations/san-francisco-bay-area)
- [Anthropic Levels.fyi 湾区总包](https://www.levels.fyi/companies/anthropic/salaries/software-engineer/locations/san-francisco-bay-area)
- [2025 生成式 AI 薪酬报告](https://pdf.dfcfw.com/pdf/H3_AP202503031644010561_1.pdf?1741083136000.pdf=)
- [北京市人社局薪酬数据摘要](https://www.fxbaogao.com/detail/4785667)






