---
title: "Coordinate Matrix Encryption"
excerpt: "A novel symmetric word-oriented stream cipher using a 2D key matrix, visual cryptography security principles, and error-correcting code theory to produce non-singular plaintext-to-ciphertext mappings with randomised padding — offering brute-force resistance 157,899 orders of magnitude greater than 128-bit AES."
collection: portfolio
permalink: /portfolio/coordinate-matrix-encryption
date: 2016-09-01
---

**Status:** Published · Auckland University of Technology

**Area:** Cryptography · Applied Mathematics

**Implementation:** Java · Custom key matrix · PRNG-based coordinate selection

---

## Overview

Coordinate Matrix Encryption (CME) is a word-oriented symmetric stream cipher proposed in my Master's thesis at Auckland University of Technology. It is built on a 2-dimensional coordinate key matrix, drawing on the security principles of Visual Cryptography (VC) and the structural concepts of error-correcting codes to produce a non-singular mapping of plaintext to ciphertext — meaning a single plaintext can encrypt to many different ciphertexts under the same key.

The thesis was awarded First Class Honours (MCIS) in 2016, supervised by Dr Brian Cusack.

---

## The Problem

Standard symmetric ciphers such as AES and the now-deprecated RC4 rely on binary key strings whose security scales with key length. Two problems arise: their mappings are deterministic (one plaintext always produces one ciphertext per key), and their security is threatened by quantum computing — Grover's algorithm reduces a 128-bit AES key to the effective security of 64-bit DES.

The thesis asked whether **graphic-based methods** — drawing on the structures of visual cryptography and matrix theory — could provide higher security with a structurally different key, rather than simply a longer one.

---

## How CME Works

### Key Structure

An *n*-bit CME scheme generates a 2ⁿ × 2ⁿ coordinate matrix — the key. Within this matrix:

- Every possible *n*-bit string (codeword) is assigned to **multiple random coordinate locations**, not just one
- The remaining locations are designated **blank padding entries**, equally distributed and equally numerous as the codeword entries
- A **key string** is derived from the matrix itself (the first x-coordinate for each codeword), requiring no separate key communication

For an 8-bit scheme, this produces a 256 × 256 matrix with 65,536 total coordinate positions.

### Encryption

1. **XOR pre-processing:** the plaintext is combined bit-by-bit with the key string via exclusive-OR, eliminating statistical frequency properties before encryption begins
2. **Coin-toss coordinate selection:** for each unit of plaintext, a PRNG determines whether the next ciphertext segment is:
   - A *message coordinate* — a randomly chosen (x, y) location containing that plaintext codeword, or
   - A *blank padding coordinate* — a randomly chosen empty location
3. Equal numbers of message and padding coordinates are always included, so ciphertext length is fixed at exactly **4x the plaintext length**

The result: each plaintext encryption produces a different ciphertext, because both the coordinate choice (among multiple valid locations per codeword) and the placement of padding are randomised.

### Decryption

For each (x, y) coordinate pair in the ciphertext:
- **Blank entry:** discard
- **Message entry:** recover the codeword, XOR with the key string, append to plaintext

Because decryption is purely lookup-based, its time complexity is lower than encryption in practice.

---

## Security Properties

CME was evaluated against AES-128, RC4-128, ECDH-192, and a binary-adapted 2-out-of-2 Visual Cryptography scheme across brute-force resistance, avalanche effect, and frequency analysis.

### Brute-force resistance

The number of possible key matrices for an 8-bit CME scheme is astronomically large — this is **157,899 orders of magnitude** greater than the number of possible 128-bit AES keys, and equally greater than 128-bit RC4. Even under Grover's quantum algorithm — which halves the effective key size of any symmetric cipher — the 8-bit CME scheme retains a security level far exceeding the post-quantum security of AES-256.

### Avalanche effect

After a single-bit change in plaintext:

