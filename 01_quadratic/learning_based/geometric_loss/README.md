# Quadratic Curve Recovery – Geometric Loss

This experiment investigates whether a convolutional neural network can recover the geometry of a quadratic implicit curve directly from its rendered image.

The target equation has the form

$$
ax^2 + bxy + cy^2 + dx + ey + f = 0.
$$

Unlike the coefficient-loss baseline, supervision is provided through sampled points on the curve rather than through the ground-truth coefficients themselves.

---

## Dataset Generation

Synthetic data is generated as follows:

- Random quadratic coefficients sampled from uniform distributions
- Degenerate quadratic parts rejected
- L2 normalization applied to remove scale ambiguity
- Only curves that genuinely intersect the evaluation grid are retained
- Curves rendered as contour plots (224×224 RGB images)
- 256 points sampled from each curve and stored for geometric supervision

During training, a random subset of the stored points is selected for loss computation.

Each sample therefore consists of:

- A curve image
- A collection of sampled points lying on the corresponding curve

---

## Model Architecture

The model architecture is intentionally kept identical to the coefficient-loss baseline:

- 5 convolutional blocks (Conv → Conv → MaxPool)
- Global Average Pooling
- Dense regression head (6 outputs)

The network predicts the six normalized coefficients, but they are optimized using a geometric objective rather than direct coefficient regression.

---

## Loss Function

For each sampled point $(x_i, y_i)$ on the ground-truth curve, the predicted implicit polynomial $F(x, y)$ is evaluated and normalized by the magnitude of its gradient:

$$
L=\frac{1}{N}\sum_{i=1}^{N}
\frac{|F(x_i,y_i)|}{\|\nabla F(x_i,y_i)\|+\varepsilon}.
$$

This objective approximates the geometric distance between the sampled points and the predicted curve.

Unlike coefficient-based losses, it is naturally invariant to the sign ambiguity of implicit equations:

$$
F(x,y)=0
\equiv
-F(x,y)=0.
$$

---

## Training Setup

- Optimizer: Adam (1e-4)
- Early stopping on validation loss
- Train/validation split: 80/20
- Dataset size: 5000 synthetic samples
- Random subsets of stored curve points sampled during training

---

## Evaluation

Evaluation is performed on 100 newly generated random curves.

- The network predicts the implicit polynomial.
- Sampled points from the true curve are used to compute the mean normalized geometric error.
- Best-case and worst-case examples are visualized.

Unlike the coefficient-loss baseline, evaluation focuses on geometric agreement rather than coefficient similarity.

---

## Observations

### Strengths

- Directly optimizes geometric consistency
- Naturally handles sign ambiguity
- Does not rely on coefficient-space distances, which may not reflect visual similarity

### Limitations

- Pure geometric supervision is more difficult to optimize than direct coefficient regression
- Local geometric agreement does not necessarily enforce global structural correctness
- Preliminary experiments indicate that coefficient-based supervision often converges faster and produces stronger qualitative results for quadratic curves

---

## Research Direction

This experiment serves as an intermediate step toward topology-aware learning.

Future work will combine geometric supervision with additional structural constraints, including positive/negative point classification and hybrid loss formulations.

The ultimate goal is to jointly preserve:

- Geometric fidelity
- Global curve structure
- Topological correctness

while extending the methodology to higher-degree implicit algebraic curves.

---

## Example Results

### Successful Recovery

Ground truth (black) vs predicted curve (red):

![Success](assets/example_success_overlay.png)

---

### Failure Case

Example exhibiting the largest geometric error on the evaluation set:

![Failure](assets/example_failure_overlay.png)
