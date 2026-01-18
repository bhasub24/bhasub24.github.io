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

At the beginning, the cue is meaningless (it does not predict anything yet). The reward is unexpected, so the prediction error is large and positive.

At reward time, the TD error is:

$$
\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)
$$

Since the agent has not learned any predictions yet, we can assume:

$$
V(s_t) \approx 0
\qquad \text{and} \qquad
V(s_{t+1}) \approx 0
$$

So at reward delivery:

$$
\delta_{\text{reward}} \approx r_t > 0
$$

### Neural response
Dopamine neurons show a **burst of firing at the time of reward**, because the reward is better than expected (positive prediction error).

---

## Phase 2: After learning

After repeated pairings, the cue becomes predictive: the agent learns that reward follows the cue. This means the value of the cue increases:

$$
V(\text{cue}) \gg 0
$$

Now the most important subtle point is the following:

At the time the cue is presented, **the reward has not happened yet**, so the immediate reward is:

$$
r_{\text{cue}} = 0
$$

But the TD error can still be positive at cue time because TD learning is **bootstrapped**:

$$
\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)
$$

At cue time:
- $$r_{\text{cue}} = 0$$
- the next state is the reward-delivery state, so $$V(s_{t+1}) = V(\text{reward})$$ is high
- initially $$V(\text{cue})$$ is lower than it should be (underestimation during learning)

So:

$$
\delta_{\text{cue}} = 0 + \gamma V(\text{reward}) - V(\text{cue})
$$

A common intuition mistake is to assume $$r=1$$ at cue time, but that is incorrect. The cue is only a predictor; it does not deliver reward. The prediction error is positive **because the next state's value is high**, not because reward is received.

If (for intuition) we take:

$$
V(\text{reward}) = 1, \quad V(\text{cue}) \approx 0
$$

then

$$
\delta_{\text{cue}} = \gamma \cdot 1 - 0 = \gamma
$$

and if $$\gamma = 1$$:

$$
\delta_{\text{cue}} = 1
$$

### What happens at reward time after learning?

At reward time, the reward is now expected. Also, after the reward is delivered, the episode effectively ends (or there is no further reward), so:

$$
V(\text{after reward}) \approx 0
$$

Thus, at reward delivery:

$$
\delta_{\text{reward}} = r_t + \gamma V(\text{after reward}) - V(\text{reward})
$$

With $$r_t = 1$$, $$V(\text{after reward}) \approx 0$$, and $$V(\text{reward}) \approx 1$$:

$$
\delta_{\text{reward}} = 1 + 0 - 1 = 0
$$

### Neural response
After learning:
- Dopamine neurons show a **burst at cue onset** (because prediction is updated there)
- Dopamine neurons show **little to no change at reward delivery** (because reward is fully predicted)

This is what is meant by the firing **“shifting from reward to cue.”**

---

## Phase 3: Reward omission

Now suppose the cue occurs but the reward is omitted.

Since the cue predicts reward, the cue still has high value:

$$
V(\text{cue}) \gg 0
$$

But at the expected reward time:

$$
r_t = 0
$$

and again:

$$
V(\text{after reward}) \approx 0
$$

So the TD error becomes:

$$
\delta_{\text{reward}} = 0 + \gamma V(\text{after reward}) - V(\text{reward}) < 0
$$

or equivalently, thinking one step earlier:

$$
\delta = 0 - V(\text{cue}) < 0
$$

### Neural response
Dopamine neurons show a **dip in firing at the time the reward was expected**, representing a **negative prediction error** ("worse than expected").

---

## Why does firing shift from reward to cue?

After learning, the cue becomes predictive, meaning:

$$
V(\text{cue}) \approx \gamma V(\text{reward})
$$

So the TD error becomes concentrated at cue onset:

$$
\delta_{\text{cue}} = 0 + \gamma V(\text{reward}) - V(\text{cue})
$$

while at reward time:

$$
\delta_{\text{reward}} = 1 + \gamma V(\text{after reward}) - V(\text{reward}) \approx 0
$$

So dopamine neurons fire earlier, at the cue, because that is when the brain updates reward expectation.
