# CCL 2026 非字面义翻译任务说明文档

## 1. 组织者及联系方式

- **评测组织者**：张冬瑜（大连理工大学教授）
- **任务负责人、联系人**：杨森淇（ysq1997@mail.dlut.edu.cn）

---

## 2. 任务背景

在跨文化传播与多语言智能应用快速发展的背景下，谚语、成语、习语、俚语与典故等非字面表达的跨语言对齐成为自然语言理解与翻译系统的关键挑战。这类表达往往依赖隐喻意象与语用推理，呈现“形式差异大、语义可等值替代”的特征：同一核心命题常需通过目标语言中的既有习语、谚语或更自然的释义来实现文化映射与语气一致；若仅做逐词直译，容易造成语义失真与语用偏移。

鉴于通用评测对非字面表达覆盖不足，本任务构建生成与判别互补的评测框架：任务一要求模型生成地道英文译文并优先采用英语现成等值表达，任务二要求在候选中识别公认的金标签，从而分别检验模型的非字面义表达生成能力与标准非字面义辨识能力，为跨文化非字面语义对齐与翻译质量诊断提供可量化的模型。

---

## 3. 任务内容

### 任务一：非字面义中文翻译为英文

给定一条包含谚语、成语、习语等非字面表达的中文句子，模型需生成 1 条自然地道的英文译文，优先采用英语中现成的习语、谚语、格言或固定搭配实现“文化映射/等值替代”，以复现原文的核心命题与语用效果（如劝诫、讽刺、警示、褒贬与语气强度），避免逐词直译造成语义失真；当缺乏公认等值表达时，输出应转为简洁清晰的释义性翻译，确保信息、情感色彩与交际意图一致且可用。

**任务示例：**

- **输入**：对牛弹琴。
- **输出**：cast pearls before swine.

### 任务二：非字面义中英选择

给定若干组（2–6 个）中英文候选对或英文候选译法，模型需进行不定项选择，识别并输出与中文非字面表达在英语语境中构成公认“等值替代/文化映射”的金标签项（可多选），以检验模型对标准非字面义对应关系的判别能力与对语用效果一致性的把握。

**任务示例：**

**输入**：对牛弹琴 对应的英文应该为：

- A. cast pearls before swine.
- B. (It’s) like talking to a brick wall.
- C. Your advice/warnings fell on deaf ears (no one listened).
- D. It’s pointless to explain something to someone who can’t/won’t understand.

**输出**：A

---

## 4. 评测数据

本次评测所使用的非字面意义数据集来源于 33 本中英对照书籍与 20 余个公开网站的中英对译资源（涵盖词典、文化与格言类网站等，并包含新东方等具有代表性的来源）。数据共 5,000 条高质量样本，重点覆盖谚语、成语、习语、俚语、名言、典故引喻等语义隐含强、文化依赖高的非字面表达类型。

- **Gold（谚语/格言体等价）**：1–3 条
- **Silver（解释性等价改写）**：1–3 条
- **总参考数**：2–6 条

### 样例

| 中文 | Gold | Silver |
|---|---|---|
| 家丑不可外扬。 | 1) Don’t wash your dirty linen in public.  2) Don’t tell tales out of school. | 1) Keep family scandals/private troubles within the family; don’t make them public.  2) Don’t reveal embarrassing private matters to outsiders. |
| 对牛弹琴 | 1) cast pearls before swine | 1) Your advice/warnings fell on deaf ears (no one listened).  2) It’s pointless to explain something to someone who can’t/won’t understand.  3) (It’s) like talking to a brick wall. |
| 入乡随俗 | 1) When in Rome, do as the Romans do. | 1) Follow the local customs when you’re in a new place.  2) Adapt to local practices/ways of doing things. |
| 画蛇添足 | 1) gild the lily  2) over-egg the pudding | 1) Don’t add unnecessary embellishments to something already good.  2) Don’t overdo it—trying to improve it too much may spoil it. |

---

## 5. 评价标准

### 任务一评价标准

采用基于参考集合相似度的自动评价方式。

对于每个测试样本，设模型输出为 $y$，对应的 Gold 参考集合为 $G=\{g_1,\dots,g_m\}$，Silver 参考集合为 $S=\{s_1,\dots,s_n\}$。其中，Gold 表示英语中现成的习语、谚语体等值表达，Silver 表示解释性等价改写。

