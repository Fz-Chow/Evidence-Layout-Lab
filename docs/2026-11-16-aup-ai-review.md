# Evidence Layout Lab 八题 AI 复核记录

由 Codex 于 2026-11-16 对照原文复核题干、标准答案和必要证据。原题集保留。运行副本只包含原预留的八题，通过已有探索入口运行，人工复核字段仍为待审。两条材料分别用于确定中间实体或其属性，部分题可凭唯一数值、专名或模型先验直接猜到答案。编号覆盖率衡量引用了哪些材料，不能证明模型实际执行了两步推理。

## 逐题复核

- `bright-model-base`。答案 **Mistral-7B-v0.1**，必要证据 E3 → E6。BRIGHT 摘要把 59.0 nDCG@10 对应的模型标为 SFR-Embedding-Mistral。Salesforce 模型卡列出 E5-mistral-7b-instruct 与 Mistral-7B-v0.1。 [第一步原文](https://arxiv.org/html/2407.12883v4) · [第二步原文](https://huggingface.co/Salesforce/SFR-Embedding-Mistral)
- `alce-open-ended-origin`。答案 **Reddit**，必要证据 E3 → E5。ALCE 表 1 与第 3 节识别出 ELI5。ELI5 原论文摘要确认问答讨论串来自 Reddit。ALCE 原文自身也交代来源，复核时分别核对两条材料。 [第一步原文](https://aclanthology.org/2023.emnlp-main.398.pdf) · [第二步原文](https://aclanthology.org/P19-1346/)
- `agentbench-ecommerce-n`。答案 **12087**，必要证据 E1 → E4。AgentBench 第 3 节与附录 H 采用 WebShop。WebShop 摘要报告 12,087 条众包文本指令。 [第一步原文](https://arxiv.org/html/2308.03688v3) · [第二步原文](https://proceedings.neurips.cc/paper_files/paper/2022/hash/82ad13ec01f9fe44c01cb91814fd7b8c-Abstract-Conference.html)
- `agentbench-house-render`。答案 **AI2-THOR**，必要证据 E1 → E3。AgentBench 第 3 节确认为 ALFWorld。ALFWorld 第 2 节脚注 1 将渲染和物理归于 THOR。 [第一步原文](https://arxiv.org/html/2308.03688v3) · [第二步原文](https://openreview.net/references/pdf?id=G23_TnrVBI)
- `alce-ambiguous-source`。答案 **AmbigQA**，必要证据 E2 → E5。ALCE 表 1 与第 3 节识别出 ASQA。ASQA 图 1 和 §3.3 确认问题采自 AmbigQA。ALCE 原文自身也交代来源，复核时分别核对两条材料。 [第一步原文](https://aclanthology.org/2023.emnlp-main.398.pdf) · [第二步原文](https://aclanthology.org/2022.emnlp-main.566.pdf)
- `divalign-embed-dim`。答案 **768**，必要证据 E1 → E5。DivAlign 实现部分指定 all-mpnet-base-v2。模型卡给出 768 维。 [第一步原文](https://arxiv.org/html/2607.28087v1) · [第二步原文](https://huggingface.co/sentence-transformers/all-mpnet-base-v2)
- `crag-kdd-track-count`。答案 **3**，必要证据 E3 → E6。CRAG 摘要将基准与 KDD Cup 2024 挑战关联。官方规则的报名条款明确可选择 1、2 或 3 项，取上限 3。 [第一步原文](https://arxiv.org/abs/2406.04744) · [第二步原文](https://www.aicrowd.com/challenges/meta-comprehensive-rag-benchmark-kdd-cup-2024/challenge_rules)
- `selfrag-context-base`。答案 **4096**，必要证据 E3 → E5。Self-RAG 训练设置指出生成器基座为 Llama 2 7B 和 13B。Llama 2 技术报告表 1 与附录 A.2.1 给出 4096 token。 [第一步原文](https://arxiv.org/html/2310.11511v1) · [第二步原文](https://arxiv.org/html/2307.09288)

四道数字题将数字约束明确限定到 answer 字段，同时保留 JSON 引用编号输出。答案和必要编号保持原标注，修正部分来源定位。题目在模型生成之前冻结。

## 云端 GPU 运行与结果

- 模型 `Qwen/Qwen2.5-3B-Instruct`，权重 revision `aa8e72537993ba99e69dfaafa59ed015b17504d1`，代码提交 `cfcd651a24db320d5314a4c25a2de122328d9df4`。
- 输入题集 SHA-256 `f2d0e24e2745ddf18e1b3e44c9b689e2c77bb88f9f4f54fca9855a59eaea0788`。固定随机种子 42，最多输入 4096 token、生成 128 token。沿用同题、同材料、等长的集中与分散配对，另有仅必要证据组。
- AMD GPU 验收通过。8 道独立题，每题 6 组集中／分散配对与 2 次仅必要证据生成，共 48 组配对、112 条输出。

| 条件 | 生成 | 运行失败 | JSON 格式无效 | 答案正确率 | 必要证据编号覆盖率 |
|---|---:|---:|---:|---:|---:|
| 集中 | 48 | 0 | 0 | 62.50% | 47.92% |
| 分散 | 48 | 0 | 0 | 60.42% | 48.96% |
| 仅必要证据 | 16 | 0 | 2 | 68.75% | 62.50% |

## 逐题结果

| 题目 | 答案正确率 集中／分散 | 必要证据编号覆盖率 集中／分散 |
|---|---|---|
| agentbench-ecommerce-n | 33.33%／50.00% | 58.33%／50.00% |
| agentbench-house-render | 16.67%／16.67% | 50.00%／50.00% |
| alce-ambiguous-source | 33.33%／33.33% | 50.00%／50.00% |
| alce-open-ended-origin | 100.00%／83.33% | 41.67%／33.33% |
| bright-model-base | 16.67%／0.00% | 33.33%／58.33% |
| crag-kdd-track-count | 100.00%／100.00% | 50.00%／50.00% |
| divalign-embed-dim | 100.00%／100.00% | 50.00%／50.00% |
| selfrag-context-base | 100.00%／100.00% | 50.00%／50.00% |

- 题级配对差（集中减分散）中，答案正确率平均差为 +2.08 个百分点（逐题范围 -16.67 至 +16.67）。必要证据编号覆盖率平均差为 -1.04 个百分点（逐题范围 -25.00 至 +8.33）。

## 答对但缺少必要编号的样例

- 记录 `agentbench-ecommerce-n--concentrated--r0--v0`。正确答案为 12087，输出列出 E4，缺少 E1。

```text
{"answer":"12087","citations":["E4"]}
```

## 解释范围

- 每题的排列是重复观测。逐题配对差以八道独立题为单位。仅必要证据组的输入更短，作为能力检查单列。
- 这批结果是 AI 原文复核后的补充实验。编号覆盖率按程序计算，生成输出的引用语义支持未独立评审。结合题量和题型，结果仅用于本次证据布局对照。
