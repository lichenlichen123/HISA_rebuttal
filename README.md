# Rebuttal for HISA
## Reviewer 1
**Response to  Comment \#1:**
Thank you for your valuable question. The stopping criterion used for the results in Table 2 of our manuscript is based on the convergence of the model's validation loss. Specifically, the total number of epochs is set to 100, and training is halted if the validation loss does not decrease over the course of 10 consecutive epochs. Additionally, to ensure a robust evaluation, we repeat each experiment five times using the same setup for each method and report the average results.

**Response to  Comment \#2:**
Thank you for your insightful comment. We have now included the training time of HISA, as well as the additional training time incurred from the review and examine stages. It is worth noting that the extra time for the review and examine stages is related to the triggering conditions for the review, which is proportional to the number of epochs. If review and examine are performed in the later stages of training, the corresponding time will increase. Nevertheless, when compared to the current state-of-the-art methods, particularly those using graph-based approaches, such as the 2024 method DCHL [1], we still have an advantage in terms of time efficiency. The results are shown in Table 1.

**Comparisons of Training Time (Second/Minute [s/m]).**  
"Training per Epoch" is the average training time for each epoch; "Training" denotes the total training time to converge.
**Table 1: Comparisons of Training and Testing Time.**

| Datasets | Time               | HISA w/o re_ex | HISA  | DCHL  |
|----------|--------------------|----------------|-------|-------|
| **CAL**  | Training per epoch  | 6.2s          | 7.8s  | 7.9s  |
|          | Training            | 5m             | 7m    | 9m    |
| **CHA**  | Training per epoch  | 9.8s          | 13.1s | 16.6s |
|          | Training            | 9m             | 13m   | 15m   |
| **PHO**  | Training per epoch  | 18.5s         | 21.1s | 24.8s |
|          | Training            | 15m            | 19m   | 22m   |


*References:  
[1] Lai, Yantong and Su, Yijun and Wei, Lingwei and He, Tianqi and Wang, Haitao and Chen, Gaode and Zha, Daren and Liu, Qiang and Wang, Xingxing. 2024. Disentangled contrastive hypergraph learning for next POI recommendation, in: SIGIR, pp. 1452--1462.*

## Reviewer 2

**Response to  Comment \#1:**
Thank you for your valuable feedback. The *review* and *examine* modules have different focuses and functions. The detailed explanation is as follows.

First, the *review*  module improves the model's performance during training by re-learning the learning units in the revisiting instance memory (RIM), akin to the principle of *reviewing the old to learn the new.*  Second, the *examine* module, on the other hand, identifies incorrectly predicted learning units (IPUs) during training and updates them in RIM for re-learning in the *review* phase. In summary, the *review* stage involves re-learning previously learned knowledge, while the *examine* stage identifies which units need re-learning. We will clarify these differences further in the revised manuscript.

**Response to  Comment \#2:**
Thank you for your valuable feedback. We agree that POI embeddings may exhibit long-tail or clustered distributions. In fact, in our difficulty estimator, we use a Gaussian kernel (Equation 10 in our manuscript), which is a density-based estimator that implicitly captures the characteristics of the difficulty distribution. By applying this kernel function, the model prioritizes "typical samples" that are more concentrated and closer to the central distribution, similar to the concept of clustering, rather than samples located in the long-tail region. This helps mitigate the impact of long-tail distributions during the early stages of training. Therefore, this approach also accounts for the effects of clustering or long-tail distributions. We will provide further clarification on this point in the revised manuscript.

**Response to  Comment \#3:**
Thank you for your insightful feedback. The three weight parameters in Equations 9 and 15 are preset through empirical tuning. Specifically, these weights are chosen within the range of [0.1, 0.8], subject to the constraint that their sum equals 1. We performed extensive experiments across various weight combinations to identify the set that yielded optimal model performance.

**Response to  Comment \#4:**
Thank you for your feedback. We followed previous work [1][2] and selected datasets (CAL, CHA, and PHO) that are specifically constructed for the uncertain check-in scenario. Additionally, to further demonstrate the scalability and effectiveness of our method, we additionally conducted experiments on SIN, a large-scale dataset, including 2,676 users, 3,440 POIs, and 116,757 check-in records. Results show that incorporating HISA leads to an improvement of 15.1\% in HR and 9.2\% in MRR over the baseline without HISA, as presented in Table 2. And all experiments were repeated five times, and we report the average performance to ensure the reliability and robustness of the results.

