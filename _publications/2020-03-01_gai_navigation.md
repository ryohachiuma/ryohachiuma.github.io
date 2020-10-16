---
title: "Efficient Exploration in Constrained Environments with Goal-Oriented Reference Path"
collection: publications
permalink: /publications/2020-03-01_gai_navigation
excerpt: 'This paper proposes to **decouple planning and control** by combining traditional path planning algorithms, supervised learning (SL) and reinforcement learning (RL) algorithms in a synergistic way. By exploiting waypoints produced from SL, an RL agent easily learns to navigate to arbitrary goal locations, and generalize to novel environments.<br/><center><img src="/images/2003_gai_proposed_method.png" width="750"></center>'
date: 2020-03-01
venue: 'IROS'
paperurl: 'https://arxiv.org/abs/2003.01641'
# citation: 'Your Name, You. (2009). &quot;Paper Title Number 1.&quot; <i>Journal 1</i>. 1(1).'
---

<center><img src="/images/2003_gai_proposed_method.png" width="750"></center><br>
Designing RL agents that can learn complex, safe behavior in constrained environments for navigation has been getting a lot of attention recently. This has been mainly driven by the race to achieve artificial general intelligence where AI agents can achieve human-like performance. It is desirable that the RL agents can generalize to novel environments while achieving optimal performance. In this paper, we present a sample-efficient way of designing agents that can learn to generalize to different environments by combining classical planning algorithm, supervised learning, and reinforcement learning. The motivation of this combination comes from:

- generating optimal path on obstacle cluttered environment is **easy** using traditional path planning algorithm (A*, RRT, etc.)
- imitating the path (which we call waypoints) using SL is **easy**
- Waypoints that guides an RL agent to goal location makes **easier** to learn optimal policy by limiting exploration area

Below figure shows the architecture of the proposed method.

<center><img src="/images/2003_gai_architecture.png" width="750"></center><br>
Then, how to exploit the waypoints? We use the similar technique with our [previous research](https://keiohta.github.io/publications/2019-11-04_iros), that exploits the waypoints information to limit the search areas to explore for an RL agent as a form of reward function. The reward terms are: $w_1 d_{\rm path} + w_2 n_{\rm progress}$, essentially the first term penalizes to explore too far from waypoints, and second term encourages the agent to move toward a goal location along with the waypoints.

We tested our method on recently proposed [Safety Gym](https://openai.com/blog/safety-gym/), which easily allows to produce different environments with various types of obstacles, and also provides three types of robots (point, car, and doggo). The results shows the sample efficiency is improved.

<center><img src="/images/2003_gai_results.png" width="750"></center><br>
Next, we demonstrated our method also improves generalization capability. We prepared four different types of unseen environments and evaluate goal reach rate and steps to reach goals.

<center><img src="/images/2003_gai_envs.png" width="750"><br><img src="/images/2003_gai_generalization_table.png" width="750"></center><br>
If you are interested in our paper, please check [our paper](https://arxiv.org/abs/2003.01641) for more details!