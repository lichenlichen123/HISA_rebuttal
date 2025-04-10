# Rebuttal for HISA
## Reviewer 1
Thank you for your valuable comments and suggestions.  

**W1:** 
We have included a larger dataset including 2,676 users, 3,440 POIs, and 116,757 check-ins. As shown in Table 1, incorporating HISA yields a 15.1% improvement in HR and 9.2% in MRR compared to without HISA. 

Table 1: Performance of HISA on the SIN dataset.
|Metrics|HR@5|MRR@5|HR@10|MRR@10 |
|-|-|-|-|-|
| HISA|0.122|0.107|0.136|0.112|
| w/o HISA|0.109|0.092|0.115|0.106|

**W2:** We will replace the exponential base with *exp* in Eq. (10) and check for similar issues throughout the paper.

**W3:** We have now included the training time of HISA, as shown in Table 2. Compared to the recent baseline DCHL [1] from SIGIR 2024, as mentioned by *Reviewer 4Sah*, our approach achieves comparable training efficiency.

Table 2: Comparisons of Training Time (Second/Minute [s/m]).  
"Training per epoch" is the average training time for each epoch; "Training" denotes the total training time to converge.
| Datasets|Time|HISA w/o re_ex|HISA|DCHL|
|-|-|-|-|-|
|CAL|Training per epoch|6.2s|7.8s|7.9s|
|          |Training|5m|7m|9m|
|CHA|Training per epoch|9.8s|13.1s| 16.6s|
|          |Training|9m|13m|15m|
|PHO|Training per epoch|18.5s|21.1s| 24.8s|
|          |Training|15m|19m|22m|

*References:  
[1] Disentangled contrastive hypergraph learning for next POI recommendation, 2024, in: SIGIR.*

**W4:** The results in Table 2 correspond to the purple curve in Fig. 7, and HR@10 on the CHA dataset should be 0.159 instead of 0.151; this typo will be corrected in the revised version. The training process is run for up to 100 epochs and is stopped early if the validation loss does not improve for 10 consecutive epochs. Moreover, each experiment is repeated five times under the same settings, and the average results are reported. Importantly, all reported improvements over baseline methods are statistically significant with p-value < 0.01.


## Reviewer 2
Thank you for your valuable feedback and suggestions.  
**W1:**
The *review* and *examine* stages serve distinct purposes. 
Specifically, the *review* improves model performance by re-learning units stored in the revisiting instance memory (RIM), following the principle of "reviewing the old to learn the new." The *examine* identifies units that require re-learning—such as incorrectly predicted ones—during training and updates them in RIM for subsequent review. In short, the *review* focuses on re-learning, while the *examine* selects what to review. We will highlight this in the future manuscript.

**W2:**
Our cosine similarity-based difficulty estimator can handle long-tail or clustered distributions of POI embeddings. Specifically, we use a Gaussian kernel (Eq. 10) as a scheduling strategy, which serves as a density-based method that implicitly captures the difficulty distribution. This kernel guides the model to prioritize “typical” samples that are densely distributed and closer to the central tendency—similar to clustering—while deemphasizing those in the long-tail regions. As a result, it helps mitigate the effect of long-tail distributions, especially in the early stages of training. We will highlight this point in the future manuscript.

**W3:**
The three weights in Eq. 9 and 15 are empirically set, each from [0.1, 0.8] and summing to 1. We chose the best-performing combinations, with specific values provided in Section 5.1.

**W4:**
Following [52,32], we used CAL, CHA, and PHO, which are suitable for uncertain check-ins, as NYC, TKY, and CA lack the required features. To demonstrate scalability, we also additionaly tested on SIN (2,676 users, 3,440 POIs, 116,757 check-ins). As shown in Table 1, HISA improves HR by 15.1% and MRR by 9.2%.

Moreover, each experiment is repeated five times with the same settings, and average results are reported for reliability. All reported improvements over baseline methods are statistically significant with p-value < 0.01.  

Table 1: 
|Dataset|HR@5|MRR@5|HR@10|MRR@10|
|-|-|-|-|-|
|HISA|0.122|0.107|0.136|0.112|
|w/o HISA|0.109|0.092|0.115|0.106|


## Reviewer 3
Thank you for your valuable comments.  
**W1:**
Uncertain check-ins refer to cases where a user enters a collective POI (e.g., a shopping mall), but due to GPS inaccuracy or privacy protection, the specific individual POI (e.g., a store inside the mall) remains unknown. In our manuscript, Fig. 1 provides a visual example, and the concept is introduced in Section 1. We also follow prior works[52,32], where the definition and setting are formally established.

**W2:**
If A simple instance and a difficult instance are equally distant from the threshold *σ*, they receive the same weight *ρ*. The Gaussian kernel, however, captures both instance-level difficulty and the overall difficulty distribution. It emphasizes typical instances near *σ* while down-weighting long-tail samples early in training, improving stability. Thus, instance prioritization depends on both difficulty and its distribution, enabling more stable and gradual learning. We will highlight this in the future manuscript.

**W3:**
We added two recent baselines: (1) SNPM (AAAI 2023) and (2) DCHL (SIGIR 2024, mentioned by Reviewer 4Sah). Due to time constraints, STHGCN (2023) cannot be finished. As shown in Table 1, HISA outperforms these methods.

Table 1:
| Dataset |CAL||||PHO||||
|-|-|-|-|-|-|-|-|-|
|Metrics| HR@5| MRR@5|HR@10|MRR@10|HR@5|MRR@5|HR@10|MRR@10|
|DCHL|0.115|0.107|0.132|0.123|0.112|0.109|0.137|0.114|
|SNPM|0.129|0.111|0.155|0.132|0.128|0.106|0.159|0.119|

