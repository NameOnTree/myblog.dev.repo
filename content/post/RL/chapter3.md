---
title: "Chapter 3: Finite Markov Decision Processes"
#description: <descriptive text here>
date: 2022-05-16T13:03:22-04:00
draft: true
math: true
toc: false
image: ""
tags: ["RL"]
categories: ["RL"]
---

# Credits
All contents of this post are referenced from ***"Reinforcement Learning, An Introduction second edition" by Richard Sutton and Andrew Barto***

# Chapter 3: Finite Markov Decision Processes

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

Anything that cannot be changed by agent is considered to be a part of environment