<table>
  <caption>Table 2: Model performance of HISA on SIN dataset with recent baselines.</caption>
  <tr>
    <th>Dataset</th>
    <th colspan="4">SIN</th>
  </tr>
  <tr>
    <th>Metrics</th>
    <th>HR@5</th>
    <th>MRR@5</th>
    <th>HR@10</th>
    <th>MRR@10</th>
  </tr>
  <tr>
    <td>HISA</td>
    <td>0.122</td>
    <td>0.107</td>
    <td>0.136</td>
    <td>0.112</td>
  </tr>
  <tr>
    <td>w/o HISA</td>
    <td>0.109</td>
    <td>0.092</td>
    <td>0.115</td>
   <td>0.106</td>
  </tr>
</table>
				
*References:*  
*[1] Zhang, Lu and Sun, Zhu and Zhang, Jie and Lei, Yu and Li, Chen and Wu, Ziqing and Kloeden, Horst and Klanner, Felix. 2021. An interactive multi-task learning framework for next poi recommendation with uncertain check-ins, in: IJCAI, pp. 3551–3557.*  
*[2] Sun, Zhu and Li, Chen and Lei, Yu and Zhang, Lu and Zhang, Jie and Liang, Shunpan. 2021. Point-of-interest recommendation for users-businesses with uncertain check-ins. TKDE 34, 5925–5938.*

## Reviewer 3
**Response to  Comment \#1:**
Thank you for your valuable comment. To clarify, uncertain check-ins refer to situations where a user enters a collective POI (e.g., a shopping mall), but due to GPS inaccuracy or privacy protection, it is unclear which specific individual POI (e.g., a store inside the mall) the user actually visited. For illustration, a visual example is provided in Figure 1, and this concept is also introduced in Section 1 (Page 1) of our manuscript: *"In real-world scenarios, user visits are often uncertain and ambiguous, largely due to inaccuracies in GPS devices and privacy concerns. For instance, when a user enters a shopping mall (i.e., collective POI) containing numerous individual POIs, recommender systems (RSs) struggle to accurately determine which specific locations the user has visited. This challenge gives rise to the problem of uncertain check-ins."* Moreover, this concept has been formally discussed in prior works~[1][2], which we follow in our setting.

*References:  
[1] Zhang, Lu and Sun, Zhu and Zhang, Jie and Lei, Yu and Li, Chen and Wu, Ziqing and Kloeden, Horst and Klanner, Felix. 2021. An interactive multi-task learning framework for next poi recommendation with uncertain check-ins, in: IJCAI, pp. 3551–3557.  
[2] Sun, Zhu and Li, Chen and Lei, Yu and Zhang, Lu and Zhang, Jie and Liang, Shunpan. 2021. Point-of-interest recommendation for users-businesses with uncertain check-ins. TKDE 34, 5925–5938.*

**Response to  Comment \#2:**
Thank you for your insightful question. If a simple instance and a difficult instance are equidistant from the threshold $\sigma$, they will receive the same weight $\rho$. However, the advantage of the Gaussian kernel lies in its ability to model not only the difficulty of each instance but also the density distribution of difficulty levels. Specifically, the model tends to focus on typical instances that are densely distributed around $\sigma$ rather than instances located in the long-tail region. This mechanism helps prevent the early-stage training process from being influenced by long-tail samples, thereby improving stability during the initial learning phase. Therefore, the prioritization of training instances is determined by both difficulty values and the overall shape of the difficulty distribution, leading to a more stable and gradual learning process. A detailed explanation of this mechanism will be added in Section 4.1.2 of the revised paper.

**Response to  Comment \#3:**
We have added two recent baselines, namely SNPM [1] (2023) and DCHL [2] (2024), with results shown in Table 3.

<table>
  <caption>Table 3: Comparison of model performance with recent baselines.</caption>
  <tr>
    <th>Dataset</th>
    <th colspan="4">CAL</th>
    <th colspan="4">PHO</th>
  </tr>
  <tr>
    <th>Metrics</th>
    <th>HR@5</th>
    <th>MRR@5</th>
    <th>HR@10</th>
    <th>MRR@10</th>
    <th>HR@5</th>
    <th>MRR@5</th>
    <th>HR@10</th>
    <th>MRR@10</th>
  </tr>
  <tr>
    <td>DCHL</td>
    <td>0.115</td>
    <td>0.107</td>
    <td>0.132</td>
    <td>0.123</td>
    <td>0.112</td>
    <td>0.109</td>
    <td>0.137</td>
    <td>0.114</td>
  </tr>
  <tr>
    <td>SNPM</td>
    <td>0.129</td>
    <td>0.111</td>
    <td>0.155</td>
    <td>0.132</td>
    <td>0.128</td>
    <td>0.106</td>
    <td>0.159</td>
   <td>0.119</td>
  </tr>
