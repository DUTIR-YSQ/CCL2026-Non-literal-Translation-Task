# 测试结果提交说明

请各位参赛选手按照以下格式提交测试结果文件。

## 1. 文件格式
- 提交文件格式：`xlsx`
- 请使用 Excel 文件进行提交，不接受其他格式。

## 2. 表头要求
测试文件需包含以下三列：

| ID | task1 | task2 |
|----|-------|-------|

其中：
- `ID`：样本编号
- `task1`：子任务1的预测结果
- `task2`：子任务2的预测结果

## 3. 示例
提交文件内容示例如下：

| ID   | task1        | task2 |
|------|--------------|-------|
| 2077 | I can't hide | B     |

## 4. 提交要求
- 请确保表头名称与要求完全一致：`ID`、`task1`、`task2`
- 每一行对应一个测试样本
- 不要修改 `ID`
- `task1` 和 `task2` 中填写模型预测结果
- 最终提交文件请保存为 `.xlsx` 格式

## 5. 注意事项
- 请勿增加额外列
- 请勿删除或修改原始测试样本编号
- 请检查是否有空值、缺失值或格式错误

__________________________________
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
| 模型结果 | 命中率 | avg_gold_sim | avg_silver_sim | avg_margin |
|---|---:|---:|---:|---:|
| Qwen2-7B zero-shot | 29.84% | 0.3809 | 0.4777 | -0.0968 |
| Qwen2-7B Fine-tune | 45.97% | 0.3282 | 0.3070 | 0.0211 |

#### 子任务2
| 模型结果 | 准确率 |
|---|---:|
| Qwen2-7B zero-shot | 26.61% |
| Qwen2-7B Fine-tune | 36.29% |


---

## 结果分析
从当前结果看，微调相比 zero-shot 有明显提升。以 **Qwen2-7B** 为例：

- **子任务1**：29.84% → 45.97%
- **子任务2**：26.61% → 36.29%

这表明，针对非字面表达翻译与识别任务，基于 Dataset A 进行任务微调能够有效提升中等规模开源模型的表现。
