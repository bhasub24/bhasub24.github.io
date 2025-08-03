---
title: "Beyond Reward: The Limitations of Thorndike’s Law in Modeling the Mind"
excerpt: "From Dopamine to Decision: RL as a Cognitive Framework"
layout: single
author_profile: false
read_time: true
image: 
permalink: /blog/TD/
math: true
---

# Introduction

Reinforcement Learning (RL) has become a foundational framework for understanding both artificial and biological learning. Its core premise is simple: learn through trial and error to maximize future rewards. This echoes an early psychological principle, Thorndike’s Law of Effect, which states:

>Responses that produce a satisfying effect in a particular situation become more likely to occur again
– Edward Thorndike, 1911

While this idea forms the philosophical backbone of many RL algorithms, modern neuroscience and cognitive science increasingly reveal its limitations. Let’s explore how RL formalizes this principle, how it maps onto brain activity, and why we must go beyond it to capture the richness of human cognition.

# Temporal Difference Learning
In RL, Temporal Difference (TD) learning is a key mechanism by which agents learn to estimate state values, how much future reward can be expected from any given situation.

The value of a state $$V(s_{t})$$ is updated using the TD error:

$$
\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)
$$

This prediction error $$\delta_t$$ tells the agent how much it under- or over-estimated the value of its current state.

# Link between TD and Dopamine firing

Lets walk through an experiment to show how firing evolves over learning.

## setup
A monkey hears a cue (e.g., a tone), then gets a juice reward.

## Phase 1: Before learning
