---
title: "Introduction to Reinforcement Learning: Chapter 3"
#description: <descriptive text here>
date: 2022-05-16T13:03:22-04:00
draft: false
toc: false
image: ""
tags: ["RL"]
categories: ["RL"]
mathjax: true
---

# Credits
All contents are summaries of ***"Reinforcement Learning, An Introduction second edition" by Richard Sutton and Andrew Barto***

## Chapter 3: Finite Markov Decision Processes

## 3.0 Introduction
- Markov Decision Processes is mathematically idealized form of reinforcement learning
- There is always a tension between Applicability and Mathematical Tractability, but MDPs offers well generalized solution to many RL problems.

## 3.1 The Agent-Environment Interface

### Agent-Environment Interaction in MDPs

***MDPs (Markov Decision Processes):*** Framing of a problem of learning & decision making from interaction to acheive a goal.

![Agent-Environment Interaction in MDPs](/InteractionInMDP.png "Agent-Environment Interaction in MDPs")

- ***Agent:*** Learner and decision maker that interacts with environment
- ***Environment:*** Everything outside of agent

The interaction is characterized by the three discrete or continuous spaces: States, Actions, and Reward.

1. ***States:*** Representation of environmnent, denoted by $S_t\in S$

2. ***Action:*** Action based on given State, denoted by $A_t\in A(s)$

3. ***Reward:*** Goal that agent tries to maximize over time, denoted by $R_t\in R$

At each time step t, Agent receives a state, selects an action based on the state, and receives reward.

Often, this processes is expanded by a series of time steps, which is called ***Trajectory***:

$$S_0, A_0, R_1, S_1, A_1, R_2, S_2, A_2, R_3, ... \tag{3.1}$$

### Dynamics of MDP

In finite MDP, sets of $S$, $A$, $R$ all have a finite number of elements, and the corresponding random variables $S$, $R$ have discrete probability distributions.

A probability distribution $p$ completely characterizes the dynamics of MDP, and is defined by a following equation:

$$p(s',r|s,a) = Pr(S_t=s', R_t=r | S_{t-1}=s, A_{t-1}=a) \tag{3.2}$$

$$\sum_{s'\in S} \sum_{r\in R} p(s', r|s, a) = 1 \tag{3.3}$$

MDP makes an important assumption that the preceding state must includes all information about all aspects of the past agent-environment interaction that makes a difference for the future.

When state follows the above assumption, then the state is called to have ***Markov property***

If dynamics $p$ exists, then obtaining different probabilities associated with it is trivial

$$p(s'|s, a) = \sum_{r\in R} p(s', r|s, a) \tag{3.4}$$

$$r(s,a) = E[R_t | S_{t-1}=s, A_{t-1}=a] = \sum_{r\in R}r\sum_{s'\in S}p(s', r|s, a) \tag{3.5}$$

### Distinction between Agent and Environment

A boundary between agent and environment is not the same as the physical boundary of a robot's or animal's body

Anything that cannot be changed by agent is considered to be a part of environment.

## 3.2 Goals and Rewards

- ***Reward:*** a simple number $R_t\in \mathbb R$, the agent's goal is to maximize the total amount of reward it receives

- ***Reward hypothesis:*** all of what we mean by goals and purpose can be well though of as the maximization of the expected value of the cumulative sum of a received scalar signal

## 3.3 Returns and Episodes

We seek to maximize expected ***return***.

- ***Return:*** $G_t$, some specific function of the reward sequence.

In the simplest case,
$$G_t = R_{t+1} + R_{t+2} + R_{t+3} + ... + R_{T} \tag{3.7}$$
Where $T$ is a final time step.

Breaking the whole reward sequence by a final time step especially makes sense when agent-environment interaction naturally breaks into several ***episodes***

- ***episodes:*** any sort of repeated interaction. For example, playing a game several times can be thought of as sampling several episodes. Each episodes terminate in a special state called ***terminal state***, followed by reset to a starting state.

- ***episodic tasks:*** tasks that consist of several episodes

- ***continuing tasks:*** tasks that agent-environment interaction does not naturally break into several episodes, but goes on continually without limit.

The previous return fomulation could be a problem in continuing tasks because returns can easily sum up to infinite as $T \to \infty$.

Fortunately, ***discounted*** return can solve this issue.

$$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... = \sum_{k=0}^\infty \gamma^k R_{t+k+1} \tag{3.8}$$

where $\gamma$ is a parameter, $0 \leq \gamma \leq 1$, called the ***discount rate***.

