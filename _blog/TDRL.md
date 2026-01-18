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

Some breakthroughs in neuroscience suggest a remarkable parallel between temporal difference (TD) error in reinforcement learning and the firing behavior of dopamine neurons in the brain. This correspondence, first observed by Schultz, Dayan, and Montague (1997), revealed that dopamine responses reflect reward prediction errors, just like those computed in TD learning.

To illustrate this link, we’ll walk through a classic Pavlovian conditioning experiment and analyze it using the lens of RL.

## What is Pavlovian Conditioning?
A Pavlovian conditioning experiment, pioneered by Ivan Pavlov, demonstrates how a neutral stimulus (like a bell) can trigger a learned, reflexive response (like salivation) by being repeatedly paired with a naturally rewarding stimulus (like food).

Initially, food causes salivation (unconditioned stimulus → unconditioned response). Over time, the bell (neutral stimulus) is paired with the food. Eventually, the bell alone causes salivation, this becomes a conditioned response triggered by a conditioned stimulus.

In neuroscience, a similar paradigm is used: a cue (e.g., a tone) is paired with a reward (e.g., juice) to study learning and reward signaling in the brain.

## setup
A monkey hears a cue (e.g., a tone), then gets a juice reward.

## Phase 1: Before learning

Before learning, the cue has no predictive meaning. The reward is unexpected.

At reward time, TD error is:

$$
\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)
$$

Since values are not yet learned:

$$
V(s_t)\approx 0, \quad V(s_{t+1})\approx 0
$$

So:

$$
\delta_{\text{reward}} \approx r_t > 0
$$

### Neural response
Dopamine neurons show a **burst at reward delivery** (positive prediction error).

---

## Phase 2: After learning

After learning, the cue predicts reward, so the cue value increases:

$$
V(\text{cue}) > 0
$$

At cue time the reward has not occurred yet, so:

$$
r_{\text{cue}} = 0
$$

But the TD error becomes positive because the next state predicts reward:

$$
\delta_{\text{cue}} = 0 + \gamma V(\text{reward}) - V(\text{cue}) > 0
$$

At reward time, the reward is fully predicted, so prediction error is near zero:

$$
\delta_{\text{reward}} \approx 0
$$

### Neural response
Dopamine firing **shifts from reward time to cue time**:
- burst at cue onset
- little/no change at reward delivery

---

## Phase 3: Reward omission

If the cue occurs but reward is omitted:

$$
r_t = 0
$$

Then:

$$
\delta_{\text{reward}} < 0
$$

### Neural response
Dopamine neurons show a **dip** at the time reward was expected (negative prediction error).
