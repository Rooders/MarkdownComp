## Review 1

We sincerely appreciate the comments from you. Our response to the raised concerns or questions is as follows, with the expectation that they will effectively address the concerns and further improve the soundness of this work.

**Q**: This paper appears to be an application of LLM to the G-transformer idea. Given this, there should be experimental results when using sentence-by-sentence translation and multi-sentence translation methods in G-transformer.

**A**: We apologize for the unclear description of the G-transformer, which led to this misunderstanding. Essentially, the G-transformer is a traditional document-level model with an encoder-decoder architecture. In Table 1, G-transformer and +mBART represent models trained from random initialization and pre-trained parameters, respectively. Therefore, deploying a decoder-only large language model (LLM) with the G-transformer concept is not feasible. Moreover, the G-transformer is specifically designed for document-level (many-to-many) translation, making sentence-by-sentence (one-to-one) or multi-sentence (many-to-one) translation methods unsuitable. We will provide a clearer description of the G-transformer in the next version.

**Q**: The searching for the appropriate values of $\lambda_1$ and $\lambda_2$ for different language pair.

**A**: Due to the limited computational resources, we did not perform extensive experiments to find the optimal combination of $\lambda_1$ and $\lambda_2$ for different translation tasks, simply setting them to be equal. For example, verifying each combination of \$\lambda_1$ and $\lambda_2$ requires 10 experiments (5 x 2 for the number of translation directions and foundation models). However, in response to your interest, we conducted targeted experiments using a combination of $\lambda_1$ and $\lambda_2$ on ZH-EN and FR-EN only. The results are as follows:

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| $\lambda_1$=1/3, $\lambda_2$=1/3, ZH-EN   | 32.46 | 0.8649 | 50.62 | 
|**$\lambda_1$=1/4, $\lambda_2$=1/3, ZH-EN**| 32.51 | 0.8653 | 50.31 |
| $\lambda_1$=1/3, $\lambda_2$=1/3, FR-EN   | 41.92 | 0.8790 | 58.30 | 
| **$\lambda_1$=1/4, $\lambda_2$=1/3, FR-EN** |41.82 |0.8785 | 57.92|

We use a smaller value for $\lambda_1$ here and observe that the BlonDe scores are more sensitive to changes in $\lambda_1$ compared to BLEU and COMET. For example, a smaller $\lambda_1$ results in -0.31 and -0.38 for ZH-EN and FR-EN, respectively. This sensitivity is reasonable because $\lambda_1$ is used for adjusting the utilization of inter-sentence context.

**Q**: Combine context and inter-sentence as a single LLM.

**A**: We merge phases 1 and 2 into a single encoding phase (merged encoding, i.e., transferring the single-phase CMT-PT into the two-phase CMT-PT) to conduct experimentation on the ZH-EN translation task:

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| CMT-PT  | 30.82 | 0.8504 | 49.61 |
| MPT  | 31.81 | 0.8601 | 50.22 |
| DeMPT  | 32.46 | 0.8649 | 50.62 | 
| **merged encoding**| 31.01 | 0.8503 | 49.91 |

We observe the model with **merged encoding** shows slight improvement over CMT-PT but still lags behind MPT and DeMPT, demonstrating the effectiveness of our multi-phase and decode-enhanced strategy.

## Review 2

We are grateful for the insightful comments you have offered. The following is our response to the concerns and questions raised, hoping that it will satisfactorily address them and enhance the soundness of this research.

**Q**: The authors do not discuss why they are not using target-side context. While this can be acceptable, the choice of using source-side context only should be justified. Ideally, a baseline using both source and target-side context should be added.

**A**: In our humble opinion, using the target-side context would introduce an additional exposure bias, as the ground truth target-side context is utilized during training, whereas during inference, the target-side context is predicted by the MT model itself. We implemented a baseline using both source and target-side context in Appendix G (CMT-PT with m2m in Table 11) to compare with our DeMPT. We plan to integrate the target-side context into DeMPT for comparison in the next version.

**Q**: Does that "long-distance issue" apply only to C, but not S? 

**A**: In our humble opinion, the length of S is much shorter than C (C includes one or more sentences, while S consists of only a single sentence), thereby mitigating the long-distance dependency issue for S. The long-distance issue may be not significant for S. 

**Q**: why is BlonDe higher without $\hat{p}$ or $\bar{p}$, but the text says otherwise?

**A**: Thank you for pointing out this error. We inadvertently transcribed the incorrect BLOND score from other experiments. The correct BLOND score should be 50.29 (without $\hat{p}$) and 50.51 (without $\bar{p}$) in Table 8. We will rectify this in the next version.

**Q**: Could concatenation catch up to the proposed approach with more data?

**A**: Thank you for the suggestion. We will conduct a comparative experiment, in which we will extend the training set by incorporating additional document-level corpora.  

**Q**: How is latency almost the same as the baseline(s) when you need to combine 3 different predictions?

