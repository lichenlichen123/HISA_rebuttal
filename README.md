# Rebuttal for HISA
## Reviewer 1
Thank you for your valuable comments and suggestions.  

**Response to Weakness \#1:** 
We have included a larger dataset including 2,676 users, 3,440 POIs, and 116,757 check-ins. As shown in Table 1, incorporating HISA yields a 15.1% improvement in HR and 9.2% in MRR compared to without HISA. 

<table>
  <caption>Table 1: Performance of HISA on the SIN dataset.</caption>
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

**Response to Weakness \#2:** We will replace the exponential base with *exp* in Eq. (10) and check for similar issues throughout the paper.

**Response to Weakness \#3:** We have now included the training time of HISA, as shown in Table 2. Compared to the recent baseline DCHL [1] from SIGIR 2024, as mentioned by *Reviewer 4Sah*, our approach achieves comparable training efficiency.

Table 2: Comparisons of Training Time (Second/Minute [s/m]).  
"Training per epoch" is the average training time for each epoch; "Training" denotes the total training time to converge.

| Datasets | Time               | HISA w/o re_ex | HISA  | DCHL  |
|----------|--------------------|----------------|-------|-------|
| **CAL**  | Training per epoch  | 6.2s          | 7.8s  | 7.9s  |
|          | Training            | 5m             | 7m    | 9m    |
| **CHA**  | Training per epoch  | 9.8s          | 13.1s | 16.6s |
|          | Training            | 9m             | 13m   | 15m   |
| **PHO**  | Training per epoch  | 18.5s         | 21.1s | 24.8s |
|          | Training            | 15m            | 19m   | 22m   |


*References:  
[1] Disentangled contrastive hypergraph learning for next POI recommendation, 2024, in: SIGIR.*

**Response to Weakness \#4:** The results in Table 2 correspond to the purple curve in Fig. 7. The HR@10 value for the CHA method should be 0.159 rather than 0.151; this typo will be corrected in the revised version. The stopping criterion is based on the convergence of the validation loss. The training process is run for up to 100 epochs and is stopped early if the validation loss does not improve for 10 consecutive epochs. Each experiment is repeated five times under the same settings, and the average results are reported. All reported improvements over baseline methods are statistically significant with p-value < 0.01.


## Reviewer 2
Thank you for your valuable feedback and suggestions.  
**Response to Weakness \#1:**
The *review* and *examine* modules have different focuses and functions.
Specifically, the *review* module enhances model performance during training by re-learning learning units stored in the revisiting instance memory (RIM), following the principle of "reviewing the old to learn the new." In addition, the *examine* module identifies units that require re-learning—such as incorrectly predicted ones—during training and updates them in RIM for subsequent review. In summary, the *review* phase focuses on re-learning previously acquired knowledge, while the *examine* phase determines which units should be reviewed. We will highlight these differences in the future manuscript.

**Response to Weakness \#2:**
Our Cosine Similarity-Based Difficulty Estimator can handle long-tail or clustered distributions of POI embeddings. Specifically, we use a Gaussian kernel (Eq. 10 of our manuscript) as a scheduling strategy, which serves as a density-based method that implicitly captures the difficulty distribution. This kernel guides the model to prioritize “typical” samples that are densely distributed and closer to the central tendency—similar to clustering—while deemphasizing those in the long-tail regions. As a result, it helps mitigate the effect of long-tail distributions, especially in the early stages of training. We will highlight this point in the future manuscript.

**Response to Weakness \#3:**
The three weight parameters in Equations 9 and 15 are empirically preset. They are selected from the range [0.1, 0.8], subject to the constraint that their sum equals 1. We tested various combinations and chose the ones that yielded the best performance. Specifically, the weights for POI, intention, and type are set to (0.4, 0.2, 0.4), (0.5, 0.3, 0.2), and (0.3, 0.3, 0.4) for the CAL, CHA, and PHO datasets, respectively.

**Response to Weakness \#4:**
We followed prior works [1][2] and selected the CAL, CHA, and PHO datasets, which are specifically designed for modeling uncertain check-in scenarios. In contrast, commonly used datasets such as NYC, TKY, and CA lack the necessary features for modeling uncertain check-ins. To further demonstrate the scalability and effectiveness of our method, we conducted experiments on SIN, a relatively larger dataset that includes 2,676 users, 3,440 POIs, and 116,757 check-in records. As shown in Table 1, incorporating HISA yields a 15.1% improvement in HR and a 9.2% improvement in MRR compared to the model without HISA.

Moreover, each experiment is repeated five times under the same settings, and the average values are reported to ensure result reliability and robustness. All reported improvements over baseline methods are statistically significant with p-value < 0.01.

