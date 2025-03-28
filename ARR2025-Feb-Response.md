
##Review 1
Generally, I do not have found any major concerns about the submission, I have only these two points:

Thank you very much for your kind comments and for recognizing our work. We respond to two concerns you mentioned as follows:

**Q:**  The statistical test in the main results (Table 1).

**A:** Since COMET does not support statistical significance testing, we currently do not provide significance tests for COMET scores. However, we will consider report significance testing based on BLEU scores in the revised version.

**Q:** _Why did you choose only LLaMa and GPT-4o-mini out of four originally tested models_?

**A:** Considering the computational resource consumption and API costs, our ablation study maintains the same model selection setup as other analytical experiments in Section 5, in which we selected one lightweight open-source model and a closed-source model with a more cost-effective API from the four candidate models as representatives for the experiments.

Additionally, we have supplemented more comprehensive ablation experiments in the other two models, and the results are as follows:

| **System**             | **En ⇒ De** |          | **En ⇒ Ru** |          |
|------------------------|-------------|----------|-------------|----------|
|                        | *s*-Comet   | *d*-Comet| *s*-Comet   | *d*-Comet|
| **`LLaMA-3.1-70B`**     |             |          |             |          |
| DoCIA                  | 82.63       | 6.373    | 82.69       | 6.168    |
|     *w/o* R.D.         |        | 5.812    | 77.50       | 5.431    |
|     *w/o* S.C.         | 78.63       | 5.792    | 77.81       | 5.331    |
|     *w/o* L.C.         | 78.41       | 5.761    | 77.88       | 5.311    |
| **`GPT-3.5-turbo`**      |             |          |             |          |
| DoCIA                  | 82.95       | 6.192    | 81.97       | 5.841    |
|     *w/o* R.D.         | 82.66       | 6.299    | 83.11       | 6.116    |
|     *w/o* S.C.         | 83.11       | 6.231    | 83.88       | 6.061    |
|     *w/o* L.C.         | 83.01       | 6.201    | 83.77       | 6.011    |

We will include the new experimental results in the revised version.


# Review 2
There is one critical baseline that is needed to understand how promising this technique is. You use ASR-SMT (sentence-level translation) and ASR-DMT (document-level translation, using all preceding context) as your baselines. What about using DMT with a fixed number of preceding segments? The equivalent would be DoCIA with only MT stage and a fixed short-term-context of m segments. This is a basic technique for document-level MT that should have been included in the comparison.

The writing style and language is overly complicated at times, and this makes the explanation hard to follow. The paper would benefit from a careful rewrite to improve clarity and correctness. See for example lines 246-249 and 346.
Figure 2 can be improved. For example, the determining module is labelled as "identify". This verb does not help the reader to understand what is being done here. Consider that the determining module is basically making an "accept/reject" decision over the proposed ASR/MT correction. For ASR/MT context, the context is labelled as "retrieval". I would argue that retrieval should be put instead in the blue arrow going from Document-level Context to Long- and Short-Memory Context.
All equations of Section 2.1 use argmax, which implies a search, but I see no mention of search elsewhere in the paper. How are you making these decisions? Beam search? Greedy search? Sampling?
Section 4. Is this the full DoCIA (a-m-p)? I'm assuming so, but this should be stated somewhere, considering that DoCIA a-m performs better for some configurations.
I do not understand section 4.3. Online DoCIA refers to the setting where you add the produced transcriptions/translations to the ASR and MT histories, respectively. This makes sense, and it is what you would do during inference in a production scenario. What is the offline setting? You say that "the context is not updated and relies solely on segment-level translation/ASR results" What does this mean? What is being done in the offline scenario ff you are not using the transcriptions/translations of your system? This is a critical issue that needs to be clarified.
Figure 8 is titled "ASR Refinement Prompt Template", but it is about MT refinement.
Please indicate which specific version of MuST-C you used for the experiments (v1,v3,etc)

# Review 3

The contribution of LLM refinement is not new in L115. It is already been done in [1] where the system performed ASR refinement and MT doc-level refinement with LLMs for speech translation. Hence, this needs to be edited as the main novelty in the paper lies in Multi-Level Context Integration and Refinement Determination Mechanism

The Multi-Level Context Integration use BM25 instead of the other retrieval techniques such as using embeddings-based or others that are used for LLM few-shot prompting.

The refinement determinaton mechanism can also be better explored as currently it is based on edit distance. Using quality estimation would be much more relevant such as Speech QE [2]

[1] Koneru, S., Nguyen, T. B., Pham, N. Q., Liu, D., Li, Z., Waibel, A., & Niehues, J. (2024, August). Blending LLMs into Cascaded Speech Translation: KIT’s Offline Speech Translation System for IWSLT 2024. In The 21st International Conference on Spoken Language Translation (p. 231).

[2]SpeechQE: Estimating the Quality of Direct Speech Translation (Han et al., EMNLP 2024)

One main suggestion would be is to mention if you use segmenter to split the audio or use the gold segmentation provided by MuST-C? I could not find this in the main paper and should be mentioned there

In Introduction, i would also appreciate if there are few lines on the impact of segmentation of long-form audio on ST outputs and how document-level refinement can help alleviate this issue. Currently, the motivation is only refining ASR outputs but not alleviating segmentation issues.

L451 The discussion of offline/online is unclear.  The conventional definition of online ST means simultaneous translation as far as i know and it is a bit confusing. Please clarify here

