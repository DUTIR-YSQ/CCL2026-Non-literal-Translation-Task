## Dataset A

Dataset A 是本次 **非字面义翻译和理解评测** 公开发布的缩小版训练数据，供参赛选手先行微调模型并调整实验策略。

本次 Dataset A 对应两个子任务：

- **子任务1：非字面义中英选择**  
  给定中文表达和若干英文候选，选择最合适的英文译法。

- **子任务2：非字面义中文翻译为英文**  
  给定中文非字面表达，生成自然、地道的英文译文；若存在英语中的现成习语、谚语或固定表达，优先采用等值替代；否则输出清晰自然的释义性翻译。

每条样本包含两类参考答案：

- **Gold**：英语中现成的习语、谚语、格言体等值表达  
- **Silver**：对原始非字面含义的自然英文释义改写  

---

## Baseline 实验说明

我们在 Dataset A 上进行了初步实验，并提供 baseline 结果供参赛选手参考。选手可根据自身模型情况，自行调整提示词、参数设置和训练策略。

## 本次测试所使用的 Prompt 模板

### 子任务1
```text
Choose the BEST English translation for the Chinese expression below.

Chinese: {chinese_text}

Options:  

### 子任务2
You are an expert translator specializing in Chinese idioms, proverbs, and cultural expressions.

Translate the following Chinese expression into natural, idiomatic English.
If there is an equivalent English proverb or idiom, use it. Otherwise, provide a clear paraphrase.

Chinese: {chinese_text}
English translation:

## 初步测试结果（Dataset A）

我们在 **Qwen2-7B（推荐）** 和 **Llama2-7B** 上进行了初步实验，当前已整理出的结果如下。

### Qwen2-7B

#### 子任务1
| 模型结果 | 条数 | 命中 | 命中率 | avg_gold_sim | avg_silver_sim | avg_margin |
|---|---:|---:|---:|---:|---:|---:|
| Qwen2-7B zero1 | 124 | 37 | 29.84% | 0.3809 | 0.4777 | -0.0968 |
| Qwen2-7B 微调1 | 124 | 57 | 45.97% | 0.3282 | 0.3070 | 0.0211 |

#### 子任务2
| 模型结果 | 条数 | 正确 | 准确率 |
|---|---:|---:|---:|
| Qwen2-7B zero2 | 124 | 33 | 26.61% |
| Qwen2-7B 微调2 | 124 | 45 | 36.29% |

### Llama2-7B

#### 子任务1
| 模型结果 | 条数 | 命中 | 命中率 | avg_gold_sim | avg_silver_sim | avg_margin |
|---|---:|---:|---:|---:|---:|---:|
| Llama2-7B zero1 | 124 |  |  |  |  |  |
| Llama2-7B 微调1 | 124 |  |  |  |  |  |

#### 子任务2
| 模型结果 | 条数 | 正确 | 准确率 |
|---|---:|---:|---:|
| Llama2-7B zero2 | 124 |  |  |
| Llama2-7B 微调2 | 124 |  |  |

---

## 结果分析

从当前结果看，微调相比 zero-shot 有明显提升。以 **Qwen2-7B** 为例：

- **子任务1**：29.84% → 45.97%
- **子任务2**：26.61% → 36.29%

这表明，针对非字面表达翻译与识别任务，基于 Dataset A 进行任务微调能够有效提升中等规模开源模型的表现。
