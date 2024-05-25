## Review 1

We sincerely appreciate the comments from you. Our response to the raised concerns or questions is as follows, with the expectation that they will effectively address the concerns and further improve the soundness of this work.

**Q**: This paper appears to be an application of LLM to the G-transformer idea. Given this, there should be experimental results when using sentence-by-sentence translation and multi-sentence translation methods in G-transformer.

**A**: We sorry about the unclear descrption of G-transformer and lead to this misunderstanding. Essentially, G-transformer is a small translation model with _encoder-decoder_ structure. So it is not available for application of _decoder-only_ LLM with G-transformer idea. Meanwhile, G-transformer is pertinently designed for the document-level translation. Therefore, applying the sentence-by-sentence translation and multi-sentence translation methods for the G-transformer also is unworkable.

**Q**: The searching for the appropriate values of $\lambda_1$ and $\lambda_2$ for different language pair.

**A**: Due to the limitation of compute source, we did not conduct the addtional experiments to search a most appropriate combination of $\lambda_1$ and $\lambda_2$, and just simply set them be equal. We assume there are three combinations of $\lambda_1$ and $\lambda_2$, e.g., 1/3 and 1/3, 1/4 and 1/3, and 1/3 and 1/4, we need to conduct 3 x 5 = 15 experimations to search the best combination for the five translaton directions. However, we follow your interet and conduct a simple experimentation using an addional comination of $\lambda_1$ and $\lambda_2$ on ZH-EN and FR-EN. The results as follows:

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| $\lambda_1$=1/3, $\lambda_2$=1/3, ZH-EN   | 32.46 | 0.8649 | 50.62 | 
|**$\lambda_1$=1/4, $\lambda_2$=1/3, ZH-EN**|  |  |  |
| $\lambda_1$=1/3, $\lambda_2$=1/3, FR-EN   | 41.92 | 0.8790 | 58.30 | 
| **$\lambda_1$=1/4, $\lambda_2$=1/3, FR-EN** | | | |

We use samller value for $\lambda_1$ here, and can observe ...

**Q**: Combine context and inter-sentence as a single LLM.

**A**: We merge the phase 1 and 2 into a single encoding phase, i.e., transfering the three-phase process into the two-phase process, to conduct a experimentaion on ZH-EN translation task: 

|  Model | BLEU | COMET | BlonDe |
| --- | --- | --- | --- | 
| Three-phase DeMPT  | 32.46 | 0.8649 | 50.62 | 
|**Two-phase DeMPT**|  |  |  |

We obvervse the two-phase DeMPT is ...


## Review 2

We sincerely appreciate the comments from you. Our response to the raised concerns or questions is as follows, with the expectation that they will effectively address the concerns and further improve the soundness of this work.

**Q**: DeMPT is not use target-side context, and a baseline using both source and target-side context should be added.

**A**: We assume incorpate the target-side context into DeMPT or CMT-PT is less efficient. On the one hand, the incorporation of target-side context will increase length of the decoding sequence and aggravate the long-distance issues. On the other hand, the using of target-side context will introduce an aditional exposure bias, i.e., the ground truth target-side context is used for training while at inference the target-side context is prediected by MT model. We also compare our DeMPT to the CMT-PT with target-side context in Appendix G (_m2m_ in Table 11).

**Q**: Does that "long-distance issue" apply only to C, but not S? 

**A**: Generally, the length of S is much shorter than C (C includes one or more sentences while S is consisted of one sentence only), thus the long-distance issue is not sigficant for S. 

**Q**: why is BlonDe higher without $\hat{p}$ or $\bar{p}$, but the text says otherwise?

**A**: Thank you for pointing this mistake, we cursorily transcibe the Blonde score based ``llama-2-7b`` model (should have be based ``bloomz-7b1-mt``, 50.29 (w/o $\hat{p}$) and 50.51 (w/o $\bar{p}$) in Table 8. We will fix it in the next version. 


**Q**: Could concatenation catch up to the proposed approach with more data?

**A**: Thank your for the suggeston. We will conduct a comparation experimentation, in which we will extend the training set with other document-level corpus.  

**Q**: How is latency almost the same as the baseline(s) when you need to combine 3 different predictions?

**A**: There are a little misunderstanding due to our unclear description for the three predictions. The compute of $\hat{p}$ and $\bar{p}$ _do not need a full decoding forward_, it only forwards a FFN layer (two linear transfer layer and a ReLU transfer layer), a LLM-Head layer (a linear transfer layer) and a softmax function layer. Therefore, it may be resonable for little latency compared to the baselines.
