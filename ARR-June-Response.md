## Response for Reviewer 1
Thank you very much for your kind comments and for recognizing our work. We’re also glad to hear that you believe our current version is free of significant weaknesses. If you have any further concerns, welcome to discuss with us.

## Response for Reviewer 2

We are very grateful for your thoughtful revision suggestions on this work during the two rounds of review. Our response in this round is as follows:

**Q**: Aside from using automatic evaluation scores on translation tasks, deeper analytical experiments on verifying the benefits of the proposed "inter-sentence" and "intra-sentence" training are needed.

**A**: In the latest revision, we have analyzed the detailed benefits of phase-wise training, i.e., inter-sentence and intra-sentence training, in Section 4.2. In addition to these analyses based on **automatic evaluation scores**, we also report comparative analyses using **human evaluation methods** in *Section 4.3* and *Appendix H*. These manual analyses not only include detailed overall score comparisons and *case studies* but also include *some manual error statistics* (the cases in the proposed approach perform worse than one-phase methods, i.e., without inter-sentence and intra-sentence training). However, due to the high cost of human evaluation, we only conducted these manual evaluations on one-phase training. In the future, we will conduct more manual analyses and comparisons, including but not limited to:

- Manual analysis of two-stage training, i.e., the system of *Merging 1&2* in Tables 4 and 5, including case studies, error statistics, etc.
- Manual comparison for addressing discourse problems, such as anaphora and ellipsis.

Meanwhile, we would also appreciate any further suggestions you might have in this regard. Again, if our revision in Section 4.3 and Appendix H can address this issue to some extent, we sincerely hope that you would consider raising your evaluation score for this work. 

## Response for Reviewer 3

We sincerely appreciate your comments in both the April and June review circles. We are thankful that our revisions have successfully addressed most of the issues identified in the earlier version. Our response in this review round is as follows:

**Q**: The datasets are pretty small. It is still unclear whether concatenation-based approaches would catch up to the proposed approach on larger datasets. The appendix (I) includes some experiments with moderately larger training sets, but these are still far from production-level scale.

**A**: On the one hand, the limited availability of publicly accessible parallel document-level corpora can make it quite challenging to assess the effectiveness of our proposed approach with production-level data. The dataset used in our experiments is comparable to, or even larger than, the parallel document-level dataset utilized in some related work, e.g., Wu et al., Zheng et al., and Petrick et al.

On the other hand, we further extend the dataset scale in Appendix I to 1 million and verify the effectiveness of our approach.

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| **CMT-PT**| 30.82 | 0.8504 |49.61
|  300K+200K|  31.21 | 0.8521 |49.88
|  300K+400K|  31.73 | 0.8555 |50.11
|  300K+700K|  31.89 | 0.8559 | 50.23 
| **DeMPT**  | 32.46 | 0.8649 | 50.62
|  300K+200K|  32.77 |0.8663 |50.99
|  300K+400K|  33.56 | 0.8701 |51.47
|  300K+700K | 33.91| 0.8721| 51.97|

The results show that the larger-scale dataset further widened the performance gap between CMT-PT and DeMPT. Our DeMPT outperforms CMT-PT by 1.64/0.0145/1.01 across three metrics at the 300K dataset scale, while the gap is enlarged to 2.02/0.0162/1.74 with the 300K+700K training dataset. Meanwhile, we also observe the performance improvement of CMT-PT has already converged with the expansion of the data scale. Further increasing the scale of training data may not improve CMPT's performance to narrow the gap with DeMPT.

**Q**: Writing and citation error in lines 302, 309,481.

**A**: Thank you for your thoughtful comments on our writing and the missing citations. We will address these issues in the next version.

**Q**: Examine how the proposed approach interacts with few-shot demonstrations.

**A**: This is an excellent idea. We are also very interested in this suggestion. We are considering the possibility of interaction through an 'instruction-tuning + DeMPT' approach. Specifically, we could first construct an instruction-tuning dataset that incorporates examples of inter- and intra-sentence context utilization. Each example would be a triple: (inter-sentence context, intra-sentence context, target). We plan to explore this idea further in future research and welcome to discuss more with us.

Lastly, if our response and revision in the two review rounds effectively address the concerns you raised, we kindly ask you to consider raising the overall and soundness scores. Thank you again for your kind comments on this work.


#### References
- Document-Level Language Models for Machine Translation. Frithjof Petrick, Christian Herold, Pavel Petrushkov, Shahram  Khadivi and Hermann Ney. WMT. 2023.
- Adapting large language models for document-level machine translation. Minghao Wu, Thuy-Trang Vu, Lizhen Qu, George Foster and Gholamreza Haffari, preprint, 2024.
- Fine-tuning Large Language Models for Domain-specific Machine Translation. Jiawei Zheng, Hanghai Hong, Xiaoli Wang, Jingsong Su, Yonggui Liang and Shikai Wu. preprint. 2024.
