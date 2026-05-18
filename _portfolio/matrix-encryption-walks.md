---
title: "Matrix Encryption Walks — Lightweight IoT Cryptography"
excerpt: "A symmetric stream cipher based on graph walks over 2D matrices, designed for low-resource IoT and smart-device environments, with linear complexity and a provably large key space."
collection: portfolio
permalink: /portfolio/matrix-encryption-walks
date: 2023-08-16
---

**Status:** Published

**Area:** Cryptography · IoT Security

**Tech:** Graph Theory · Stream Ciphers · Python

---

Matrix Encryption Walks (MEW) is a novel lightweight symmetric encryption algorithm suited for constrained devices — IoT sensors, smart devices, and edge systems — where computational overhead is a hard constraint.

## Design

The algorithm constructs cipher keystreams by performing random-walk traversals over a 2-dimensional key matrix. The walk path is determined by an initial key, and the resulting output stream is XOR-combined with plaintext. This yields linear time complexity, a vast key space, and strong resistance to frequency analysis and chosen/known plaintext attacks.

## From thesis to publication

This work originated in my Master's thesis at AUT (2016) and was formalised into a deployable algorithm published in *Cryptography* (MDPI) in 2023.

## Publication

Published in *Cryptography* (MDPI), Vol. 7, Issue 3, Article 41, August 2023.
DOI: [10.3390/cryptography7030041](https://doi.org/10.3390/cryptography7030041)