<table>
  <caption>Table 1: Performance of HISA on the SIN dataset.</caption>
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
*[1] An interactive multi-task learning framework for next poi recommendation with uncertain check-ins, in: IJCAI.*  
*[2] Point-of-interest recommendation for users-businesses with uncertain check-ins. TKDE.*

## Reviewer 3
Thank you for your valuable comment.  
**Response to Weakness \#1:**
To clarify, uncertain check-ins refer to scenarios where a user enters a collective POI (e.g., a shopping mall), but due to GPS inaccuracy or privacy protection, it remains unclear which specific individual POI (e.g., a store inside the mall) the user actually visited. For illustration, a visual example is provided in Figure 1, and this concept is also introduced in Section 1 (Page 1) of our manuscript: *"In real-world scenarios, user visits are often uncertain and ambiguous, largely due to inaccuracies in GPS devices and privacy concerns. For instance, when a user enters a shopping mall (i.e., collective POI) containing numerous individual POIs, recommender systems (RSs) struggle to accurately determine which specific locations the user has visited. This challenge gives rise to the problem of uncertain check-ins."* Moreover, this concept has been formally discussed in prior works~[1][2], which we follow in our setting.

*References:  
[1] An interactive multi-task learning framework for next poi recommendation with uncertain check-ins, in: IJCAI.
[2] Point-of-interest recommendation for users-businesses with uncertain check-ins. TKDE.*

**Response to Weakness \#2:**
If a simple instance and a difficult instance are equidistant from the threshold *σ*, they will receive the same weight *ρ*. However, the advantage of the Gaussian kernel lies in its ability to model both the individual difficulty of each instance and the overall density distribution of difficulty levels. Specifically, the model tends to focus on typical instances that are densely distributed around *σ* rather than instances located in the long-tail region. This mechanism reduces the influence of long-tail samples in the early training phase, thereby enhancing model stability. Therefore, the prioritization of training instances is determined by both difficulty values and the overall shape of the difficulty distribution, leading to a more stable and gradual learning process. We will highlight this point in the future manuscript.

**Response to Weakness \#3:**
We have added two recent baselines: (1) SNPM (2023 AAAI); however, due to the limited time, STHGCN could not be completed, but we added another recent baseline, (2) DCHL (2024 SIGIR), which was mentioned by *Reviewer 4Sah*. The results shown in Table 1 demonstrate the advantages of HISA.

<table>
  <caption>Table 1: Comparison of model performance with recent baselines.</caption>
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


**Response to Weakness \#4:**
First, regarding the *HISA w/o ex* variant: since the primary function of *examine* in HISA detects incorrectly predicted learning units (IPUs), the results for *HISA w/o ex* are identical to those for *HISA w/o IPU* in Figure 7. This is clarified in Section 5.5 (Page 7) of our manuscript: "*HISA w/o IPU and HISA w/o ex produce identical results and are reported once"*.  
Second, regarding the *w/o HISA variant*, *w/o HISA* means the removal of the three stages proposed in this paper *(acquire, review, examine)*, which means using the original loss function (i.e., Eq. 7) to replace the loss functions from *acquire* (i.e., Eq. 11) and *review* (i.e., Eq. 13).  
Lastly, regarding HLU performance: from Fig. 7, we observe dataset-specific differences. In the CAL and CHA datasets, HISA’s performance drops slightly when HLU is removed, while in PHO, removing HLU has no significant effect. We believe this may be due to two potential reasons. (1) The complexity of different datasets varies. For example, the PHO dataset has slightly more uncertain check-ins than others, which may lead to HLU containing more of the users' uncertain and complex behaviors, which may negatively affect the model’s ability to capture user preferences. (2) The method used to obtain HLU is a simple threshold-based approach, which may introduce noise, such as sparse trajectories, affecting model learning. This observation motivates future work to develop more effective strategies for selecting HLU.

## Reviewer 4
Thank you for your valuable feedback.  
**Response to  Weakness \#1:** 
First, to the best of our knowledge, this is the first work to leverage human-like learning paradigms for a novel research problem—uncertain check-ins. To address this challenge, HISA introduces a multi-view collaborative mechanism that comprehensively evaluates the complexity of user behavior from different perspectives, including POI, intention, and type views—features that are unique to uncertain check-in scenarios—thereby improving recommendation performance in such contexts.

Second, although our self-adaptive framework is relatively simple, it is highly effective. Compared to existing baselines, the HISA framework not only brings methodological innovation but also achieves superior performance. Additionally, we empirically demonstrate that HISA is encoder-agnostic and can be easily integrated into various recommendation models, showing strong adaptability and flexibility.

