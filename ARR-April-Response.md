## Review 1

We sincerely appreciate the comments from you. Our response to the raised concerns or questions is as follows, with the expectation that they will effectively address the concerns and further improve the soundness of this work.

**Q**: This paper appears to be an application of LLM to the G-transformer idea. Given this, there should be experimental results when using sentence-by-sentence translation and multi-sentence translation methods in G-transformer.

**A**: We are sorry about the unclear description of the G-transformer which led to this misunderstanding. Essentially, the G-transformer is a small translation model with an _encoder-decoder_ structure. So it is not available for application of _decoder-only_ LLM with G-transformer idea. Meanwhile, the G-transformer is pertinently designed for document-level translation. Therefore, applying the sentence-by-sentence translation and multi-sentence translation methods for the G-transformer also is unworkable.

**Q**: The searching for the appropriate values of $\lambda_1$ and $\lambda_2$ for different language pair.

**A**: Due to the limitation of the compute source, we did not conduct the additional experiments to search for the most appropriate combination of $\lambda_1$ and $\lambda_2$, and just simply set them to be equal. We assume there are three combinations of $\lambda_1$ and $\lambda_2$, e.g., 1/3 and 1/3, 1/4 and 1/3, and 1/3 and 1/4, we need to conduct 3 x 5 = 15 experimentations to search the best combination for the five translation directions. However, we follow your interest and conduct simple experimentations using an additional combination of $\lambda_1$ and $\lambda_2$ on ZH-EN and FR-EN. The results are as follows:

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| $\lambda_1$=1/3, $\lambda_2$=1/3, ZH-EN   | 32.46 | 0.8649 | 50.62 | 
|**$\lambda_1$=1/4, $\lambda_2$=1/3, ZH-EN**|  |  |  |
| $\lambda_1$=1/3, $\lambda_2$=1/3, FR-EN   | 41.92 | 0.8790 | 58.30 | 
| **$\lambda_1$=1/4, $\lambda_2$=1/3, FR-EN** | | | |

We use a smaller value for $\lambda_1$ here, and can observe ...

**Q**: Combine context and inter-sentence as a single LLM.

**A**: We merge the phase 1 and 2 into a single encoding phase, i.e., transferring the three-phase process into the two-phase process, to conduct experimentation on ZH-EN translation task: 

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| Three-phase DeMPT  | 32.46 | 0.8649 | 50.62 | 
|**Two-phase DeMPT**|  |  |  |

We observe the two-phase DeMPT is ...


## Review 2

We sincerely appreciate the comments from you. Our response to the raised concerns or questions is as follows, with the expectation that they will effectively address the concerns and further improve the soundness of this work.

**Q**: DeMPT does not use target-side context, and a baseline using both source and target-side context should be added.

**A**: We assume incorporating the target-side context into DeMPT or CMT-PT is less efficient. On the one hand, the incorporation of target-side context will increase the length of the decoding sequence and aggravate the long-distance issues. On the other hand, the using of target-side context will introduce an additional exposure bias, i.e., the ground truth target-side context is used for training while at inference the target-side context is predicted by MT model. We also compare our DeMPT to the CMT-PT with target-side context in Appendix G (_m2m_ in Table 11).

**Q**: Does that "long-distance issue" apply only to C, but not S? 

**A**: Generally, the length of S is much shorter than C (C includes one or more sentences while S consists of one sentence only), thus the long-distance issue is not significant for S. 

**Q**: why is BlonDe higher without $\hat{p}$ or $\bar{p}$, but the text says otherwise?