**A**: There appears to be a slight misunderstanding due to our unclear description of the three predictions. The computation of $\hat{p}$ and $\bar{p}$ does not require a full decoding forward pass. Instead, it only involves forwarding through an FFN layer (comprising two linear transformation layers and a ReLU activation layer), an LLM-Head layer (a linear transformation layer), and a softmax function layer. Consequently, the additional latency compared to the baselines should be reasonably small. We will provide a clearer description of this in the next version.

**Q**: How would you apply the proposed approach when the source and target documents do not have the same number of sentences?

**A**: In a document pair, all training sentence pairs must match to ensure parallelism between the target and source documents. Our model is trained on these instances, mostly single-sentence pairs, but some may have differing numbers of sentences due to language variations, though they remain parallel in _training sentence_. During inference, the model automatically determines the appropriate number of sentences for translation based on the given source sentence.

**Q**: For concatenation, while it's true that a priori the source and context have equal prioritization (L56), could LLMs learn the appropriate prioritization (through their attention mechanisms)?

**A**: We conducted experiments to verify whether LLMs using the concatenation strategy appropriately learn to prioritize attention to the source and context. In these experiments, we replaced the original inter-sentence context with a random inter-sentence context, i.e., randomly selecting sentences from other documents, to generate the target translation. The results are as follows:

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| CMT-PT _w/_ org. CTX   |  30.82 | 0.8504|  49.61 |
| **CMT-PT _w/_ rnd. CTX**   | 28.63 | 0.8402| 48.01|
| DeMPT _w/_ org. CTX   | 32.46 | 0.8649 | 50.62 |
| **DeMPT _w/_ rnd. CTX**   | 31.56 | 0.8581 | 49.71 |

The random context (_w/_ rnd. CTX) consistently degrades the performance of both the CMT-PT and DeMPT models. However, the performance deterioration of CMT-PT is more pronounced than that of DeMPT (-2.19/0.0102/1.60 vs. -0.90/0.0068/0.91). This may indicate that LLMs using the concatenation strategy (CMT-PT) inappropriately prioritize the random context, resulting in more severe performance degradation.

Lastly, we appreciate your tender comments on the missing citation and typo error. We will fix them in the next version.

## Review 3

We appreciate the insightful comments you provided. In response to the raised concerns and questions, we present the following, aiming to address them satisfactorily and improve the soundness of this research.

**Q**: The efficiency of the proposed work is of concern. Both the proposed multi-phase prompt tuning and the "decoding-enhanced approach" need extra forward-passing costs. (Although Section 4.3 shows the inference time does not increase much.)

**A**: There seems to be a slight misunderstanding due to our unclear description of the three predictions in the decoding-enhanced approach. The computation of $\hat{p}$ and $\bar{p}$ does not require a full decoding forward pass. It only involves an FFN layer (two linear transformation layers and a ReLU activation layer), an LLM-Head layer (a linear transformation layer), and a softmax function layer. Therefore, the additional latency compared to the baselines should be minimal. We will describe this more clearly in the next version.

**Q**: The original LLM encoding can also capture the differences and significance of inter- and intra-sentence context via a large attention map. The proposed method explicitly divides the encoding processing into three phases and then aggregates the separately encoded hidden states by a gate mechanism (with $\lambda$ hyperparameters). Is it inherently better? Why?

**Q**:The results in Table 1 and Table 2 show the general effectiveness of the proposed method on the translation task using sacreBLEU, COMET, and BlonDe metrics. Is there any deep analysis of the reason for the proposed method utilizing inter- and intra-sentence better? (Section 4.5 and Appendix D can provide some insights though.)

**A**: We conducted experiments to address the two questions. Here, we replaced the original inter-sentence context with a random inter-sentence context, meaning we randomly selected some sentences from other documents to serve as the inter-sentence context for generating the target translation. The results are as follows:

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| CMT-PT _w_ org. CTX   |  30.82 | 0.8504|  49.61 |
| **CMT-PT _w_ rnd. CTX**   | 28.63 | 0.8402| 48.01|
| DeMPT _w_ org. CTX   | 32.46 | 0.8649 | 50.62 |
| **DeMPT _w_ rnd. CTX**   | 31.56 | 0.8581 | 49.71 |

The performance of both the CMT-PT and DeMPT models consistently deteriorates when exposed to random context (w rnd. CTX). However, the decline in performance is more pronounced for CMT-PT than for DeMPT (-2.19/0.0102/1.60 vs -0.90/0.0068/0.91). This suggests that DeMPT, with its multi-phase and decoding-enhanced approach, exhibits greater resilience in utilizing inter-sentence context compared to CMT-PT. DeMPT appears to allocate more appreciative focus to inter-sentence context and effectively mitigate the negative impact of incorrect inter-sentence context.

**Q**: How do we choose better hyperparameters introduced in this paper? Why are the current settings good or optimal? Would the settings be different when using other LLMs or MT datasets?

**A**: For experimental efficiency and limited computational resources, we tune hyperparameters like prompt length and learning rate specifically for the ZH-EN translation task, utilizing ``bloomz-7b1-mt`` as our base model. Afterward, we apply these optimized hyperparameters directly to other translation tasks, including those utilizing ``llama-2-7b``, without individual tuning for each task. We will clear it in the next version.


We thank you for bringing attention to the typo error. We will rectify these issues in the next version.







