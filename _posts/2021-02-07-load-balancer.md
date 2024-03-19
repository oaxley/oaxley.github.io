---
title: A dynamic load balancer
tags: [research]
mathjax: true
---
# Dynamic Left or Right?

While reviewing some old sources, I was wondering if they was a way to perform Load Balancing
of traffic (like order passing) automatically, only with the knowledge of previous traffic and
more specifically the response time from where we are connected.  

TL;DR: Yes it's possible and it works ;)

## Definitions

The goal is to modify the dispatching of objects among the lines so that we always get the best latency 
available for our objects. The load on each line is done automatically depending on the latency of latest items.

**Terms**:

$$
L_i: \text{The latency of line i}
\\
T_i: \text{The target ratio of line  i}
\\
R_i: \text{The latency ratio between the 'reference' line and the line i}
$$

## Latency Ratio

The latency ratio for a line is defined against the reference line.

Let's $L_1$ be the reference Line, we have:

$$
R_1 = \dfrac{L_1}{L_1} = 1.0 \\
R_2 = \dfrac{L_1}{L_2} = x \\
R_3 = \dfrac{L_1}{L_3} = y
$$

For example:

If $L_1 = 10ms$, $L2 = 15ms$, and $L3 = 19ms$, then we have:

$$
R_1 = 1.0 \\ R_2 = 0.67 \\ R_3 = 0.53
$$

## Target Ratio

- The global target ratio (when considering all the available lines) should always be equal to 1.0:

$$
\sum_{i=1}^{n}T_i = 1.0 ^{(I)}
$$

- The target ratio for a specific line is proportional to its latency ratio and the target ratio of the reference line:
    
    $$
    \forall i \in \{2 \ldots (n-1)\} \\T_i = T_1 \times R_i ^{(II)}
    $$
    
- The ratio for the last line (line #n) should take care of all the previous lines and account for the precision errors in the computation:

$$
T_n = 1 - \sum_{i=1}^{n-1}T_i
$$

- From (I) and (II) we can deduct the current target ratio for the reference line (with n=3):

$$
T_1 + T_2 + T_3 = 1.0
$$

$$
T_1 \times R_1 + T_1 \times R_2 + T_1 \times R_3 = 1.0
$$

$$
T_1 \cdot (R_1 + R_2 + R_3) = 1.0
$$

$$
T_1 = \dfrac{1.0}{\sum_{1}^{n}R_n}
$$

## Example

1. The current latency for the lines are the same than before:

$$
L_1 = 10ms, L_2 = 15ms, L_3 = 19ms
$$

1. We compute the ratio of each lines:

$$
R_1 = \dfrac{L_1}{L_1} = 1.00\\
R_2 = \dfrac{L_1}{L_2} = 0.67\\
R_3 = \dfrac{L_1}{L_3} = 0.53
$$

1. From here, we can compute $T_1$:

$$
T_1 = \dfrac{1.0}{\sum_{1}^{n}R_n} = \dfrac{1.0}{1.0 + 0.67 + 0.53} = \dfrac{1.0}{2.19} = 0.46
$$

1. Then each target ratio for the remaining lines

$$
T_2 = T_1 \times R_2 = 0.46 \times 0.67 = 0.30 \\
T_3 = T_1 \times R_3 = 0.46 \times 0.53 = 0.24
$$

For the last line, we can also compute $T_3$ with:

$$
T_3 = 1 - \sum_{i=1}^{2}T_i \\
T_3 = 1 - 0.46 - 0.30 = 0.24
$$

The producer will then use this new target ratio to dispatch orders on the proper line without manual intervention.

## Improvement ideas

- Latency measurement can use the exponential average of the last 5 to 10 orders

$$
S_t = \alpha \cdot x + (1 - \alpha) \cdot S_{t-1}
$$

- To improve stability, change of the line is not performed if the latency is between a threshold (cf hysteresis model)