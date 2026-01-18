---
title: "Noise2Noise: How Corruption Produces Clarity"
excerpt: "Why learning from noisy targets still recovers clean signals"
layout: single
author_profile: false
read_time: true
image:
permalink: /blog/noise2noise/
math: true
---

# Learning-based denoising

Most classical denoising methods start from an explicit *physical* or *statistical* model:

- a noise model (Gaussian / Poisson / Poisson–Gaussian),
- a likelihood function,
- and then an estimator (MAP, MMSE, etc.).

Deep denoising methods flip this story:

> Instead of writing down the model of the image, we learn the mapping that removes noise.

But here is the apparent paradox:

### **How can we learn to denoise without clean images?**

That is precisely the central surprise behind **Noise2Noise**:

> A network can learn to output *clean* signals even if we train it using only *noisy targets*.

This idea is often summarized by the phrase:

## Corruption produces clarity

Let’s start with a simple scalar estimation problem — exactly the intuition from the slide.

---

# Preliminaries: a scalar estimation perspective

Imagine you are measuring the room temperature, but your sensor is unreliable.

You collect repeated measurements:

\[
y_1, y_2, \dots
\]

Each measurement is noisy, but the true unknown temperature is a fixed value:

\[
z \in \mathbb{R}
\]

So your problem is:

- **Given unreliable measurements \(y\)**,
- **estimate the true value \(z\)**.

A general way to define this estimation problem is:

\[
\hat{z}
\;=\;
\arg\min_z \; \mathbb{E}_y\left[\,L(z, y)\,\right]
\]

where \(L(z,y)\) is a loss function that penalizes deviations between your estimate \(z\) and measurement \(y\).

This simple expression is the *entire foundation* of why Noise2Noise works.

---

# Why L2 loss gives the mean

Assume we use L2 loss:

\[
L(z,y) = (z-y)^2
\]

Then:

\[
\hat{z}
=
\arg\min_z \mathbb{E}_y[(z-y)^2]
\]

Let’s compute what value of \(z\) minimizes this.

Expand:

\[
\mathbb{E}[(z-y)^2]
=
\mathbb{E}[z^2 - 2zy + y^2]
=
z^2 - 2z\mathbb{E}[y] + \mathbb{E}[y^2]
\]

Differentiate w.r.t \(z\):

\[
\frac{d}{dz}
\left(
z^2 - 2z\mathbb{E}[y] + \mathbb{E}[y^2]
\right)
=
2z - 2\mathbb{E}[y]
\]

Set derivative to zero:

\[
2z - 2\mathbb{E}[y] = 0
\quad\Rightarrow\quad
z = \mathbb{E}[y]
\]

✅ **So under L2 loss, the best estimator is the mean of the observations:**

\[
\hat{z} = \mathbb{E}[y]
\]

This is a deep idea:

> L2 training makes the model learn conditional expectations.

In denoising language, it means:

\[
f^\*(x) = \mathbb{E}[y \mid x]
\]

---

# Why L1 loss gives the median

If instead we use L1 loss:

\[
L(z,y) = |z-y|
\]

Then the minimizer is the median:

\[
\hat{z} = \text{median}(y)
\]

✅ This explains why L1 loss tends to preserve edges better and is more robust to outliers.

So:

- **L2 → mean**
- **L1 → median**

---

# The key bridge to Noise2Noise

Now comes the crucial denoising translation.

In denoising, the measurements \(y\) are noisy versions of the clean signal \(x\):

\[
y = x + n
\]

where \(n\) is random noise.

If the noise is **zero mean**, i.e.

\[
\mathbb{E}[n] = 0,
\]

then:

\[
\mathbb{E}[y]
=
\mathbb{E}[x+n]
=
x + \mathbb{E}[n]
=
x
\]

So:

\[
\mathbb{E}[y] = x
\]

This is the *magic step*.

It means:

> If your loss makes you learn the mean, and the noisy observation has mean equal to the clean signal, then learning from noisy targets still recovers the clean truth.

This is exactly why corruption can produce clarity.

---

# What this means for training neural networks

Now replace the scalar \(z\) by a **network output** \(f_\theta(x)\).

Noise2Noise trains with pairs of noisy images:

- input: \(y_1 = x + n_1\)
- target: \(y_2 = x + n_2\)

and minimizes:

\[
\min_\theta \;
\mathbb{E}\big[\|f_\theta(y_1) - y_2\|^2\big]
\]

At first this looks wrong:

> “The target is noisy! Won’t the network learn noise?”

But the scalar analysis already answered this.

Under L2 loss:

\[
f_\theta^\*(y_1)
=
\mathbb{E}[y_2 \mid y_1]
\]

and because:

\[
\mathbb{E}[y_2 \mid y_1]
=
\mathbb{E}[x+n_2 \mid y_1]
=
\mathbb{E}[x \mid y_1]
+
\underbrace{\mathbb{E}[n_2 \mid y_1]}_{=0}
\]

we get:

\[
f_\theta^\*(y_1)
=
\mathbb{E}[x \mid y_1]
\]

✅ **So training to predict a noisy target is equivalent to training to predict the clean signal — as long as the noise is independent and zero mean.**

That is the entire Noise2Noise theorem in one line.

---

# Intuition: why the network cannot learn the noise

If \(n_1\) and \(n_2\) are independent, then:

- the input noise has no predictive power about the target noise.

So the best strategy for the network is:

- ignore noise-like fluctuations,
- predict stable structure shared across corrupted views.

This is why:

> Random corruption cancels out in expectation, but structure accumulates.
