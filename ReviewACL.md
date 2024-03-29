# Review 1

**Q:**  For some translation directions, DeMPT built upon the ``bloomz-7b1-mt`` seems no better than Transformer.

**A:**  We have two points to response this question:
- The inferior performance of  DeMPT built upon the ``blooms-7b1-mt `` compared to Transformer is due to the **inherent capability difference** across different base models (for example, the DeMPT built upon stronger  ``llama-2-7b`` base model surpasses Transformer across all translation directions), rather than an issue caused by our proposed approach. On the contrary, the DeMPT can enhance the capability of the base large model to make up for the performance gap with the original Transformer in context-aware MT. While a detailed analysis  about **inherent capability difference** across different LLMs, i.e., base models, has obviously exceeded the scope of this research. This paper mainly focuses on improving the context-aware MT capability of LLMs, and experiments in this paper  suggest that the proposed DeMPT is significantly superior to the traditional fine-tuning methods in improving the context-aware MT capability of LLMs.
- In LLMs-based MT community, experimental results in related studies ([Li et al., 2023; Wang et al., 2023; Zhu et al., 2023; Mu et al., 2023; Zheng et al., 2024](#1)) show that the translation performance of some larger-scale LLMs in some translaiton directions are not as good as transformer models trained on some domain-specific data. These experimental results in this paper may verify this finding again.

**Q:** Provide a scientific and logical basis for the some assertion.

**A:** The information in intra-sentence context is parallel with that in target sentence, it may be natural and intuitive that giving intra-sentence context a higher priority of attention compared to inter-sentence context with a non-parallel information from target sentence.

## References
- Yachao Li, Junhui Li, Jing Jiang and Min Zhang. Enhancing document-level translation of large language model via translation mixed-instructions in _arXiv preprint arXiv:2401.08088_. 2023  
- Longyue Wang, Chenyang Lyu, Tianbo Ji, Zhirui Zhang, Dian Yu, Shuming Shi and Zhaopeng Tu. Document-level machine translation with large language models in _arXiv preprint arXiv:2304.02210_. 2023
- Wenhao Zhu, Hongyi Liu, Qingxiu Dong, Jingjing Xu, Lingpeng Kong, Jiajun Chen, Lei Li and Shujian Huang. Multilingual machine translation with large language models: Empirical results and analysis in _arXiv preprint arXiv:2304.04675_. 2023
- Yongyu Mu, Abudurexiti Reheman, Zhiquan Cao, Yuchun Fan, Bei Li, Yinqiao Li, Tong Xiao, Chunliang Zhang and Jingbo Zhu. Augmenting large language model translators via translation memories in _arXiv preprint arXiv:2305.17367_. 2023
- Jiawei Zheng, Hanghai Hong, Xiaoli Wang, Jingsong Su, Yonggui Liang and Shikai Wu. Fine-tuning Large Language Models for Domain-specific Machine Translation in _arXiv preprint arXiv:2402.15061_. 2024

# Review 2

We sincerely appreciate the comment from you. Our response to the raised concerns or questions is as follows, with the expectation that they will effectively address the concerns and further improve the soundness of this work.

**Q:** The discussion about the difference between proposed DeMPT and MSP.

**A:** DeMPT mainly differs from MSP in the following aspects:
- DeMPT is designed to adapt **LLMs** rather than **pre-trained model** used in MSP.
- DeMPT adopts a **phase-aware prompt** to make distinctive modeling for different inputs, i.e., inter- , intra-sentence contexts and target sentence, which is **not available** in MSP.
- DeMPT adopts a **decoding-enhanced strategy** to further improve the effectiveness of utilization of different context informations, which is **not available** in MSP.
- DeMPT is designed for the alleviation of discourse problems in **context-aware LLM-based** MT task rather than that for **sentence-level** MT task in MSP. 

We have discussed the differences in lines 547-550 of the paper briefly, and will add a more detailed discussion about this in the revised version.

**Q:** Most of the baselines are not document-level MTs, which makes the results less convincing.

**A:** Considering most of existing document-level MT (DMT) studies are Transformer-based, direct comparison to our LLMs-based approach may be unfair, we therefore do not involve the document-level MT baselines. We will involve the SOTA Transformer-based DMT in the revised version if required.

**Q:** There is no ablation study. The claim for the heuristic method in the decoding phase is not well-supported.

**A:**  On the one hand, we have provided the ablation study about the effect of heuristic decoding-enhanced strategy across all translation tasks in Tables 1 and 2. For example, the ablation results in BLEU  when taking ``llama-2-7b`` as the foundation model as follows:

|  Model | ZH-EN | FR-EN | DE-EN | ES-EN | RU-EN |       
| --- | --- | --- | --- | --- | --- | 
| **MPT**     | 33.21 | 43.11 | 43.88 | 52.01 | 36.49 |
| **DeMPT** | 33.89 | 43.71 | 44.69 | 53.10 | 36.55 |

The only difference between DeMPT and MPT is whether they are equipped with the _heuristic decoding-enhanced strategy_ or not. Thus we can handily observe the the effect of heuristic decoding-enhanced strategy by comparing DeMPT with MPT. For example, the heuristic decoding-enhanced strategy gains a + 0.69 BLEU score averaged across five translation directions here. Please refer to Tables 1 and 2 for more details about the effect of _heuristic decoding-enhanced strategy_ across different evaluation terms and foundation models. On the other hand, we have constructed another ablation study on ZH-EN translation direction when taking ``bloomz-7b-mt`` as the foundation model, which clarified the effect of three $p$ in _Equ 17_ for _heuristic decoding-enhanced strategy_ :

|  Model | BLEU | COMET | BlonDe |       
| --- | --- | --- | --- | 
| **MPT**     | 31.81 | 0.8601 | 51.56 | 
| **DeMPT** | 32.46 | 0.8649 | 52.68 | 
|  _w/o_ $\hat{p}$ | 32.33 | 0.8629 | 52.04 | 
|  _w/o_ $\bar{p}$ | 32.11 | 0.8641 | 52.54 | 

We observe that removing $\hat{p}$, i.e., _w/o_ $\hat{p}$, brings a significant degeneration on discourse-related metric, i.e., BlonDe score. This is because the integration of $\hat{p}$ enhances the utilization of the inter-sentence context at the decoding phase. Differently, removing $\bar{p}$ brings the most degeneration in BLEU score. The observation demonstrates our _heuristic decoding-enhanced strategy_ can enhance the utilization of different context information during the decoding phase. Unfortunately, due to this ablation study being behind the deadline for paper submission, we do not present it in the submitted paper. We will add them to the revised version. Finally, we also provide a detailed ablation study in Appendix D and E to clarify the effect of the different modules in our DeMPT.

**Q:** The motivation is not clear. Why choose to use prompt tuning-based methods instead of in-context learning for LLMs?

**A:** On the one hand, compared to the in-context learning (ICL) method that does not require fine-tuning parameters, DeMPT needs to introduce trainable parameters, i.e., prompts, to enable the LLMs better play different roles across the different phases. On the other hand, Prompt Tuning also is an effective fine-tuning method for complex generation tasks, such as machine translation ([Li and Liang, 2021; Liu et al., 2022](#1)) . We therefore implement DeMPT upon Prompt Tuning, and we will explore more implementation of DeMPT over different fine-tuning methods in the future.

## Reference
- Xiang Lisa Li and Percy Liang. 2021. Prefix-tuning: Optimizing continuous prompts for generation. In ACL-IJCNLP.
- Xiao Liu, Kaixuan Ji, Yicheng Fu, Weng Tam, Zhengxiao Du, Zhilin Yang, and Jie Tang. 2022. P-tuning: Prompt tuning can be comparable to fine-tuning across scales and tasks. In  ACL.


# Review 3

We sincerely appreciate the kind comments from you. These insightful and considerable suggestions, such as for the description of experimental results and missing related works, are very helpful for improving the soundness and understanding of this paper.

**Q:** The comments for the experiment results.

**A:** Thanks for your considerable suggestions on the description of Experimental Results. We will rephrase the description in this section
 to make it more concise and the focal points stand out.

**Q:** The more complete related works.

**A:** Thank you for pointing out the missing of some related works with a similar hypothesis in our paper, which is crucial for a more comprehensive comparison and understanding of our approach. We will complete the related work in the revised version.

**Q:** Calculating statistical significance only for BLEU,

**A:** The official toolkits of BlonDe and COMET metrics seem not to support the calculation of statistical significance. So we only report the statistical significance in BLEU. We will survey more about the statistical significance of BlonDe and COMET and complete the statistical significance test in the revised version if it is possible. 
 
**Q:** More limitation claim.

**A:** Thank you for this insightful suggestion about experiments with non-English-centric translation direction. We will explore more about this in the future and add a related comment in the Limitation section.

**Q:** Why is discussion built upon the bloom model used instead of llama, and only the language pair zh->en and the metrics BLEU and BlonDe

**A:** Considering the consumption of computing source (In practice, we found the experiments built upon Bloom have less consumption of computing and time), we use the Bloom model only in the ZH-EN translation task to discuss our approach in Section 4. We will clarify this in the revised version. And yes, due to lack of space, we present only metrics of BLEU and BlonDe in Section 4. We will try to involve three metrics in the revised version.


**Q:**  The graphs in Figure 4 are of different types.

**A:** Thank you for this careful suggestion, we will adjust the graphs in Figure 4  in the revised version.
