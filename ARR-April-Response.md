We sincerely appreciate the comments from you. Our response to the raised concerns or questions is as follows, with the expectation that they will effectively address the concerns and further improve the soundness of this work.

**Comment:** The discussion about the difference between proposed DeMPT and MSP.

**Response:** DeMPT mainly differs from MSP in the following aspects:
- DeMPT adopts a phase-aware prompt to enable distinctive modeling for different inputs, namely inter-sentence contexts, intra-sentence contexts, and the target sentence, a feature not present in MSP.
- DeMPT incorporates a decoding-enhanced strategy to further improve the effectiveness of utilizing different context information, a capability not available in MSP.
- DeMPT is designed to alleviate discourse problems in context-aware LLM-based machine translation tasks, rather than addressing sentence-level machine translation tasks as in the case of MSP.
- DeMPT is designed to adapt **LLMs** rather than **small pre-trained model** used in MSP.

We have briefly discussed the differences in lines 547-550 of the paper, and we will provide a more detailed discussion on this in the revised version.

**Comment:** Most of the baselines are not document-level MTs, which makes the results less convincing.

**Response:** Since most existing document-level machine translation (DMT) studies are Transformer-based, a direct comparison to our large language model (LLM)-based approach may be unfair. Therefore, we have not included document-level MT baselines in this study. In the revised version, we will provide a comparison with the state-of-the-art Transformer-based DMT models.

**Comment:** There is no ablation study. The claim for the heuristic method in the decoding phase is not well-supported.

**Response:** On the one hand, we have provided an ablation study on the effect of the heuristic _decoding-enhanced strategy_ across all translation tasks in Tables 1 and 2. The sole difference between DeMPT and MPT is whether they are equipped with the heuristic decoding-enhanced strategy or not. Thus, we can readily observe the effect of the heuristic _decoding-enhanced strategy_ by comparing DeMPT with MPT. For instance, the heuristic decoding-enhanced strategy yields an average improvement of +0.69 BLEU score across the five translation directions. Please refer to Tables 1 and 2 for more details on the effect of the _heuristic decoding-enhanced strategy_ across different evaluation metrics and foundation models. Additionally, we provide a comprehensive ablation study in Appendices D and E to clarify the impact of the individual modules in our DeMPT.

On the other hand, we have conducted another ablation study on the ZH-ENtranslation direction using the ``bloomz-7b-mt`` model as the foundation model. This study clarifies the effect of the three $p$  in Equation 17 for the heuristic _decoding-enhanced strategy_:

|  Model | BLEU | COMET | BlonDe |       
| --- | --- | --- | --- | 
| **MPT**     | 31.81 | 0.8601 | 51.56 | 
| **DeMPT** | 32.46 | 0.8649 | 52.68 | 
|  _w/o_ $\hat{p}$ | 32.33 | 0.8629 | 52.04 | 
|  _w/o_ $\bar{p}$ | 32.11 | 0.8641 | 52.54 | 

We observe that removing $\hat{p}$, i.e., _w/o_ $\hat{p}$, leads to a significant degradation in the discourse-related metric, namely the BlonDe. This is because the integration of $\hat{p}$ enhances the utilization of the inter-sentence context during the decoding phase. Additionally, removing $\bar{p}$ results in the most substantial degeneration in BLEU metric. This observation demonstrates that our heuristic _decoding-enhanced strategy_ can distinctively improve the utilization of various contexts during the decoding phase. We will include these ablation results in the revised version.

**Comment:** The motivation is not clear. Why choose to use prompt tuning-based methods instead of in-context learning for LLMs?

**Response:** On the one hand, unlike in-context learning (ICL), which is a prompt engineering approach rather than a fine-tuning method as it does not introduce trainable parameters, DeMPT needs to introduce trainable parameters, namely trainable prompts, to enable large language models (LLMs) to better handle different roles across various phases. On the other hand, Prompt Tuning is an effective fine-tuning method for complex generation tasks, such as machine translation ([Li and Liang, 2021; Liu et al., 2022](#1)). Therefore, we have implemented DeMPT based on Prompt Tuning, and we will explore further implementations of DeMPT leveraging different fine-tuning methods in the future.