**W4:**
First, for the *HISA w/o ex*: since *examine* is used to detect incorrectly predicted units (IPUs), *HISA w/o ex* yields the same results as *HISA w/o IPU*, which is clarified in Section 5.5 (Page 7).
Second, *w/o HISA* removes all three proposed stages (acquire, review, examine), reverting to the original loss (Eq. 7) to replace the loss functions Eqs. 11 and 13. 
Lastly, Fig. 7 shows dataset-specific effects. Removing HLU slightly reduces performance in CAL and CHA, but has little impact on PHO. Possible reasons include: (1) PHO has more uncertain check-ins and complex trajectories, so HLU may capture behaviors that hinder preference modeling; (2) the threshold-based HLU selection may introduce noise (e.g., sparse trajectories). This motivates future work on more robust HLU selection strategies.

## Reviewer 4
Thank you for your valuable feedback. 

**W1:** 
1. To our knowledge, this is the first work to study human-like learning to uncertain check-ins. HISA introduces a multi-view collaborative mechanism to assess user behavior complexity from POI, intention, and type views—features unique to this scenario—thus improving recommendation performance.
2. Although simple, our framework is highly effective. Compared to SOTA methods, HISA provides both innovation and better performance. It is encoder-agnostic and easily integrates into various models, showing strong adaptability.
4. Although curriculum learning (CL) has been explored in recommendation system, its use for next POI recommendation under uncertain check-ins remains limited. Unlike traditional CL without feedback or review, HISA introduces human-inspired “acquire, review, and examine” stages, guided by a multi-view difficulty estimator to better handle uncertainty.
4. The proposed HISA framework is comprehensive and general, and it can be seamlessly incorporated into existing POI recommendations.

**W2:** 
We added two recent baselines: (1) DCHL (SIGIR 2024) and (2) SNPM (AAAI 2023), as mentioned by Reviewer nSJc. Due to time limits, AGRAN (2023) cannot be finished. As shown in Table 1, HISA outperforms these methods.
Table 1:
|Dataset|CAL||||PHO||||
|-|-|-|-|-|-|-|-|-|
|Metrics| HR@5| MRR@5|HR@10|MRR@10|HR@5|MRR@5|HR@10|MRR@10|
|DCHL|0.115|0.107|0.132|0.123|0.112|0.109|0.137|0.114|
|SNPM|0.129|0.111|0.155|0.132|0.128|0.106|0.159|0.119|

**W3:**
Following [52,32], we used CAL, CHA, and PHO, which are suitable for uncertain check-ins. To demonstrate scalability, we also additionally tested on a larger dataset, SIN (2,676 users, 3,440 POIs, 116,757 check-ins). As shown in Table 2, HISA improves HR by 15.1% and MRR by 9.2%.
Table 2:
|Metrics|HR@5|MRR@5|HR@10|MRR@10|
|-|-|-|-|-|
|HISA|0.122|0.107|0.136|0.112|
|w/o HISA|0.109|0.092|0.115|0.106|

**Q1:**
As shown in Table 3, which includes the results of STSP, HISA achieves average improvements of 58.5% in HR and 91.8% in MRR compared to it.

Table 3:
|Dataset|HR@5|MMR@5|HR@10|MRR@10|
|-|-|-|-|-|
|CAL|0.113|0.077|0.150|0.098|
|CHA|0.079|0.040|0.099|0.061|
|PHO|0.062|0.057|0.080|0.073|

**Q2:**
1. In Section 5.3, experiments on CAL and CHA show that (1) removing type or intention view degrades performance, and (2) removing HISA causes a notable drop, highlighting its effectiveness.
2. Section 5.4, experiments on CAL and CHA show results consistent with PHO. Across all three datasets, HISA achieves average gains of 10.24%, 13.15%, and 7.31% on RNN, LSTM, and GRU, respectively, demonstrating strong generalizability.


## Reviewer 5
Thank you for your valuable comments and suggestions.  
**Response to  Weaknesses:**
First, for the uncertain check-in problem, we are the first to introduce the concept of human-like learning, which simulates the *acquire*, *review*, and *examine* stages in the human knowledge acquisition process. Moreover, experimental results demonstrate that our proposed HISA outperforms existing methods. In addition, we have verified that the framework is encoder-agnostic, showing strong generality and flexibility. Therefore, we believe HISA serves as a general learning framework, particularly effective for POI recommendation tasks. Nevertheless, in future work, we plan to extend this learning framework to other recommendation scenarios in order to validate its generalization ability across different recommendation tasks. We will emphasize this in the future manuscript.

Second, in Eq. (4), *e* is a learnable user embedding from an embedding layer, jointly trained with other model parameters. In Eq. (5), the weight and bias are not shared—each view (POI, intention, type) has its own parameters. We will highlight this in the future paper.

Finally, while our method involves querying a difficulty estimator, its objective and learning mechanism are fundamentally different from those of model-based offline RL. Specifically, offline RL focuses on learning policies to maximize long-term rewards using environment modeling. In contrast, our HISA follows a human-inspired learning paradigm—*acquire*, *review*, and *examine*—which optimizes the learning paradigm by progressively organizing training data based on multi-view difficulty estimation without modeling user actions, environments, or rewards. Thus, it does not require components like policy networks, reward functions, and environment modeling. That said, we believe that integrating reinforcement learning mechanisms into the human-inspired *acquire–review–examine* process could further enhance the adaptive learning capabilities of HISA, making it a promising direction for future exploration.