| Algorithm | % ciphertext bytes in same position |
|---|---|
| **CME (8-bit)** | **< 1%** |
| AES-128 | 25–49% |
| RC4-128 | 97–99.9% |

CME's avalanche effect matches that of the Visual Cryptography scheme it draws on theoretically, where a 1-bit plaintext change produces approximately 50% change in the binary ciphertext.

### Frequency analysis

Over 1,000 encryptions of an 8,814-bit English-language plaintext string, the highest frequency occurrence of any byte in AES and RC4 ciphertext was 16. In CME, the maximum was **3** — and blank and full coordinates appeared at statistically indistinguishable frequencies. An adversary cannot determine, for any given coordinate pair, whether it carries a message character or padding.

### Non-singular mapping

Because each plaintext codeword maps to multiple coordinate locations, and padding is interleaved at random positions, there is no deterministic relationship between plaintext position and ciphertext position. This provides theoretical resistance to known and chosen plaintext attacks: even complete knowledge of the plaintext gives no information about which coordinates in the ciphertext are message versus padding.

---

## Efficiency

CME was implemented in Java and benchmarked against the same algorithms. Setup time was faster than AES-128 and RC4, with lower memory requirements in both setup and during encryption/decryption:

| Metric | RC4 | CME (8-bit) |
|---|---|---|
| Setup time (ms) | 258.5 | **80.13** |
| Setup memory (MB) | 2.340 | **1.217** |
| Encrypt memory (MB) | 1.362–1.366 | **1.244–1.263** |

Encryption time scales linearly with plaintext length O(n), as expected for a stream cipher. RC4 was faster in raw encryption time; decryption in CME was consistently faster than AES across all tested data sizes. The primary overhead is the generation and storage of the key matrix — a one-time cost at setup.

---

## Theoretical Significance

CME sits at the intersection of three areas:

**Visual Cryptography** — the coin-toss mechanism for deciding whether a ciphertext segment is message or padding is directly derived from the VC model of choosing pixel values. The binomial probability that an adversary correctly identifies all full coordinates in a ciphertext is computationally negligible, mirroring VC's definition of perfect secrecy.

**Error-correcting codes** — the use of binary codewords of fixed bit length as the cipher's alphabet, and the concept of a sparse matrix with designated codeword positions, draws directly from the structure of error-correcting code theory.

**Post-quantum cryptography** — the matrix-based key structure does not rely on number-theoretic hardness assumptions, unlike RSA or ECC which are broken by Shor's algorithm on a quantum computer. The key space of the coordinate matrix remains resistant even under Grover's algorithm.

---

## Limitations and Future Directions

The thesis identified several directions for further research:

- **Optimisation** for low-level hardware or embedded deployment, reducing the overhead of the key matrix generation for use in IoT and smart-card contexts
- **Multiple key strings** — the matrix structure allows for additional key strings without communicating extra keys, each adding an XOR layer to encryption
- **Comparison with ESTREAM portfolio ciphers** such as HC-128, to situate CME within the current stream cipher landscape as a potential successor to RC4
- **TLS integration** — as a candidate for stream-cipher-based protocols currently dependent on RC4
- **Non-singular mapping** — further theoretical work on the class of ciphers that produce many-to-one plaintext-ciphertext mappings, particularly in graphic-based systems

---

## Publications

- Dunmore, A. (2016). *Using Graphic Based Systems to Improve Cryptographic Algorithms*. Master's Thesis, Auckland University of Technology. [[ResearchGate]](https://www.researchgate.net/publication/363478148)
- Cusack, B., & Dunmore, A. (2016). Using Graphic Methods to Challenge Cryptographic Performance. *Australian Information Security Management Conference (AISM)*. [[ResearchGate]](https://www.researchgate.net/publication/363478221)
- Dunmore, A., & Cusack, B. (2016). Challenging Cryptographic Performance. *Digital Forensics Magazine*, Issue 29. [[ResearchGate]](https://www.researchgate.net/publication/363475919)