</table>

*References:  
[1] Yin, Feiyu and Liu, Yong and Shen, Zhiqi and Chen, Lisi and Shang, Shuo and Han, Peng. 2023. Next POI recommendation with dynamic graph and explicit dependency, in: AAAI, pp. 4827--4834.  
[2] Lai, Yantong and Su, Yijun and Wei, Lingwei and He, Tianqi and Wang, Haitao and Chen, Gaode and Zha, Daren and Liu, Qiang and Wang, Xingxing. 2024. Disentangled contrastive hypergraph learning for next POI recommendation, in: SIGIR, pp. 1452--1462.*

**Response to  Comment \#4:**
Thank you for your reminder.  
First, regarding the *HISA w/o ex* variant: since the primary function of *examine* in HISA detects incorrectly predicted learning units (IPUs), the results for the *HISA w/o ex* are identical to those for *HISA w/o IPU* in Figure 7. This is clarified in Section 5.5 (Page 7) of our manuscript: "*HISA w/o IPU and HISA w/o ex produce identical results and are reported once"*.  
Second, regarding the *w/o HISA variant*, *w/o HISA* means the removal of the three stages proposed in this paper *(acquire, review, examine)*, which results in using the loss function from the original loss fucntion (i.e., Equation 7) to replace the loss functions from *acquire* (i.e., Equation 11) and *review* (i.e., Equation 13).  
Lastly, regarding HLU performance: from Figure 7, we observe dataset-specific differences. For example, in the CAL and CHA datasets, HISA’s performance drops slightly when HLU is removed, while in PHO, removing HLU has no significant effect. This may be due to dataset-specific difficulty levels and our rule-based HLU selection method, which may introduce noise, affecting the model’s performance. Currently, we propose a novel human-inspired learning approach to address the complex, uncertain check-in problem. In the future, we aim to optimize this paradigm, including HLU selection to improve performance.


## Reviewer 4
**Response to  Comment \#1:** 
Thank you for your valuable feedback.  
First, the HISA framework we propose is specifically designed for the complex and sparse uncertain check-in scenarios. Its core innovation lies in applying human-inspired learning principles to POI recommendation tasks, especially in addressing the uncertain check-ins problem. To tackle the uncertain check-ins issue, HISA introduces a multi-view collaborative mechanism, which comprehensively evaluates the complexity of user behavior across different perspectives, including POI, intention, and type views. Notably, the POI type is a unique feature in uncertain check-ins, helping us improve performance in these scenarios.

Second, while curriculum learning (CL) has been studied in recommendation systems, its application in next POI recommendation, particularly in uncertain check-in scenarios, remains quite limited. In contrast to traditional CL methods, HISA draws on the three key stages of human learning—*acquire*, *review*, and *examine*—to construct a systematic and adaptive learning process that effectively addresses uncertain POI recommendations. Compared to existing baselines, the HISA framework not only offers methodological innovation but also demonstrates superior performance.

Finally, we have experimentally verified that the proposed HISA framework is encoder-agnostic and can be applied to existing recommendation models, offering exceptional adaptability and flexibility.

**Response to  Comment \#2:** 
We have added two recent baselines, namely SNPM [1] (2023) and DCHL [2] (2024), with results shown in Table 4.

<table>
  <caption>Table 4: Comparison of model performance with recent baselines.</caption>
  <tr>
    <th>Dataset</th>
    <th colspan="4">CAL</th>
    <th colspan="4">PHO</th>
  </tr>
  <tr>
    <th>Metrics</th>
    <th>HR@5</th>
    <th>MRR@5</th>
    <th>HR@10</th>
    <th>MRR@10</th>
    <th>HR@5</th>
    <th>MRR@5</th>
    <th>HR@10</th>
    <th>MRR@10</th>
  </tr>
  <tr>
    <td>DCHL</td>
    <td>0.115</td>
    <td>0.107</td>
    <td>0.132</td>
    <td>0.123</td>
    <td>0.112</td>
    <td>0.109</td>
    <td>0.137</td>
    <td>0.114</td>
  </tr>
  <tr>
    <td>SNPM</td>
    <td>0.129</td>
    <td>0.111</td>
    <td>0.155</td>
    <td>0.132</td>
    <td>0.128</td>
    <td>0.106</td>
    <td>0.159</td>
   <td>0.119</td>
  </tr>
</table>

