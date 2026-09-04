---
layout: post
title: "Notes on Hierarchical World Models"
date: 2024-01-15
description: "A quick exploration of how hierarchical representations can help agents reason over longer horizons in model-based RL."
tags: [RL, World Models]
---

## Motivation

Long-horizon planning is hard. Model-based RL agents that plan with flat representations suffer from compounding prediction errors — small mistakes early in the rollout snowball into useless imagined trajectories by the end.

One promising direction: **hierarchical world models**, which reason at multiple levels of abstraction simultaneously.

---

## The Core Idea

Rather than predicting every low-level step, a hierarchical model learns:

1. A **high-level latent** that captures coarse structure (e.g., "move to the left side of the room")
2. A **low-level dynamics model** conditioned on that latent for fine-grained control

This is reminiscent of how humans plan — we think "go to the kitchen" first, then figure out the individual steps.

## A Simple Formulation

Given states $s_t$ and actions $a_t$, we learn:

- High-level encoder: $z_t = f_\phi(s_t)$
- Low-level dynamics: $s_{t+1} = g_\theta(s_t, a_t, z_t)$
- High-level transition: $z_{t+k} = h_\psi(z_t, g_t)$ for some goal $g_t$

The key tension is deciding **how often** to update the high-level latent — too frequent and you lose abstraction; too infrequent and you lose accuracy.

---

## Open Questions

- How do you train the high-level and low-level models jointly without collapse?
- What's the right temporal abstraction — fixed $k$ steps, or variable?
- How does this interact with offline data, where the high-level structure isn't always apparent?

These are things I've been thinking about. More notes to come.
