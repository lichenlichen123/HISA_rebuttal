# Rebuttal for HISA
## Reviewer 1
Thank you for your valuable comments and suggestions.  

**Response to Weakness \#1:** 
We have added a larger dataset including 2,676 users, 3,440 POIs, and 116,757 check-ins. As shown in Table 1, incorporating HISA yields a 15.1% improvement in HR and 9.2% in MRR compared to without HISA. 

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

**Response to Weakness \#3:** We have now included the training time of HISA, as shown in Table 1. Compared to the recent baseline DCHL [1] from SIGIR 2024, as mentioned by *Reviewer 4Sah*, our approach achieves comparable training time.

Table 1: Comparisons of Training Time (Second/Minute [s/m]).  
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

**Response to Weakness \#4:** The results in Table 2 correspond to the purple curve in Fig. 7. The HR@10 value for the CHA method should be 0.159 instead of 0.153; this typo will be corrected in the revised version. The stopping criterion is based on the convergence of the validation loss. Training is run for up to 100 epochs and stops early if the validation loss does not improve for 10 consecutive epochs. Each experiment is repeated five times under the same settings, and the average results are reported. All reported improvements over baseline methods are statistically significant with p-value < 0.01.


## Reviewer 2
Thank you for your valuable feedback and suggestions.  
**Response to Weakness \#1:**
The *review* and *examine* modules have different focuses and functions.
Specifically, the *review* module enhances model performance during training by re-learning learning units stored in the revisiting instance memory (RIM), following the principle of "reviewing the old to learn the new." In addition, the *examine* module identifies units that require re-learning—such as incorrectly predicted ones—during training and updates them in RIM for subsequent review. In summary, the *review* phase focuses on re-learning previously acquired knowledge, while the *examine* phase determines which units should be reviewed. We will highlight these differences in the future manuscript.

**Response to Weakness \#2:**
Our Cosine Similarity-Based Difficulty Estimator can handle long-tail or clustered distributions of POI embeddings. Specifically, we use a Gaussian kernel (Eq. 10 of our manuscript) as a scheduling strategy, which serves as a density-based method that implicitly captures the difficulty distribution. This kernel encourages the model to prioritize “typical” samples that are more concentrated and closer to the central distribution—similar to clustering—rather than those in the long-tail regions. As a result, it helps mitigate the effect of long-tail distributions, especially in the early stages of training. We will highlight this point in the future manuscript.

**Response to Weakness \#3:**
The three weight parameters in Equations 9 and 15 are empirically preset. They are selected from the range [0.1, 0.8], under the constraint that their sum equals 1. We tested various combinations and chose the ones that yielded the best performance. Specifically, the weights for POI, intention, and type are set to (0.4, 0.2, 0.4), (0.5, 0.3, 0.2), and (0.3, 0.3, 0.4) for the CAL, CHA, and PHO datasets, respectively.

**Response to Weakness \#4:**
We followed prior works [1][2] and selected the CAL, CHA, and PHO datasets, which are specifically constructed for the uncertain check-in scenario. In contrast, commonly used datasets such as NYC, TKY, and CA do not contain the necessary features required for modeling uncertain check-ins. To further demonstrate the scalability and effectiveness of our method, we additionally conducted experiments on SIN, a relatively larger dataset that includes 2,676 users, 3,440 POIs, and 116,757 check-in records. As shown in Table 1, incorporating HISA yields a 15.1% improvement in HR and a 9.2% improvement in MRR compared to the version without HISA. 

Moreover, each experiment is repeated five times under the same settings, and the average results are reported to ensure the reliability and robustness of the results. All reported improvements over baseline methods are statistically significant with p-value < 0.01.

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
To clarify, uncertain check-ins refer to situations where a user enters a collective POI (e.g., a shopping mall), but due to GPS inaccuracy or privacy protection, it is unclear which specific individual POI (e.g., a store inside the mall) the user actually visited. For illustration, a visual example is provided in Figure 1, and this concept is also introduced in Section 1 (Page 1) of our manuscript: *"In real-world scenarios, user visits are often uncertain and ambiguous, largely due to inaccuracies in GPS devices and privacy concerns. For instance, when a user enters a shopping mall (i.e., collective POI) containing numerous individual POIs, recommender systems (RSs) struggle to accurately determine which specific locations the user has visited. This challenge gives rise to the problem of uncertain check-ins."* Moreover, this concept has been formally discussed in prior works~[1][2], which we follow in our setting.

*References:  
[1] An interactive multi-task learning framework for next poi recommendation with uncertain check-ins, in: IJCAI.  
[2] Point-of-interest recommendation for users-businesses with uncertain check-ins. TKDE.*

**Response to Weakness \#2:**
If a simple instance and a difficult instance are equidistant from the threshold *σ*, they will receive the same weight *ρ*. However, the advantage of the Gaussian kernel lies in its ability to model not only the difficulty of each instance but also the density distribution of difficulty levels. Specifically, the model tends to focus on typical instances that are densely distributed around *σ* rather than instances located in the long-tail region. This mechanism helps prevent the early-stage training process from being influenced by long-tail samples, thereby improving stability during the initial learning phase. Therefore, the prioritization of training instances is determined by both difficulty values and the overall shape of the difficulty distribution, leading to a more stable and gradual learning process. We will highlight this point in the future manuscript.

