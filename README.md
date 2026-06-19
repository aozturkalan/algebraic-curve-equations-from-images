# algebraic-curve-equations-from-images

## Overview

This repository investigates the recovery of implicit algebraic curve equations directly from visual representations of planar curves.

Given an image of a curve, the objective is to reconstruct a polynomial equation

$F(x,y)=0$

where $F(x,y)$ is a polynomial of degree $d$.

The project is organized primarily by polynomial degree and secondarily by reconstruction paradigm.

The long-term goal is not to produce a single optimal model, but to understand how curve degree, topology, representation, and reconstruction strategy affect recoverability from images.

---

## Core Research Question

The central question of this project is:

> Given an image of an algebraic curve, what is the most reliable way to recover its implicit polynomial equation?

This question is studied through three fundamentally different paradigms:

1. Learning-Based Reconstruction
2. Analytical Reconstruction
3. Hybrid Reconstruction

and across increasing polynomial degrees.

---

## Project Structure

```text
algebraic-curve-equations-from-images/

├── quadratic/
│
│   ├── quadratic_dataset/
│   ├── learning_based/
│   │   ├── coefficient_loss/
│   │   ├── geometric_loss/
│   │   └── topology_aware_loss/
│   │
│   ├── analytical_methods/
│   │   ├── exact_reconstruction/
│   │   ├── overdetermined_fit/
│   │   ├── random_sampling/
│   │   └── ransac/
│   │
│   └── hybrid_methods/
│       ├── coefficient_initialized/
│       ├── candidate_ranking/
│       └── topology_guided/
│
├── cubic/
│
│   ├── learning_based/
│   ├── analytical_methods/
│   └── hybrid_methods/
│
├── quartic/
│
│   ├── learning_based/
│   ├── analytical_methods/
│   └── hybrid_methods/
│
└── higher_degree/
```

---

# Reconstruction Paradigms

## 1. Learning-Based Methods

Learning-based methods use neural networks to predict polynomial coefficients directly from rendered curve images.

Typical experiments include:

- Coefficient-space regression
- Geometric loss formulations
- Topology-aware losses

Example workflow:

```text
Image
 ↓
CNN
 ↓
Polynomial coefficients
```

The primary objective is to understand what geometric and algebraic information can be learned directly from visual data.

---

## 2. Analytical Methods

Analytical methods reconstruct equations directly from sampled curve geometry without machine learning.

These approaches rely on classical algebraic reconstruction techniques.

### Exact Reconstruction

Recover coefficients from the minimum number of points required to determine the polynomial.

Examples:

- Quadratic: 5 points
- Cubic: 9 points

Advantages:

- Exact mathematical formulation
- No training required

Limitations:

- Extremely sensitive to noise

---

### Overdetermined Fit

Use substantially more points than the theoretical minimum and solve an overdetermined system using least-squares or SVD.

Advantages:

- More stable than exact reconstruction

Limitations:

- Sensitive to outliers

---

### Random Sampling

Generate multiple candidate equations from randomly selected minimal point subsets.

Example:

```text
95 sampled points

↓

Random 9-point subsets

↓

Multiple candidate cubics
```

The resulting candidates can then be evaluated and ranked.

---

### RANSAC

Random Sample Consensus (RANSAC) extends random sampling by evaluating each candidate against the entire point set.

Example:

```text
Random subset

↓

Candidate polynomial

↓

Evaluate all points

↓

Count inliers

↓

Select best candidate
```

Advantages:

- Robust to noise
- Robust to outliers
- Naturally handles imperfect point detection

---

## 3. Hybrid Methods

Hybrid methods combine learning-based and analytical approaches.

The goal is to combine:

- Global visual understanding from neural networks
- Local algebraic precision from analytical reconstruction

### Coefficient-Initialized Hybrid

```text
Image
 ↓
CNN
 ↓
Initial coefficients
 ↓
Analytical refinement
 ↓
Final equation
```

### Candidate Ranking Hybrid

```text
Curve points
 ↓
Many analytical candidates
 ↓
CNN ranking
 ↓
Best candidate
```

In this setup the neural network does not generate equations.

Instead, it evaluates and ranks analytical candidates.

### Topology-Guided Hybrid

```text
Image
 ↓
Topological prediction
 ↓
Candidate filtering
 ↓
Analytical reconstruction
 ↓
Final equation
```

Topology information may include:

- Number of connected components
- Number of intersections
- Presence of closed loops

This approach is motivated by the principle:

> A topologically incorrect reconstruction should be considered a failure, even if it is geometrically close.

---

## Mathematical Setting

An algebraic curve of degree d is represented as

$F(x,y)=\sum_{i+j\le d} a_{ij}x^iy^j$

The coefficient vector is inherently scale-invariant:

$\theta \sim k\theta$

for any nonzero scalar $k$.

To remove this ambiguity, coefficient vectors are normalized.

Many experiments additionally account for sign ambiguity:

$\theta \sim -\theta$

through sign-invariant formulations.

---

## Evaluation Philosophy

The repository does not assume that coefficient accuracy alone is sufficient.

Multiple evaluation criteria are considered.

### Coefficient Error

Measures agreement between predicted and true coefficient vectors.

### Geometric Error

Measures how closely the reconstructed curve matches the target geometry.

### Topological Accuracy

Measures preservation of:

- Connected components
- Intersections
- Closed loops

### Component Accuracy

Measures whether the correct number of curve components is recovered.

This metric becomes increasingly important for higher-degree curves.

---

## Research Questions

The repository investigates:

- How reliably can CNNs recover implicit polynomial structure?
- When is coefficient-space supervision sufficient?
- When is geometric supervision necessary?
- How do analytical and learning-based approaches compare?
- How does polynomial degree affect recoverability?
- Which topological properties are easiest or hardest to recover?
- Can hybrid methods outperform either paradigm alone?
- Does the advantage of hybrid methods increase with polynomial degree?

---

## Current Status

### Quadratic

Completed:

- Synthetic dataset generation
- Coefficient-loss experiments
- Baseline evaluation

In Progress:

- Geometric-loss experiments
- Analytical reconstruction methods
- Comparative evaluation

### Cubic

Completed:

- Dataset generation
- Initial learning-based experiments

Under Investigation:

- Geometry-aware losses
- Topology-aware formulations
- Analytical reconstruction

### Quartic and Higher Degrees

Planned future work.

---

## Long-Term Objective

The long-term objective is to understand how implicit algebraic structure can be recovered from visual information and to identify the regimes in which:

- Learning-based methods
- Analytical methods
- Hybrid methods

are most effective.

Rather than searching for a single universal solution, the project aims to develop a systematic understanding of algebraic curve recovery across increasing geometric and topological complexity.

## Author

Aysegul Ozturkalan
