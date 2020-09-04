---
title: Follow-up Thoughts on a Couple Previous Pulbications from Dr. Shuran Song Lab.
datePublished: September 3, 2020
abstract: I have read the papers "Spatial Action Maps for Mobile Manipulation" and "Learning Synergies between Pushing and Grasping with Self-supervised Deep Reinforcement Learning). I found the general approach having the policy represent a pixel-wise map of Q-values really interesting, and I have summarized a few of my follow-up thoughts below, with particular focus on the Push-Grasp paper.
imagePath: placeholder_thumbnail.png
---


I have read the papers "Spatial Action Maps for Mobile Manipulation" and "Learning Synergies between Pushing and Grasping with Self-supervised Deep Reinforcement Learning). I found the general approach having the policy represent a pixel-wise map of Q-values really interesting, and I have summarized a few of my follow-up thoughts below.

### 1) Distributional value approximations embedded into pixel-wise output map

Distributional value approximation [1, 2, 3] works great with deep reinforcement learning. I am wondering if these performance gains can be translated to learning pixel-wise value maps. If the added computational costs are too prohibitive (e.g. the output channel will now be multiple channels deep, each representing a single discretized unit of the distribution), how can we make it more efficient? 

Another reason for considering distributional value approximation is that, intuitively, it can introduce "risk sensitive" behavior, allowing agents to act conservatively when a certain state-action pair is estimated to have a high variance value distribution. To test this concept, how can we formulate similar motion-primitive based tasks that involves risk sensitivity?

### 2) Can we learn synergies offline?

I've noticed that offline RL has recently gained a lot of momentum in RL research [5]. Given a rich data set of transitions including multiple motion primitives, can a policy learn synergies between multiple motion primitives without any interaction with the environment? It would be nice to conduct thorough ablations on what affects final performance for learning offline.

### 3) Learning task hierarchies as another notion of "Synergy"

From my understanding, the agent selects the action $a_t = (a, q)$, where
$$
a = {\arg\max}_{a \in \{p, g\}} \Big(\max\big(\phi_{p}(s_t)\big), \max \big(\phi_{g}(s_t)\big)\Big),
$$
and
$$
q = {\arg\max}_{q \in s_t} \big(\phi_{a} (s_t)\big).
$$

I am curious if the hierarchical reinforcement learning [4] approach have been experimented: Given multiple policies each corresponding to a single motion primitive, we also learn a meta-policy that learns to choose which policy to execute given a state. 

This approach could potentially improve the scalability with respect to incorporating more motion primitives: Suppose we enable more motion primitives (rolling ($r$), toppling ($t$), squeezing ($s$), leveraging ($l$), stacking ($s$)), so that we have the action set $\mathcal{A} = \{p, g, r, t, q, l, s\}$. With the proposed model, we would have to run the input 


- Reducing action selection time of *trained* models. Suppose we have learned FCNs for more of the primitive manipulation actions , as mentioned in the paper.  Denote the set of primitive manipulations as . Then to compute an action, 
  $$
  a = {\arg\max}_{a \in \mathcal{A}} \Big(\max\big(\phi_{a}(s_t)\big)\Big)
  $$
  we would need to run the input $s_t$ through all 6 FCNs. 

  On the other hand, with the hierarchical RL approach, we only need to run the input $s_t$ once through the meta-policy, and then through the chosen single-motion-primitive-policy to obtain the position $q$.   


### References
[1] Bellemare et al. A Distributional Perspective on Reinforcement Learning. In *Proceedings of the International Conference in Machine Learning,* 2017.

[2] Dabney et al. Distributional Reinforcement Learning with Quantile Regression. In *Proceedings of the AAAI Conference on Artificial Intelligence*, 2018.

[3] Dabney et al. Implicit Quantile Networks for Distributional Reinforcement Learning. In *Proceedings of International Conference in Machine Learning,* 2018. 

[4] Kulkarni et al. Hierarchical Deep Reinforcement Learning: Integrating Temporal Abstraction and Intrinsic Motivation. In *Advances in Neural Information  Processing Systems*, 2016.

[5] Levine et al. Offline Reinforcement Learning: Tutorial, Reivew, and Perspectives on Open Problems. 2020. 