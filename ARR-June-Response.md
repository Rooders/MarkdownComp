## Response for Reviewer 1
We sincerely appreciate your comments in the April and June review circles. These comments are invaluable for further perfecting this work. If our responses and revisions in the two review rounds effectively address the concerns you raised, we kindly ask you to consider reassgining the overall and soundness scores.

## Response for Reviewer 2

**Q**: Aside from using automatic evaluation scores on translation tasks, deeper analytical experiments on verifying the benefits of the proposed "inter-sentence" and "intra-sentence" training are needed.

**A**: In the latest revision, we have analyzed the detailed benefits of phase-wise training, i.e., inter-sentence and intra-sentence training, in Section 4.2. In addition to these analyses based on **automatic evaluation scores**, we also report comparative analyses using **human evaluation methods** in *Section 4.3* and *Appendix H*. These manual analyses not only include detailed overall score comparisons and *case studies* but also include *some manual error statistics* (the cases in the proposed approach perform worse than one-phase methods, i.e., without inter-sentence and intra-sentence training). However, due to the high cost of human evaluation, we only conducted these manual evaluations on one-phase training. In the future, we will conduct more manual analyses and comparisons, including but not limited to:

- Manual analysis of two-stage training, i.e., the system of *Merging 1&2* in Tables 4 and 5, including case studies, error statistics, etc.
- Manual comparison for addressing discourse problems, such as anaphora and ellipsis.

Meanwhile, we would also appreciate any further suggestions you might have in this regard. And again, if our revision in Section 4.3 and Appendix H can address this issue to some extent, we sincerely hope that you would consider raising your evaluation score for this work. 

## Response for Reviewer 3
**Q**: The datasets are pretty small. It is still unclear whether concatenation-based approaches would catch up to the proposed approach on larger datasets. The appendix (I) includes some experiments with moderately larger training sets, but these are still far from production-level scale.
**A**: In the one hand, due to the scarcity of publicly available parallel discourse corpora, it may be very difficult to verify the effectiveness of our proposed approach on production-level corpora. Therefore, we use a comparable, even larger, dataset scale with some related works, such as the 240K dataset scale used in Wu et al., Zheng et al., and Petrick et al.

In the other hand, we further extend the dataset scale to 1 million in Appendix I to verify the effectiveness of our approach.

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| **CMT-PT**   | 30.82 | 0.8504 |49.61| 
|  300K+700K|  |  |  |
| **DeMPT**   | 32.46 | 0.8649 | 50.62 | 
|   300K+700K | | | |

From the result in the Table, we observe ... 


#### References
- Document-Level Language Models for Machine Translation. Frithjof Petrick, Christian Herold, Pavel Petrushkov, Shahram  Khadivi and Hermann Ney. WMT. 2023.
- Adapting large language models for document-level machine translation. Minghao Wu, Thuy-Trang Vu, Lizhen Qu, George Fosterand Gholamreza Haffari, preprint, 2024.
- Fine-tuning Large Language Models for Domain-specific Machine Translation. Jiawei Zheng, Hanghai Hong, Xiaoli Wang, Jingsong Su, Yonggui Liang and Shikai Wu. preprint. 2024.



@article{zheng2024fine,
  title={Fine-tuning Large Language Models for Domain-specific Machine Translation},
  author={Jiawei Zheng, Hanghai Hong, Xiaoli Wang, Jingsong Su, Yonggui Liang and Shikai Wu.},
  journal={arXiv preprint arXiv:2402.15061}
  year={2024}
}

@article{wu2024adapting,
  title={Adapting large language models for document-level machine translation},
  author={},
  journal={arXiv preprint arXiv:2401.06468},
  year={2024}
}

@inproceedings{petrick2023document,
  title={Document-Level Language Models for Machine Translation},
  author={Petrick, Frithjof and Herold, Christian and Petrushkov, Pavel and Khadivi, Shahram and Ney, Hermann},
  booktitle={Proceedings of the Eighth Conference on Machine Translation},
  pages={375--391},
  year={2023}
}