**Response to Weakness \#3:**
We have added two recent baselines: (1) SNPM (2023 AAAI); however, due to the limited time, STHGCN could not be completed, but we added another recent baseline, (2) DCHL (2024 SIGIR), which was mentioned by Reviewer 4Sah. The results shown in Table 1 demonstrate the advantages of HISA.

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
Thank you for your reminder.  
First, regarding the *HISA w/o ex* variant: since the primary function of *examine* in HISA detects incorrectly predicted learning units (IPUs), the results for the *HISA w/o ex* are identical to those for *HISA w/o IPU* in Figure 7. This is clarified in Section 5.5 (Page 7) of our manuscript: "*HISA w/o IPU and HISA w/o ex produce identical results and are reported once"*.  
Second, regarding the *w/o HISA variant*, *w/o HISA* means the removal of the three stages proposed in this paper *(acquire, review, examine)*, which results in using the loss function from the original loss fucntion (i.e., Equation 7) to replace the loss functions from *acquire* (i.e., Equation 11) and *review* (i.e., Equation 13).  
Lastly, regarding HLU performance: from Figure 7, we observe dataset-specific differences. For example, in the CAL and CHA datasets, HISA’s performance drops slightly when HLU is removed, while in PHO, removing HLU has no significant effect. There may be two reasons. First, the complexity of different datasets varies. For example, the PHO dataset has slightly more uncertain check-ins than others, which may lead to HLU containing more of the users' uncertain and complex behaviors, negatively impacting the model. Second, the method used to obtain HLU is a simple threshold-based approach, which may introduce noise, such as sparse trajectories, affecting model learning. This motivates us to improve strategies for selecting HLU in the future.


## Reviewer 4
Thank you for your valuable feedback. 
**Response to  Comment \#1:** 
First, to our knowledge, this is the first work to leverage human-like learning paradigms for next POI recommendation with uncertain check-ins. It apply a human-inspired learning method to POI recommendation tasks, especially in addressing the uncertain check-ins problem. To tackle the uncertain check-ins issue, HISA introduces a multi-view collaborative mechanism, which comprehensively evaluates the complexity of user behavior across different perspectives, including POI, intention, and type views, helping us improve performance in this scenarios.

Second, while curriculum learning (CL) has been studied in recommendation systems, its application in next POI recommendation, particularly in uncertain check-in scenarios, remains quite limited. In contrast to traditional CL methods, we design a multi-view difficulty estimator specifically for uncertain check-ins and introduce the *acquire*, *review* and *examine* phases, inspired by human learning paradigms, to effectively addresses uncertain POI recommendations. 

Third, compared to existing baselines, the HISA framework not only offers methodological innovation but also demonstrates superior performance. Furthermore, we have experimentally verified that the proposed HISA framework is encoder-agnostic and can be applied to existing recommendation models, offering exceptional adaptability and flexibility.

Finally, the human-like learning paradigms we propose are general and can be seamlessly integrated into existing POI recommendations.

**Response to  Comment \#2:** 
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

**Response to  Comment \#3:** 
We followed prior works [1][2] and selected the CAL, CHA, and PHO datasets, which are specifically constructed for the uncertain check-in scenario. In contrast, commonly used datasets such as NYC, TKY, and CA do not contain the necessary features required for modeling uncertain check-ins. To further demonstrate the scalability and effectiveness of our method, we additionally conducted experiments on SIN, a relatively larger dataset that includes 2,676 users, 3,440 POIs, and 116,757 check-in records. As shown in Table 1, incorporating HISA yields a 15.1% improvement in HR and a 9.2% improvement in MRR compared to the version without HISA. 

<table>
  <caption>Table 1: Performance of HISA on SIN dataset.</caption>
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
Thank you for the suggestion. Due to space limitations, we only presented results on the PHO dataset in Sections 5.3 and 5.4. And we will include these results in the appendix or revised version.

we have conducted experiments on the other two datasets as well. In Section 5.3, we present results on these datasets and have drawn similar conclusions. Regarding Section 5.4, we clearly observe that, like the PHO dataset, the other two datasets also achieve encoder-independent improvements, with RNN, LSTM, and GRU benefiting from HISA in terms of performance.

## Reviewer 5
**Response to  Comment:**  Thank you for your valuable comment.  
First, regarding the term ''novel learning paradigm,'' we acknowledge that our current evaluation focuses on next POI recommendation with uncertain check-ins. This is because we chose this challenging setting to demonstrate the effectiveness of our approach in high-noise, sparse-feedback scenarios where traditional training methods often fail. Furthermore, our goal is to highlight the proposed training mechanism—*acquire*, *review*, and *examine*—which is inspired by human learning. Moreover, experiments show that this mechanism is encoder-agnostic and can potentially generalize to other sequential tasks. We also plan to explore its broader applicability in future work.

Second, in Eq. (4), $\mathbf{e}_u$ is a learnable user embedding from an embedding layer, jointly trained with other model parameters. In Eq. (5), the weight and bias are not shared—each view (POI, intention, type) has its own decoder parameters. We will clarify this in the revised paper.

Finally, while our method involves querying a difficulty estimator, its objective and learning mechanism are fundamentally different from those of model-based offline RL. Offline RL focuses on learning policies to maximize long-term rewards using environment modeling. In contrast, our approach follows a human-inspired learning paradigm—*acquire*, *review*, and *examine*—which optimizes the learning paradigm by progressively organizing training data based on multi-view difficulty estimation without modeling user actions, environments, or rewards. Thus, it does not require components like policy networks, reward functions, and environment modeling. That said, we believe that integrating reinforcement learning mechanisms into the human-inspired *acquire–review–examine* process could further enhance the adaptive learning capabilities of HISA, making it a promising direction for future exploration.