**A**: Thank you for pointing out this mistake, we cursorily transcibe the Blonde score based ``llama-2-7b`` model (should have been based ``bloomz-7b1-mt``, 50.29 (w/o $\hat{p}$) and 50.51 (w/o $\bar{p}$) in Table 8. We will fix it in the next version. 


**Q**: Could concatenation catch up to the proposed approach with more data?

**A**: Thank you for the suggestion. We will conduct a comparison experimentation, in which we will extend the training set with other document-level corpus.  

**Q**: How is latency almost the same as the baseline(s) when you need to combine 3 different predictions?

**A**: There is a little misunderstanding due to our unclear description of the three predictions. The compute of $\hat{p}$ and $\bar{p}$ _do not need a full decoding forward_, it only forwards an FFN layer (two linear transfer layers and a ReLU transfer layer), an LLM-Head layer (a linear transfer layer) and a softmax function layer. Therefore, it may be reasonable for little latency compared to the baselines.

**Q**: How would you apply the proposed approach when the source and target documents do not have the same number of sentences?

**A**: A document pair in the training set must have the same segment pair, i.e., parallelity between target and source. Our model is trained over these segment-level training instances. Most of these segment pairs are a sentence pair. However, some of the segment pairs in the training set have different numbers of source and target sentences due to the differences among customs of language expression. Therefore, during inference, the model will automatically decide the number of sentences in translation when giving a source segment.

**Q**: For concatenation, while it's true that a priori the source and context have equal prioritization (L56), could LLMs learn the appropriate prioritization (through their attention mechanisms)?

**A**: We experiment to verify if the LLMs with the concatenation strategy learn appropriate attention (i.e., prioritization) to source and context. In this experimentation, we replace the original inter-sentence context with a random inter-sentence context, i.e., randomly select some sentences from other documents, to generate the target translation. The results are as follows:

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| CMT-PT _w_ org. CTX   |  30.82 | 0.8504|  49.61 |
| **CMT-PT _w_ rnd. CTX**   | 28.63 | 0.8402| 48.01|
| DeMPT _w_ org. CTX   | 32.46 | 0.8649 | 50.62 |
| **DeMPT _w_ rnd. CTX**   | 31.56 | 0.8581 | 49.71 |

The random context (_w_ rnd. CTX) consistently degenerates the performance of the CMT-PT and DeMPT models. However, the deterioration of CMT-PT is more serious than that of DeMPT (-2.19/0.0102/1.60 vs -0.90/0.0068/0.91). This may indicate the LLMs with concatenation strategy (CMT-PT) pay inappropriate attention to the random context and lead to more serious deterioration.

## Review 3

**Q**: The efficiency of the proposed work is of concern. Both the proposed multi-phase prompt tuning and the "decoding-enhanced approach" need extra forward-passing costs. (Although Section 4.3 shows the inference time does not increase much.)

**A**: There is a little misunderstanding due to our unclear description of the three predictions in decoding-enhanced approach. The compute of $\hat{p}$ and $\bar{p}$ _do not need a full decoding forward_, it only forwards an FFN layer (two linear transfer layers and a ReLU transfer layer), an LLM-Head layer (a linear transfer layer) and a softmax function layer. Therefore, it may be reasonable for little latency compared to the baselines. We will describe this more clearly in the next version.

**Q**: The original LLM encoding can also capture the differences and significance of inter- and intra-sentence context via a large attention map. The proposed method explicitly divides the encoding processing into three phases and then aggregates the separately encoded hidden states by a gate mechanism (with $\lambda$ hyperparameters). Is it inherently better? Why? **AND**
The results in Table 1 and Table 2 show the general effectiveness of the proposed method on the translation task using sacreBLEU, COMET, and BlonDe metrics. Is there any deep analysis of the reason for the proposed method utilizing inter- and intra-sentence better? (Section 4.5 and Appendix D can provide some insights though.)

**A**: We conduct experimentation and hope to answer the two questions. Here, we replace the original inter-sentence context with a random inter-sentence context, i.e., randomly select some sentences from other documents, to generate the target translation. The results are as follows:

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| CMT-PT _w_ org. CTX   |  30.82 | 0.8504|  49.61 |
| **CMT-PT _w_ rnd. CTX**   | 28.63 | 0.8402| 48.01|
| DeMPT _w_ org. CTX   | 32.46 | 0.8649 | 50.62 |
| **DeMPT _w_ rnd. CTX**   | 31.56 | 0.8581 | 49.71 |

The random context (_w_ rnd. CTX) consistently degenerates the performance of the CMT-PT and DeMPT models. However, the deterioration of CMT-PT is more serious than that of DeMPT (-2.19/0.0102/1.60 vs -0.90/0.0068/0.91). This may indicate the multi-phase and decoding-enhanced DeMPT is more robust for utilization of inter-sentence context than CMT-PT. DeMPT can pay more appreciative attention to inter-sentence context and weaken the negative effect of incorrect inter-sentence context.

**Q**: How do we choose better hyperparameters introduced in this paper? Why are the current settings good or optimal? Would the settings be different when using other LLMs or MT datasets?

**Q**: For the experimental effectiveness and limited compute resource, we tune hyperparameters, such as prompt length and learning rate, on ZH-EN translation task based ``bloomz-7b1-mt``, and then simply apply the optimal hyperparameters for other translation tasks (including the tasks based ``llama-2-7b`` ) and do not tune these hyperparameters again on different tasks.