*References:  
[1] Yin, Feiyu and Liu, Yong and Shen, Zhiqi and Chen, Lisi and Shang, Shuo and Han, Peng. 2023. Next POI recommendation with dynamic graph and explicit dependency, in: AAAI, pp. 4827--4834.  
[2] Lai, Yantong and Su, Yijun and Wei, Lingwei and He, Tianqi and Wang, Haitao and Chen, Gaode and Zha, Daren and Liu, Qiang and Wang, Xingxing. 2024. Disentangled contrastive hypergraph learning for next POI recommendation, in: SIGIR, pp. 1452--1462.*

**Response to  Comment \#3:** 
Thank you for your feedback. We followed previous work [1][2] and selected datasets (CAL, CHA, and PHO) that are specifically constructed for the uncertain check-in scenario. Additionally, to further demonstrate the scalability and effectiveness of our method, we additionally conducted experiments on SIN, a large-scale dataset, including 2,676 users, 3,440 POIs, and 116,757 check-in records. Results show that incorporating HISA leads to an improvement of 15.1\% in HR and 9.2\% in MRR over the baseline without HISA, as presented in Table 5. And all experiments were repeated five times, and we report the average performance to ensure the reliability and robustness of the results.

<table>
  <caption>Table 5: Model performance of HISA on SIN dataset with recent baselines.</caption>
  <tr>
    <th>Dataset</th>
    <th colspan="4">SIN</th>
  </tr>
  <tr>
    <th>Metrics</th>
    <th>HR@5</th>
    <th>MRR@5</th>
    <th>HR@10</th>
    <th>MRR@10</th>
  </tr>
  <tr>
    <td>HISA</td>
    <td>0.122</td>
    <td>0.107</td>
    <td>0.136</td>
    <td>0.112</td>
  </tr>
  <tr>
    <td>w/o HISA</td>
    <td>0.109</td>
    <td>0.092</td>
    <td>0.115</td>
   <td>0.106</td>
  </tr>
</table>
				
*References:*  
*[1] Zhang, Lu and Sun, Zhu and Zhang, Jie and Lei, Yu and Li, Chen and Wu, Ziqing and Kloeden, Horst and Klanner, Felix. 2021. An interactive multi-task learning framework for next poi recommendation with uncertain check-ins, in: IJCAI, pp. 3551–3557.*  
*[2] Sun, Zhu and Li, Chen and Lei, Yu and Zhang, Lu and Zhang, Jie and Liang, Shunpan. 2021. Point-of-interest recommendation for users-businesses with uncertain check-ins. TKDE 34, 5925–5938.*

**Response to  Q \#1:** 
Thank you for your feedback. Following your suggestion, we have included the relevant comparative results for STSP in Table 6 below.  

Table 6: Model performance of STSP.
| Dataset  | HR@5   | MMR@5  | HR@10  | MRR@10 |
|----------|--------|--------|--------|--------|
| **CAL**  | 0.113  | 0.077  | 0.150  | 0.098  |
| **CHA**  | 0.079  | 0.040  | 0.099  | 0.061  |
| **PHO**  | 0.062  | 0.057  | 0.080  | 0.073  |



**Response to  Q \#2:** 
Thank you for the suggestion. Due to space limitations, we only presented results on the PHO dataset in Sections 5.3 and 5.4. And we will include these results in the appendix or revised version.


## Reviewer 5
**Response to  Comment:**  Thank you for your valuable comment.  
First, regarding the term ''novel learning paradigm,'' we acknowledge that our current evaluation focuses on next POI recommendation with uncertain check-ins. This is because we chose this challenging setting to demonstrate the effectiveness of our approach in high-noise, sparse-feedback scenarios where traditional training methods often fail. Furthermore, our goal is to highlight the proposed training mechanism—*acquire*, *review*, and *examine*—which is inspired by human learning. Moreover, experiments show that this mechanism is encoder-agnostic and can potentially generalize to other sequential tasks. We also plan to explore its broader applicability in future work.

Second, in Eq. (4), $\mathbf{e}_u$ is a learnable user embedding from an embedding layer, jointly trained with other model parameters. In Eq. (5), the weight and bias are not shared—each view (POI, intention, type) has its own decoder parameters. We will clarify this in the revised paper.

Finally, while our method involves querying a difficulty estimator, its objective and learning mechanism are fundamentally different from those of model-based offline RL. Offline RL focuses on learning policies to maximize long-term rewards using environment modeling. In contrast, our approach follows a human-inspired learning paradigm—*acquire*, *review*, and *examine*—which optimizes the learning paradigm by progressively organizing training data based on multi-view difficulty estimation without modeling user actions, environments, or rewards. Thus, it does not require components like policy networks, reward functions, and environment modeling. That said, we believe that integrating reinforcement learning mechanisms into the human-inspired *acquire–review–examine* process could further enhance the adaptive learning capabilities of HISA, making it a promising direction for future exploration.



