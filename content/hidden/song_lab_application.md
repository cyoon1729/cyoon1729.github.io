---
title: Follow-up Thoughts on a Couple Previous Publications from Dr. Shuran Song Lab.
datePublished: September 3, 2020
abstract: I have read the papers "Spatial Action Maps for Mobile Manipulation" and "Learning Synergies between Pushing and Grasping with Self-supervised Deep Reinforcement Learning). I have summarized a few of my follow-up thoughts below.
imagePath: placeholder_thumbnail.png
---


I have read the papers "Spatial Action Maps for Mobile Manipulation" and "Learning Synergies between Pushing and Grasping with Self-supervised Deep Reinforcement Learning." I have summarized a few of my follow-up thoughts below.

### 1) Distributional value approximations embedded into pixel-wise output map

Distributional value approximation [1, 2, 3] works great with deep reinforcement learning. I am wondering if these performance gains can be translated to learning pixel-wise value maps. If the added computational costs are too prohibitive (e.g. the output channel will now be multiple channels deep, each representing a single discretized unit of the value distribution), how can we make it more efficient? 

Another reason for considering distributional value approximation is that, intuitively, it can introduce "risk sensitive" behavior, allowing agents to act conservatively when a certain state-action pair is estimated to have a high variance value distribution. To test this concept, how can we design similar motion-primitive or mobile navigation based tasks that involve risk sensitivity?

### 2) Can we learn synergies offline?

I've noticed that offline RL has recently gained a lot of momentum in RL research [5]. Given a rich data set of transitions including multiple motion primitives, can a policy learn synergies between multiple motion primitives without any interaction with the environment (starting with simulations, but eventually moving onto sim-to-real)? 

It would be particularly interesting to benchmark current state-of-the-art of offline RL to robotic manipulation tasks, and conduct thorough ablations on what affects performance of policies learned offline. 

From the pixel-map perspective, we could also cast the problem as a supervised learning task where we use labeled reward (or value) maps as an analogue to "expert data" in imitation learning.

### 3) Is there a more efficient formulation of models when incorporating more motion primitives?

The Pushing-Grasping paper explains as one of its future directions incorporating multiple types of motion primitives, such as rolling ($r$), toppling ($t$), squeezing ($s$), leveraging ($l$), stacking ($s$). Suppose, as an extreme case, we design a model that aims to learn the synergies between all of these. With the model proposed in the paper, a single MDP step would have to run the input state through 7 FCNs (for each motion primitive), and then find the pixel with the maximum q value of each output to determine the action. This would mean an increase in computational cost and time in between action selection, which could be problematic in real-world settings where low latency is crucial.

A more scalable approach could be inspired by hierarchical RL. For instance, if we have a meta-policy that maps state observations to motion-primitives, and then we obtain an action by running the observation through the sub-policy corresponding to a single motio-primitive. Such approach would only need run the input through two networks (meta-policy and chosen sub-policy) regardless of how many motion-primitives we incorporate. Some related papers that come to mind are [5, 6].

### References
[1] Bellemare et al. A Distributional Perspective on Reinforcement Learning. In *Proceedings of the International Conference in Machine Learning,* 2017.

[2] Dabney et al. Distributional Reinforcement Learning with Quantile Regression. In *Proceedings of the AAAI Conference on Artificial Intelligence*, 2018.

[3] Dabney et al. Implicit Quantile Networks for Distributional Reinforcement Learning. In *Proceedings of International Conference in Machine Learning,* 2018. 

[4] Levine et al. Offline Reinforcement Learning: Tutorial, Reivew, and Perspectives on Open Problems. 2020. 

[5] Bacon et al. THe Option-Critic Architecture. In *Proceedings of the AAAI Conference on Artificial Intelligence*, 2016.

[6] Kulkarni et al. Hierarchical Deep Reinforcement Learning: Integrating Temporal Abstraction and Intrinsic Motivation. In *Advances in Neural Information Processing Systems*, 2016.