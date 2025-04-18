Thank you for submitting your paper to KDD! I appreciate your effort to explore counterspeech generation, which is an important and underexplored topic. The paper proposes a simple and easy-to-follow method. However, as outlined in the weaknesses section, I have the following concerns:

The authors utilize two types of evaluation metrics: the first assesses the semantic and lexical quality of counterspeech, while the second relies on a score derived from JudgeLM. Are these metrics truly legitimate for evaluating the quality of counterspeech? How can we ensure there is no overlap between the two types? For instance, JudgeLM might already account for semantic and stylistic features. Moreover, if the main limitation of using JudgeLM is the potential for bias, as noted in the paper, then the complementary metrics should mainly focus on mitigating such bias. However, metrics like ROUGE-L and BLEU are not designed for that.
一方面：我们使用的几个评测方式是行业内大家都在用的普遍的counterspeech的度量，他们也被广泛用于各种生成任务，如文本摘要翻译等等，他们的评估的有效性也已经得到了认可。另一方面，我们也在3.2节做了人工评估，而人工评估的结果是和自动评估的结果一致的，即经过gen-ranking后的结果是大于baseline的结果，也进一步证明了本文使用的评测方法的有效性。



I also find that the inference objective is largely aligned with the evaluation metrics. For example, if we select candidates based on higher ROUGE-L and BLEU scores from a set of generated outputs, then naturally, those candidates will perform better under those same metrics. This evaluation setup seems unfair, it's like designing a quiz and then taking it yourself. A more rigorous approach would involve either using new, comprehensive, and convincing metrics, or having human annotators perform a thorough evaluation.

一方面，正如本文主要目标-旨在生成高质量的counterspeech，或者说对齐人工产生的counterspecch。ROUGE-L and BLEU BERT往往能通过单词覆盖率和语义相似度来效率对齐人工counterspecch。因此我们通过对齐这几个评估指标来找到更好的counterspeech是比较合理的。另一方面，使用完全由人工标注的会大大加大成本，这可能和自动生成counterspeech的目标相悖。



Regarding baselines, the paper currently only considers in-context learning (ICL) and chain-of-thought prompting. More diverse and relevant baselines, such as existing counterspeech generation models should also be included.

据我们所知，基于大模型的counterspeech生成主要是集中在使用ICL上，因此我们实现了这个baseline作为对比。而其他相关的工作，主要基于小模型的基础上，直接与我们的方法对比是不公平的，因此我们并没有将他们包含进来。



In Section 3.22 (Point-wise Scoring), if ground-truth scores can be directly calculated, why is there a need to train a predictor to approximate them?

我们在3.22计算ground-truth scores是构建predictor的训练集，用于对齐高质量的counterspeech分布。但在推理阶段，由于没有golden counterspeech是无法获得ground-truth scores的。而被训练好的predictor是reference-free的，在推理阶段在不提供reference的情况下，能找出高质量的

## Reviewer 2

Missing comparisons with RLHF-based methods (e.g., DPO, PPO, or rejection sampling), which are directly relevant for tasks involving response preference modeling.


No comparison to guided or constrained decoding techniques, which are standard for controlling generation quality and style — particularly important in safety-critical tasks like CS.

Q1，Q2，Q3，Q5，Q7
RLHF-based method是用于对齐偏好，主要用于模型生成内容的纠正，其并不能产生具体分数用于排序，因此和我们提出的方法相关性较少且无法比较。而我们的方法并未涉及任何的constrained decoding techniques，因此我们任务这个比较也是不合理的。而由于在multilingual的counterspeech领域训练集的稀缺性，去实现一个基于大模型的fine-tune也是不切实际的，事实上这类方法也是没有的。另外，如果在Counterspeech领域有使用constrained decoding techniques、RLHF-based、fine-tuned的方法的文献，也请您列出。


Generation is entirely unconstrained; diversity is achieved only via temperature and prompt variation, missing more sophisticated decoding methods.
我们ranking方法需要借助模型生成文本的多样性，来产生不同的结果，从而选择处更好的结果。如果进行解码限制，这一目的将很难达到。
The core contribution is a scoring-based selection, yet the generation process itself is relatively naïve and unchanged from basic prompting.

Only prompting-based baselines are considered; no strong fine-tuned generation model or retrieval-augmented generation baseline is included.
No iterative loop or feedback from ranking to generation — the method is static and lacks learning to improve over time.
我们对您的这个问题不太理解，我们并不需要iterative loop和feedback去生成。
Lack of ablations comparing re-ranking to reranking+finetuning, which would test if the selected outputs can improve future generations.


## Reviewer 3
Could you provide the code to ensure reproducibility?
Yes, we will provide all of the collected dataset and code later.

How to select examples from the training set to serve as context in the LLM input?
我们采用了一个随机选择的策略来选择example时， 同时我们在5.5节中也就行了其他选择策略的对比，实验结果表明，随机选择的策略更优。
JudgeLM does not incorporate domain knowledge regarding counterspeech responses. Could this omission introduce bias when evaluating the answers?
事实上，本文使用的judgeLM是将领域知识通过prompt注入到了输入中的。我们将在修改的版本中明确这一点。
Why are the values 10 and 400 used in Equation 3? Have other values been considered, and if so, what was the rationale for selecting these?
这个是ELO算法计算期望值时的使用的固定常量，具体可以参考。。
You mention that fine-tuning or retraining is computationally expensive and aim to avoid it. However, you also train LLaMA-3.1-8B as a point-wise scoring model. Could you clarify this apparent contradiction? Additionally, could you provide the performance results of fine-tuned LLMs for comparison, along with an analysis of computational efficiency to support your claims?
一方面，8b模型相对于其他更大的模型是相对轻量级的，比如32B 70B， 同时我们的point-wise的模型训练花费的训练时间是非常少的，仅仅训练了一个数据轮次。同时，为了解决您的顾虑，我们也使用了llama-3.1-8b进行了微调英文上进行了微调，结果如下：
|------------------------|-------------|----------|-------------|----------|-------------|----------|-------------|----------|-------------|----------|-------------|----------|
|                        | *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.|
| **`LLaMA-3.1-8B`**     |             |          |             |          |             |          |             |          |             |          |             |          |
| ASR-DMT                | 77.88       | 5.712    | 79.79       | 5.651    | 80.69       | 5.477    | 76.99       | 5.211    | 79.01       | 5.401    | 78.87       | 5.490    |

可以看出，由于训练数据的不足，使用参数量较少的大模型进行微调的结果并不尽人意。


