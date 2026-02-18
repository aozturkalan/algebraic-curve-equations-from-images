# algebraic-curve-equations-from-images

## Overview

This repository investigates the problem of recovering implicit algebraic curve equations directly from visual input.

Given an image of a planar curve, the objective is to reconstruct its polynomial equation:

$F(x,y) = 0$

where $F(x,y)$ is a polynomial of degree $d$.

The project explores two fundamentally different paradigms:

1. Learning-based equation recovery (Generating Equations)
2. Classical algebraic reconstruction (Classical Ways)

The long-term goal is to systematically study how curve degree, representation, and loss formulation affect recoverability from images.

---
## Project Structure

```text
algebraic-curve-equations-from-images/

├── generating_equations/
│   ├── quadratic/
│   ├── cubic/
│   ├── quartic/
│   └── higher_degree/
│
└── classical_ways/
    ├── quadratic/
    └── cubic/
```

---

## 1️⃣ Generating Equations (Learning-Based)

This approach uses neural networks to directly regress polynomial coefficients from rendered curve images.

### Key Ideas

- Synthetic dataset generation  
- Implicit polynomial representation  
- Scale normalization (L2 normalization)  
- Sign-invariant loss handling  
- Comparison of different loss formulations:
  - Coefficient loss  
  - Geometric loss  
  - Hybrid losses  

Each degree (quadratic, cubic, etc.) may include multiple experimental setups exploring different training strategies.

The objective is not to provide a single “best version,” but to compare modeling choices under controlled synthetic conditions.

---

## 2️⃣ Classical Ways (Analytical Reconstruction)

This branch investigates reconstruction without machine learning.

Example workflow:

- Detect sample points on the curve  
- Solve linear systems for polynomial coefficients  
- For example, for cubic curves: solve a 10-unknown system from 10 detected points  

This allows direct comparison between:

- Algebraic reconstruction from sampled geometry  
- Learned coefficient regression from image features  

---

## Mathematical Setting

An algebraic curve of degree d in two variables is:

$F(x,y) = \sum a_{ij} x^i y^j$  ( for $i + j ≤ d$)

Coefficient vectors are inherently scale-invariant:

$\theta \sim kθ$

To remove ambiguity, normalization is applied during dataset generation.

Some experiments additionally enforce:

$θ \sim -θ$

via sign-invariant loss functions.

---

## Research Questions

This repository explores:

- How reliably can CNNs recover implicit polynomial structure?
- How does degree affect learnability?
- Is coefficient-space loss sufficient?
- When is geometric loss necessary?
- How do learned and classical approaches compare?

---

## Current Status

- Quadratic models implemented and tested  
- Coefficient-based and alternative loss strategies explored  
- Initial experiments show strong recovery for conic sections  
- Failure cases motivate discriminant-aware and geometry-aware losses  

Future expansions include cubic and higher-degree curves.

---

## Requirements

- Python 3.9+
- TensorFlow 2.x
- NumPy
- Matplotlib
- scikit-learn

---

## Philosophy of the Repository

This is a research-oriented experimental environment.

Different modeling approaches are preserved side-by-side rather than versioned, enabling structured comparison across:

- Degrees  
- Loss functions  
- Reconstruction paradigms  

---

## Author

Aysegul Ozturkalan
