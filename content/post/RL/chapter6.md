---
title: "Chapter6"
date: 2022-07-28T04:38:34-04:00
draft: true
toc: false
image: ""
tags: ["RL"]
categories: ["RL"]
mathjax: true
---

# Credits
All contents of this post are referenced from ***"Reinforcement Learning, An Introduction second edition" by Richard Sutton and Andrew Barto***

# Chapter 6: Temporal-Difference Learning

## 6.1 TD prediction

Both TD and Monte Carlo methods use experience (sample trajectory) to solve the prediction problem. Because MC method uses a return $G_t$ as the update target, it needs to wait until the return is known.

### MC method

constant-$\alpha$ MC method can be represented as

$$V(S_t) \leftarrow V(S_t) + \alpha[G_t - V(S_t)] \tag{6.1}$$

where $G_t$ is the actual return following time t, and $\alpha$ is a constant step-size parameter.


### TD(0) or one-step TD
Whereas Monte Carlo methods must wait until $G_t$ is known, TD methods can immediately update $V(S_t)$ after the next time step by making use of observed reward $R_{t+1}$ and the estimate $V(S_{t+1})$. 

$$V(S_t) \leftarrow V(S_t) + \alpha[R_{t+1} + \gamma V(S_{t+1}) - V(S_t)] \tag{6.2}$$

Here, the update target for TD update is $R_{t+1} + \gamma V(S_{t+1})$. This TD method is called TD(0), or one-step TD.

Because TD(0) makes use of an existing estimate, it is also called as ***bootstrapping method*** like DP.

$$v_{\pi}(s) = \mathbb E_{\pi}[G_t | S_t = s] \tag{6.3}$$
$$ = \mathbb E_{\pi}[R_{t+1} + \gamma G_{t+1} | S_t = s] \tag{from 3.9}$$
$$ = \mathbb E_{\pi}[R_{t+1} + \gamma v_{\pi}(S_{t+1}) | S_t = s] \tag{6.4}$$

The quantity in brackets in the TD(0) update is an error called ***TD error***, measuring the difference between the estimated value of $S_t$ and the better estimate $R_{t+1} + \gamma V(S_{t+1})$

$$\delta_t = R_{t+1} + \gamma V(S_{t+1}) - V(S_t) \tag{6.5}$$

Actually, the Monte Carlo error can be written as a sum of TD erros if array V does not change during an episode.

$$G_t = V(S_t) = R_{t+1} + \gamma G_{t+1} - V(S_t) + \gamma V(S_{t+1}) - \gamma V(S_{t+1}) \tag{from 3.9}$$
$$ = \delta_t + \gamma(G_{t+1} - V(S_{t+1}))$$
$$ = \delta_t + \gamma \delta_{t+1} + \gamma^2(G_{t+2} - V(S_{t+2}))$$ 
$$ = \delta_t + \gamma \delta_{t+1} + \gamma^2 \delta_{t+2} + ... + \gamma^{T-t-1} \delta_{T-1} + \gamma^{T-t}(G_T - V(S_{T}))$$
$$ = \delta_t + \gamma \delta_{t+1} + \gamma^2 \delta_{t+2} + ... + \gamma^{T-t-1} \delta_{T-1} + \gamma^{T-t}(0 - 0)$$
$$ = \sum_{k=t}^{T-1} \gamma^{k-t}\delta_t \tag{6.6}$$