If $\gamma$ gets closer 0, the discounted return is weighted heavily toward the most recent reward. This encourages agent to focus more on recent events. On the other side, if $\gamma$ gets closer to 1, agent is encouraged to become more farsighted.

$$\displaylines{G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \gamma^3 R_{t+4} + ... \\\ = R_{t+1} + \gamma (R_{t+2} + \gamma R_{t+3} + \gamma^2 R_{t+4} + ...) \\\ = R_{t+1} + \gamma G_{t+1} \tag{3.9}}$$

If the reward is a constant +1, then the return is

$$G_t = \sum_{k=0}^\infty \gamma^k = \frac{1}{1-\gamma} \tag{3.10}$$

## 3.4 Unified Notation for Episodic and Continuing Tasks

For simplicity, we can unify the case of episodic and continuing task by considering terminal state to be a special absorbing state that transitions only to itself.

This helps us to establish a convention to obtain a single notation that covers both episodic and continuing task.

$$G_t = \sum_{k=t+1}^T \gamma^{k-t-1}R_k \tag{3.11}$$

including the possiblity that $T = \infty$ or $\gamma = 1$ (but not both).

## 3.5 Policies and Value Functions

Many reinforcement learning algorithms seek to estimate value functions. ***Value function*** represents how good a given state is for the agents.

### Policy and Value Functions

***policy*** is a mapping from states to probabilities of selecting each possible action. Mathmatically, for given policy $\pi$ at time $t$, the mapping is described as $\pi(a|s)$.

***state-value function*** of a state $s$ under a policy $\pi$, $v_{\pi}(s)$, is defined as

$$v_{\pi}(s) = \mathbb E_{\pi}[G_t | S_t = s] = \mathbb E_{\pi}[\sum_{k=0}^{\infty} \gamma^k R_{t+k+1} | S_t = s] \tag{3.12}$$

for all $s \in \mathbb S$, where $\mathbb E_{\pi}[\cdot]$ is an expectation of a random variable given that the agent follows policy $\pi$, and $t$ is any time step.

Similarly, ***action-value function*** of a state and an action at time $t$ under a policy $\pi$, $q_{\pi}(s,a)$, is defined as

$$q_{\pi}(s,a) = \mathbb E_{\pi}[G_t | S_t = s, A_t = a] = \mathbb E_{\pi}[\sum_{k=0}^{\infty} \gamma^k R_{t+k+1} | S_t = s, A_t = a] \tag{3.13}$$

### Bellman equation

A nice thing about these value functions is that they are recursive, meaning that the value function can be represented as the value function itself.

For any policy $\pi$ and any state $s$, the following equation holds between the value of $s$ and the value of its possible successor states:

$$v_{\pi} = \mathbb E_{\pi}[G_t | S_t = s]$$
$$ = \mathbb E_{\pi}[R_{t+1} + \gamma G_{t+1} | S_t = s]$$
$$ = \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a)[r + \gamma \mathbb E_{\pi}[G_{t+1}|S_{t+1}=s']]$$
$$ = \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a)[r + \gamma v_{\pi}(s')] \tag{3.14}$$

for all $s \in \mathbb S$. Actions, a, are taken from the set $\mathbb A(s)$ and the next states, $s'$ are taken from the set $\mathbb S$, and rewards, $r$, are taken from the set $\mathbb R$

The equation 3.14 is ***Bellman equation*** for $v_{\pi}$. 
Basically, Bellman equation is a recursive definition of the expected discounted return, $v_{\pi}$. This means $v_{\pi}(s)$ can be represented as the expression of $v_{\pi}(s')$, which makes the value of current state easier to understand in terms of the value of previous state. It states $v_{\pi}(s)$ is the average of $r + \gamma v_{\pi}(s')$ of the future states, weighted by the possibilities of $\pi(a|s)$ and $p(s',r|s,a)$.

Note that this recursive definition of Bellman equation enables us to build a backup diagram of tree-like structure, and is a convenient method to visualize the overall picture.

## 3.6 Optimal Policies and Optimal Value Functions

Choosing a policy that always maximizes the value gives us an optimal value functions.
$v_{\star}(s) = \max_{\pi} v_{\pi}(s)\tag{3.15}$
this can be also written as action-value function
$q_{\star}(s, a) = \max_{\pi} q_{\pi}(s, a)\tag{3.16}$

decomposing the function gives 
$q_{\star}(s, a) = \mathbb E[R_{t+1} + \gamma v_{\star}(S_{t+1}) | S_t = s, A_t = a]\tag{3.17}$