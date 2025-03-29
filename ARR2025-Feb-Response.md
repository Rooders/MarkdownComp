
## Review 1

Thank you very much for your kind comments and for recognizing our work. We respond to two concerns you mentioned as follows:

**Q:**  The statistical test in the main results (Table 1).

**A:** Since COMET does not support statistical significance testing, we currently do not provide significance tests for COMET scores. However, we will consider report significance testing based on BLEU scores in the revised version.

**Q:** _Why did you choose only LLaMa and GPT-4o-mini out of four originally tested models_?

**A:** Considering the computational resource consumption and API costs, our ablation study maintains the same model selection setup as other analytical experiments in Section 5, in which we selected a lightweight open-source model and a closed-source model with a more cost-effective API from the four candidate models as representatives for the experiments.

Additionally, we have supplemented more comprehensive ablation experiments in the other two models, and the results are as follows:

| **System**             | **En ⇒ De** |          | **En ⇒ Ru** |          |
|------------------------|-------------|----------|-------------|----------|
|                        | *s*-Comet   | *d*-Comet| *s*-Comet   | *d*-Comet|
| **`LLaMA-3.1-70B`**    |             |          |             |          |
| DoCIA                  | 82.63       | 6.373    | 82.69       | 6.168    |
|     *w/o* R.D.         | 81.97       | 6.299    | 81.81       | 6.037    |
|     *w/o* S.C.         | 82.23       | 6.198    | 82.11       | 5.901    |
|     *w/o* L.C.         | 82.35       | 6.211    | 82.19       | 5.863    |
| **`GPT-3.5-turbo`**    |             |          |             |          |
| DoCIA                  | 82.95       | 6.192    | 81.97       | 5.841    |
|     *w/o* R.D.         | 82.19       | 6.104    | 81.01       | 5.806    |
|     *w/o* S.C.         | 82.46       | 6.037    | 81.23       | 5.711    |
|     *w/o* L.C.         | 82.31       | 6.072    | 81.35       | 5.694    |

We will include the new experimental results in the revised version.


# Review 2

We are very grateful for your thoughtful revision suggestions on this work. Our point-to-point response to your concerns is as follows:

**Q:** What about using DMT with a fixed number of preceding segments?
**A:** This is an insightful suggestion. Following your suggestion, we implement a DMT baseline with a fixed number of context segments ($$\text{ASR-DMT}_{fix}$$). For a fair comparison, we used the fixed $m+n$ proceeding segment as context. The number of context segments is the same as that used in our DoCIA. The detailed experimental result is as follows:
| **System**             | **⇒ De** |          | **⇒ It** |          | **⇒ Pt** |          | **⇒ Ru** |          | **⇒ Ro** |          | **Avg** |          |
|------------------------|-------------|----------|-------------|----------|-------------|----------|-------------|----------|-------------|----------|-------------|----------|
|                        | *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.| *s*-Co.   | *d*-Co.|
| **`LLaMA-3.1-8B`**     |             |          |             |          |             |          |             |          |             |          |             |          |
| ASR-DMT                | 77.88       | 5.712    | 79.79       | 5.651    | 80.69       | 5.477    | 76.99       | 5.211    | 79.01       | 5.401    | 78.87       | 5.490    |
| DoCIAₐ₋ₘ₋ₚ              | 79.15       | 5.912    | 80.88       | 5.909    | 81.75       | 5.757    | 78.39       | 5.556    | 80.54       | 5.734    | 80.15       | 5.774|
| $$\text{ASR-DMT}_{fix}$$| 78.12       | 5.701    | 79.77       | 5.632    | 80.89       | 5.521    | 77.15       | 5.236    | 79.25       | 5.416    | 79.04       | 5.501    |
| **`LLaMA-3.1-70B`**    |             |          |             |          |             |          |             |          |             |          |             |          |
| ASR-DMT                | 81.54       | 6.143    | 82.36       | 5.976    | 82.85       | 5.745    | 80.99       | 5.867    | 83.15       | 5.979    | 82.17       | 5.942    |
| DoCIAₐ₋ₘ₋ₚ              | 82.63       | 6.373    | 83.66       | 6.264    | 83.99       | 6.037    | 82.69       | 6.168    | 85.32       | 6.365    | 83.66       | 6.241    |
| $$\text{ASR-DMT}_{fix}$$| 81.36      | 6.129    | 82.39       | 6.031    | 83.01       | 5.773    | 81.13       | 5.901   | 83.19        |  6.015    | 82.22       | 5.970    |
| **`GPT-4o-mini`**      |             |          |             |          |             |          |             |          |             |          |             |          |
| ASR-DMT                | 82.42       | 6.108    | 83.52       | 5.833    | 83.32       | 5.943    | 82.80       | 5.948    | 84.82       | 6.018    | 83.37       | 5.970    |
| DoCIAₐ₋ₘ₋ₚ              | 83.64       | 6.444    | 84.76       | 6.387    | 84.51       | 6.297    | 84.32       | 6.286    | 86.34       | 6.424    | 84.71       | 6.368|
| $$\text{ASR-DMT}_{fix}$$| 82.69       | 6.102    | 83.81       | 5.852    | 83.11       | 5.931    | 83.15       | 5.961    | 84.79       | 6.021    | 83.51       | 5.973   |
| **`GPT-3.5-turbo`**    |             |          |             |          |             |          |             |          |             |          |             |          |
| ASR-DMT                 | 81.68       | 5.977    | 81.93       | 5.760    | 82.53       | 5.687    | 79.50       | 5.611    | 83.30       | 5.687    | 81.78       | 5.744    |
| DoCIAₐ₋ₘ₋ₚ               | 82.95       | 6.192    | 83.39       | 5.997    | 83.90       | 5.797    | 81.97       | 5.841    | 85.01       | 6.033    | 83.45       | 5.973   |
| $$\text{ASR-DMT}_{fix}$$| 81.79       | 5.993    | 82.15       | 5.779    | 82.51       | 5.681    | 79.74       | 5.625    | 83.21       | 5.673    | 81.88       | 5.750    |