Third, although curriculum learning (CL) has been explored in recommendation systems, its application to next POI recommendation under uncertain check-in scenarios remains largely underexplored. Moreover, unlike traditional CL, which lacks feedback and review during learning, we propose HISA—a multi-view difficulty estimator specifically designed for uncertain check-ins—which incorporates the *acquire*, *review*, and *examine* stages inspired by human learning paradigms, effectively addressing the uncertainty in POI recommendations.

Finally, the proposed HISA framework is comprehensive and general, and it can be seamlessly incorporated into existing POI recommendations.


**Response to  Weakness \#2:** 
We have added two recent baselines: (1) DCHL (2024 SIGIR); however, due to the limited time, AGRAN could not be completed, but we added another recent baseline, (2) SNPM (2023 AAAI), which was mentioned by Reviewer nSJc. The results shown in Table 1 demonstrate the advantages of HISA.

<table>
  <caption>Table 1: Comparison of model performance with recent baselines.</caption>
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

**Response to  Weakness \#3:** 
We followed prior works [1][2] and selected the CAL, CHA, and PHO datasets, which are specifically constructed for the uncertain check-ins. To further demonstrate the scalability of HISA, we conducted experiments on SIN, a relatively larger dataset that includes 2,676 users, 3,440 POIs, and 116,757 check-in records. As shown in Table 2, incorporating HISA yields a 15.1% improvement in HR and a 9.2% improvement in MRR compared to the model without HISA.

<table>
  <caption>Table 2: Performance of HISA on SIN dataset.</caption>
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
*[1] An interactive multi-task learning framework for next poi recommendation with uncertain check-ins, in: IJCAI.*  
*[2] Point-of-interest recommendation for users-businesses with uncertain check-ins. TKDE.*

**Response to  Q \#1:** 
We have included the relevant comparative results for STSP in Table 1.  
Table 1: Model performance of STSP.
| Dataset  | HR@5   | MMR@5  | HR@10  | MRR@10 |
|----------|--------|--------|--------|--------|
| **CAL**  | 0.113  | 0.077  | 0.150  | 0.098  |
| **CHA**  | 0.079  | 0.040  | 0.099  | 0.061  |
| **PHO**  | 0.062  | 0.057  | 0.080  | 0.073  |

**Response to  Q \#2:** 
We have finished experiments on the CAL and CHA datasets for Sections 5.3 and 5.4. Specifically:

(1) In Section 5.3, consistent performance drops were observed on CAL and CHA when either the "type" or "intention" view was removed. This indicates that POI-level representation alone is insufficient, while type and intention views provide complementary discriminative signals for the difficulty estimator. These findings suggest that POI type and user intentions are essential for modeling uncertain check-ins. Moreover, the removal of HISA led to a significant decline in performance, further highlighting the effectiveness of this human-inspired learning framework in addressing uncertainty. Therefore, these results show that HISA effectively tackles the challenges in uncertain check-ins.

(2) In Section 5.4, we further verified on CAL and CHA that HISA is encoder-agnostic. We also performed an overall analysis across all three datasets, showing that HISA achieves average performance improvements of 10.24%, 13.15%, and 7.31% on RNN, LSTM, and GRU, respectively, demonstrating strong generalizability.



## Reviewer 5
Thank you for your valuable comments and suggestions.  
**Response to  Comment:**
First, for the uncertain check-in problem, we are the first to introduce the concept of human-like learning, which simulates the *acquire*, *review*, and *examine* stages in the human knowledge acquisition process. Moreover, experimental results demonstrate that our proposed HISA outperforms existing methods. In addition, we have verified that the framework is encoder-agnostic, showing strong generality and flexibility. Therefore, we believe HISA serves as a general learning framework, particularly effective for POI recommendation tasks. Nevertheless, in future work, we plan to extend this learning framework to other recommendation scenarios in order to validate its generalization ability across different recommendation tasks. We will emphasize this in the future manuscript.

Second, in Eq. (4), $\mathbf{e}_u$ is a learnable user embedding from an embedding layer, jointly trained with other model parameters. In Eq. (5), the weight and bias are not shared—each view (POI, intention, type) has its own parameters. We will highlight this in the future paper.

Finally, while our method involves querying a difficulty estimator, its objective and learning mechanism are fundamentally different from those of model-based offline RL. Specifically, offline RL focuses on learning policies to maximize long-term rewards using environment modeling. In contrast, our HISA follows a human-inspired learning paradigm—*acquire*, *review*, and *examine*—which optimizes the learning paradigm by progressively organizing training data based on multi-view difficulty estimation without modeling user actions, environments, or rewards. Thus, it does not require components like policy networks, reward functions, and environment modeling. That said, we believe that integrating reinforcement learning mechanisms into the human-inspired *acquire–review–examine* process could further enhance the adaptive learning capabilities of HISA, making it a promising direction for future exploration.
