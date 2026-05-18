---
title: "MAGNETO — Image Translation for Network Intrusion Detection"
excerpt: "Extended image-translation framework for classifying network attack data with machine learning, building on DeepInsight to preserve semantic feature relationships across traffic flow representations."
collection: portfolio
permalink: /portfolio/magneto
date: 2023-08-15
---

**Status:** Published · Massey University Cybersecurity Lab

**Area:** Cybersecurity · Machine Learning

**Tech:** DeepInsight · CNNs · Network Traffic Analysis

---

MAGNETO extends the DeepInsight image-translation method to network intrusion detection. By converting tabular traffic flow data into images that preserve semantic relationships between features, the framework enables convolutional neural networks to exploit spatial structure that flat feature vectors obscure.

## Approach

The translation pipeline captures inter-feature correlations through a manifold-learning layout step, then encodes them as pixel proximity. This retains contextual meaning lost by standard tabular ML approaches.

## Publication

Published in *Electronics* (MDPI), Vol. 12, Issue 16, August 2023.
DOI: [10.3390/electronics12163463](https://doi.org/10.3390/electronics12163463)