首先分别计算模型输出与 Gold 集合、Silver 集合中各参考答案的语义相似度，并取最大值作为该输出对两类参考的匹配分数：

$$
\mathrm{Score}_{\mathrm{Gold}}(y)=\max_{g_i\in G}\mathrm{Sim}(y,g_i)
$$

$$
\mathrm{Score}_{\mathrm{Silver}}(y)=\max_{s_j\in S}\mathrm{Sim}(y,s_j)
$$

其中，$\mathrm{Sim}(\cdot,\cdot)$ 表示模型输出与参考译文之间的相似度函数。

#### 评价指标：Gold-style Hit@1

若模型输出与 Gold 集合的最大相似度高于与 Silver 集合的最大相似度，则记为一次 Gold-style 命中。单样本得分定义为：

$$
\mathrm{GoldHit}_i=
\begin{cases}
1, & \text{if } \mathrm{Score}_{\mathrm{Gold}}(y_i)>\mathrm{Score}_{\mathrm{Silver}}(y_i) \\\\
0, & \text{otherwise}
\end{cases}
$$

总体指标定义为：

Gold-style Hit@1 = (1/N) * \sum_{i=1}^{N} GoldHit_i
### 任务二评价标准

评价采用 Accuracy。单样本得分定义为：

$$
\mathrm{Acc}_i=
\begin{cases}
1, & \text{if } \hat{Y}_i = Y_i^{\mathrm{Gold}} \\\\
0, & \text{otherwise}
\end{cases}
$$

其中，$\hat{Y}_i$ 表示模型选择的选项集合，$Y_i^{\mathrm{Gold}}$ 表示该样本的 Gold 集合。

总体 Accuracy 定义为：

$$
\mathrm{Accuracy}
=
\frac{1}{N}\sum_{i=1}^{N}\mathrm{Acc}_i
$$

只有当模型选择的选项集合与 Gold 集合完全一致时，该样本计 1 分，否则计 0 分（多选、漏选都会判错）。

---

## 6. 评测赛程

1. **评测任务发布**：2026/02/01  
2. **报名时间**：2026/02/01–2026/06/10  
3. **训练集发布**：2026/04/01  
4. **测试集 A 榜发布时间（非最终测试集，不计分）**：2026/04/11  
5. **测试集 A 榜提交截至时间**：2026/06/11  
6. **测试集 B 榜发布时间（即最终排名成绩）**：2026/06/21  
7. **测试集 B 榜提交截至时间**：2026/06/30  
8. **获奖队伍提交材料（核实无误即可获得颁奖资格，不合格者延顺）**：2026/07/01–2026/07/09  
9. **提交中文或英文技术报告**：2026/07/10  
10. **公布颁奖队伍**：2026/07/20  
11. **评测论文审稿 & 录用通知**：2026/07/25  
12. **评测论文 Camera-ready 版提交**：2026/08/15  
13. **评测论文纠错排版 & 提交 ACL/CCL Anthology 收录**：2026/09/15  
14. **CCL 2026 技术评测研讨会**：2026/10  

---

## 7. 资助情况

- 本届评测将设置一、二、三等奖，由中国中文信息学会提供荣誉证书。

---

## 8. 任务网站

- https://github.com/DUTIR-YSQ/CCL2026-Non-literal-Translation-Task

---

## 9. 参赛限制与说明

为保证评测的公平性、可比性与可复现性，参赛系统须遵守以下规定：

- 禁止使用任何外部 API 或在线服务。
- 禁止在推理阶段调用在线翻译、搜索、问答、字典或知识库接口。
- 测试集结果必须在离线环境下生成，禁止联网推理或通过互联网获取任何额外辅助信息。
- 参赛系统所使用的单个基础模型参数规模不得超过 10B，并以模型官方公开说明为准。
- 禁止使用闭源大模型、商业模型服务或其他不可公开复现的专有模型。
- 默认仅允许单模型系统参赛，禁止使用模型投票、级联、多模型融合或 rerank 等集成方法。
- 同时，禁止在推理阶段使用外部词典、双语习语库、谚语库、知识图谱、搜索引擎或 RAG 检索模块。

---

## 10. 交流 QQ 群

- QQ 群：1091385824