The results show DMT with a fixed number of context segments is slightly better more than with all context segments but still behind our DoCIA. We will include the new results in our revised version.

**Q:** Writing Style & Clarity  
**A:** We sincerely appreciate your careful suggestions in paper writing. They are useful for improving the correctness and soundness of our work. We will follow these suggestions and do a throughout proofreading. Including but not limited to the follows:
- Adjust the module name and position Fig. 1.
- Clarify our sampling search strategy.
- Clarify the DoCIAₐ₋ₘ₋ₚ is our full DoCIA.

**Q:** Clarity of the offline and online setting

**A:** We detailed the online and offline setting as follows:

**Offline Setting** prioritizes **parallel processing** for efficiency but requires all audio segments of a speech to be preloaded. For example, when processing a speech with $$n$$ segments, we first extract their initial ASR results in parallel and all ASR of segments then undergo context-aware refinement in parallel. Therefore, the refinement of the $$i$$-th segment, i.e., $$\bar{s}\_{i}$$ to $${s}\_{i}$$, rely on the context consisted of **unrefined ASR results** of preceding segments, i.e., $${\bar{s}_{<i}}$$). The same logic applies to translation and translation refinement processes.  

**Online Setting** adopts the **sequential processing** and is similar to simultaneous translation. The refinement of the $$i$$-th segment ASR starts when the refinements of all preceding segments are done. Because the refinement of  $$\bar{s}\_{i}$$ to $$s\_{i}$$ depends on the **final refined ASR results** of preceding segments ($$s_{<i}$$). This dynamic context ensures higher accuracy but introduces latency, as each segment must wait for its predecessors to complete the entire pipeline.

The differece bettween them as follow:

| Dimension       | Offline Setting                        | Online Setting                        |
|-----------------|--------------------------------|-------------------------------|
| **Processing Logic** | all segment parallel processing | Segment-by-segment sequential processing |
| **Used Context**      | Static (unrefined results $$\bar{s}_{<i}$$)      | Dynamic (refined results $$s_{<i}$$)      |
| **Latency**      | Low (parallel computation)      | High (sequential dependency)   |
| **Use Case**     | Non-real-time post-processing translation   | Real-time simultaneous translation      |


**Key Takeaway**:  
- **Offline** trades **context freshness** (using unrefined context) for parallel speed, ideal for non-real-time tasks (e.g., post-processing recordings).  
- **Online** trades **latency** for refined context accuracy, suited for real-time streaming (e.g., live interpretation).  

*(Notation: *̄sᵢ* = initial ASR result of segment *i*; *sᵢ* = refined result; *{̄s_{<i}}* = unrefined context; *{s_{<i}}* = refined context.)* 

## 5. Online/Offline Mode Explanation

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

