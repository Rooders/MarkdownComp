# Review 1

We kindly ask the reviewer to reconsider the assessment, as we believe there has been a misunderstanding of the contributions of our work:

**Comment:**  For some translation directions, DeMPT built upon the ``bloomz-7b1-mt`` seems no better than Transformer.

**Answer:**  We have two points to address this concern:
- The inferior performance of  DeMPT built upon the ``bloomz-7b1-mt `` compared to Transformer is due to the **inherent capability difference** across different base models (for example, the DeMPT built upon stronger  ``llama-2-7b`` base model surpasses Transformer across all translation directions), rather than an issue caused by our proposed approach. On the contrary, the DeMPT can enhance the capability of the base large model to make up for the performance gap with the original Transformer in context-aware MT. A detailed analysis of **inherent capability difference** across different LLMs, i.e., base models, has exceeded the scope of this research. This paper primarily focuses on improving the context-aware MT capability of LLMs, and experiments suggest that the proposed DeMPT is significantly superior to the traditional fine-tuning methods in enhancing the context-aware MT capability of LLMs.
- In the LLMs-based MT community, experimental results from related studies ([Li et al., 2023; Wang et al., 2023; Zhu et al., 2023; Mu et al., 2023; Zheng et al., 2024](#1)) indicate that the translation performance of certain larger-scale LLMs in specific translation directions may not match that of transformer models trained on domain-specific data. The experimental results presented in this paper serve to corroborate this observation further.
  
**Comment:** Provide a scientific and logical basis for the following assertion:

> Importantly, the intra-sentence context inherently contains richer parallel semantic information with the target sentence and should be given a higher priority than the inter-sentence context. Consequently, we propose that separately modeling and utilizing the inter- and intra-sentence contexts should explicitly inform LLMs of the document-level context and the current sentence itself, thus being able to prevent the misallocation of attention weights to source-side tokens.

**Answer:** Since the intra-sentence context, i.e., the source sentence, is semantically identical to the target sentence. It may be natural and intuitive to assign a higher priority of attention to the intra-sentence context compared to the inter-sentence context which contains non-parallel information from the target sentence. The experimental results well demonstrate this, in which our model with decoding-enhanced strategy explicitly improves the weight of the intra-sentence sentence during the decoding phase and performs better than these models with the misallocation of attention weights, i.e., with equal attention weight to inter- and intra-context.

Therefore, if we have clarified any of the misunderstandings, we kindly ask for reconsideration of the scores.


## References
- Yachao Li, Junhui Li, Jing Jiang and Min Zhang. Enhancing document-level translation of large language model via translation mixed-instructions in _arXiv preprint arXiv:2401.08088_. 2023  
- Longyue Wang, Chenyang Lyu, Tianbo Ji, Zhirui Zhang, Dian Yu, Shuming Shi and Zhaopeng Tu. Document-level machine translation with large language models in _arXiv preprint arXiv:2304.02210_. 2023
- Wenhao Zhu, Hongyi Liu, Qingxiu Dong, Jingjing Xu, Lingpeng Kong, Jiajun Chen, Lei Li and Shujian Huang. Multilingual machine translation with large language models: Empirical results and analysis in _arXiv preprint arXiv:2304.04675_. 2023
- Yongyu Mu, Abudurexiti Reheman, Zhiquan Cao, Yuchun Fan, Bei Li, Yinqiao Li, Tong Xiao, Chunliang Zhang and Jingbo Zhu. Augmenting large language model translators via translation memories in _arXiv preprint arXiv:2305.17367_. 2023
- Jiawei Zheng, Hanghai Hong, Xiaoli Wang, Jingsong Su, Yonggui Liang and Shikai Wu. Fine-tuning Large Language Models for Domain-specific Machine Translation in _arXiv preprint arXiv:2402.15061_. 2024

# To AC 
To the attention of the ACs and SACs,

We are writing to express our concern regarding the assessment of our work by the Reviewer zLGp due to the **unreasonable questions and suggestions** in this review.

Specifically, the following statements by Reviewer zLGp are particularly telling of the quality of the review.

- the reviewer suggested it lacks convincing evidence that DeMPT built upon ``bloomz-7b-mt`` performs worse than basic Transformer in some translation directions. However, the performance gap here is obviously from the **inherent capability difference** across various foundation models, and our DeMPT built upon ``llama-2-7b`` outperforms the basic Transformer across all translation tasks. To provide evidence for this and analyze **inherent capability difference** across various foundation models, i.e., LLMs, has exceeded the scope of this research. 
- the reviewer asked us to provide a scientific and logical basis for some common-sense assertions in MT research. For example, the assertion of _the intra-sentence context inherently contains richer parallel semantic information with the target sentence and should be given a higher priority than the inter-sentence context._

We believe a closer look at our paper's details would alleviate the Reviewer's concerns. Your assistance in prompting the reviewer to reconsider the assessment of our work based on the rebuttal would be greatly appreciated.

Thank you for your time and consideration.



# Review 2

We sincerely appreciate the comments from you. Our response to the raised concerns or questions is as follows, with the expectation that they will effectively address the concerns and further improve the soundness of this work.

**Comment:** The discussion about the difference between proposed DeMPT and MSP.

**Answer:** DeMPT mainly differs from MSP in the following aspects:
- DeMPT adopts a phase-aware prompt to enable distinctive modeling for different inputs, namely inter-sentence contexts, intra-sentence contexts, and the target sentence, a feature not present in MSP.
- DeMPT incorporates a decoding-enhanced strategy to further improve the effectiveness of utilizing different context information, a capability not available in MSP.
- DeMPT is designed to alleviate discourse problems in context-aware LLM-based machine translation tasks, rather than addressing sentence-level machine translation tasks as in the case of MSP.
- DeMPT is designed to adapt **LLMs** rather than **small pre-trained model** used in MSP.

We have briefly discussed the differences in lines 547-550 of the paper, and we will provide a more detailed discussion on this in the revised version.

**Comment:** Most of the baselines are not document-level MTs, which makes the results less convincing.

**Answer:** Since most existing document-level machine translation (DMT) studies are Transformer-based, a direct comparison to our large language model (LLM)-based approach may be unfair. Therefore, we have not included document-level MT baselines in this study. In the revised version, we will provide a comparison with the state-of-the-art Transformer-based DMT models.

**Comment:** There is no ablation study. The claim for the heuristic method in the decoding phase is not well-supported.

**Answer:** On the one hand, we have provided an ablation study on the effect of the heuristic _decoding-enhanced strategy_ across all translation tasks in Tables 1 and 2. The sole difference between DeMPT and MPT is whether they are equipped with the heuristic decoding-enhanced strategy or not. Thus, we can readily observe the effect of the heuristic _decoding-enhanced strategy_ by comparing DeMPT with MPT. For instance, the heuristic decoding-enhanced strategy yields an average improvement of +0.69 BLEU score across the five translation directions. Please refer to Tables 1 and 2 for more details on the effect of the _heuristic decoding-enhanced strategy_ across different evaluation metrics and foundation models. Additionally, we provide a comprehensive ablation study in Appendices D and E to clarify the impact of the individual modules in our DeMPT.

On the other hand, we have conducted another ablation study on the ZH-ENtranslation direction using the ``bloomz-7b-mt`` model as the foundation model. This study clarifies the effect of the three $p$  in Equation 17 for the heuristic _decoding-enhanced strategy_:

|  Model | BLEU | COMET | BlonDe |       
| --- | --- | --- | --- | 
| **MPT**     | 31.81 | 0.8601 | 51.56 | 
| **DeMPT** | 32.46 | 0.8649 | 52.68 | 
|  _w/o_ $\hat{p}$ | 32.33 | 0.8629 | 52.04 | 
|  _w/o_ $\bar{p}$ | 32.11 | 0.8641 | 52.54 | 

We observe that removing $\hat{p}$, i.e., _w/o_ $\hat{p}$, leads to a significant degradation in the discourse-related metric, namely the BlonDe. This is because the integration of $\hat{p}$ enhances the utilization of the inter-sentence context during the decoding phase. Additionally, removing $\bar{p}$ results in the most substantial degeneration in BLEU metric. This observation demonstrates that our heuristic _decoding-enhanced strategy_ can distinctively improve the utilization of various contexts during the decoding phase. We will include these ablation results in the revised version.

**Comment:** The motivation is not clear. Why choose to use prompt tuning-based methods instead of in-context learning for LLMs?

**Answer:** On the one hand, unlike in-context learning (ICL), which is a prompt engineering approach rather than a fine-tuning method as it does not introduce trainable parameters, DeMPT needs to introduce trainable parameters, namely trainable prompts, to enable large language models (LLMs) to better handle different roles across various phases. On the other hand, Prompt Tuning is an effective fine-tuning method for complex generation tasks, such as machine translation ([Li and Liang, 2021; Liu et al., 2022](#1)). Therefore, we have implemented DeMPT based on Prompt Tuning, and we will explore further implementations of DeMPT leveraging different fine-tuning methods in the future.

## References
- Xiang Lisa Li and Percy Liang. 2021. Prefix-tuning: Optimizing continuous prompts for generation. In ACL-IJCNLP.
- Xiao Liu, Kaixuan Ji, Yicheng Fu, Weng Tam, Zhengxiao Du, Zhilin Yang, and Jie Tang. 2022. P-tuning: Prompt tuning can be comparable to fine-tuning across scales and tasks. In  ACL.


# Review 3

We sincerely appreciate your kind comments. The insightful and considerate suggestions, such as those regarding the description of experimental results and missing related works, are very helpful in improving the soundness and clarity of this paper.

**Comment:** The comments for the experiment results.

**Answer:** Thank you for your considerate suggestions regarding the description of the Experimental Results section. We will rephrase the descriptions in this section to make them more concise and ensure the focal points are emphasized.

**Comment:** The more complete related works.

**Answer:** Thank you for pointing out the missing related works with a similar hypothesis in our paper. Including these works is crucial for providing a more comprehensive comparison and better understanding of our approach. We will include these related works in the revised version.

**Comment:** Calculating statistical significance only for BLEU.

**Answer:** We use paired bootstrap resampling, which is implemented in the sacreBLEU toolkit, to calculate statistical significance for BLEU scores. The official toolkits for BlonDe and COMET metrics do not seem to support the calculation of statistical significance. Therefore, we have only reported the statistical significance for BLEU scores in this study. 

**Comment:** More limitation claim about non-English-centric translation directions.

**Answer:** Thank you for the insightful suggestion regarding experiments with non-English-centric translation directions. We will further explore this aspect in the future and add a relevant comment in the Limitations section.

**Comment:** Why is discussion built upon the bloom model used instead of llama, and only the language pair zh->en and the metrics BLEU and BlonDe

**Answer:** Taking into account the computing resources consumed (In practice, our experiments have shown that models based on Bloom require less computing source and time), we have chosen Bloom model exclusively in the ZH-EN translation task to discuss our approach in Section 4. We intend to make this clarification in the revised version. Additionally, due to space constraints, only the metrics for BLEU and BlonDe are presented in Section 4. In the revised version, We will include three metrics.

**Comment:**  The graphs in Figure 4 are of different types.

**Answer:** Thank you for the suggestion. We will ensure that the graphs are uniform in type in Figure 4 for the revised version.
