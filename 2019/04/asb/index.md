---
title: An aliased seabed detection algorithm
<!-- author: Rob Blackwell -->
<!-- email: roback28@bas.ac.uk -->
bibliography: references.bib
---

## Introduction

In fisheries acoustics, aliased seabed is an echogram corruption that
is caused by seabed reverberations from preceding pings coinciding
with the current ping reception [@renfree2016optimizing].

$$R_{\mathcal{A}} = \frac{\mbox{mod}(2 R_S,\  c \ I_T)}{2}, \quad \textrm{where} \quad R_L < R_S < R_{max}$$ {#eq:id}

The phenomenon occurs when Equation ({@eq:id}) is satisfied, where
$R_{\mathcal{A}}$ is the range of the aliased seabed, $R_S$ is the
range of the seabed, $I_T$ is the ping interval, $c$ is the speed of
sound in water, $R_L$ is the logging range and $R_{max}$ is the
maximum range of the transducer.

Seabed reflections can be seen clearly in split-beam angle data, but
the patterns are difficult to segment because of noise.

## Method

For Simrad EK60 data, along-ship angle ($\eta_{\theta}$) and
athwart-ship angle ($\eta_{\phi}$) vary between -128 and 127 and so we
take the mean-squared over a moving window to smooth the image and
accentuate coherent signal (window sizes determined empirically).
Whilst these pixels fall within the aliased seabed regions, only a
small percentage of area is identified. However, we can take these
pixels and then examine the surrounding region in volume backscatter
($S_v$). Hence, we derive a five-step algorithm:


1. Find the mean squared of a $28 \times 28$ moving window over
  $\eta_\theta$ and select cells $> T_{\theta}$ to produce a mask
  $m_1$.

2. Find the mean squared of a $52 \times 52$ moving window over
  $\eta_\phi$ and select cells $> T_{\phi}$ to produce a mask
  $m_2$.
  
3. Combine the masks $m = m_1 \lor m_2$.

4. Select pixels from $S_v$ using the mask, $m$ and determine the
  median $S_v$ value of the selection to use as a threshold $T$.
  
5. Select regions from $S_v$ where $S_v > T$ and which intersect
  $m$. The resulting mask is the union of the selected regions and $m$.
 
The final mask is a grid indicating those pixels that have been
classified as aliased seabed.

## Discussion

In theory, aliased seabed is an additive corruption, and an alias
which crosses a scattering layer should have a higher intensity than
the surrounding scattering layer pixels. Unfortunately, this is not
always the case and we have seen the algorithm "leak" into deep
scattering layers. For this reason, the algorithm is not suitable for
autonomous, unsupervised operation, but it is suitable for supervised
environments where the output is checked by a human operator.

The algorithm has been implemented in
[Python](https://github.com/bas-acoustics/echopy) and in
[Julia](https://github.com/EchoJulia/AliasedSeabed.jl/blob/master/src/AliasedSeabed.jl).

## References
