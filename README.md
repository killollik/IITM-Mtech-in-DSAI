# Mid-Semester Exam Syllabus

This document outlines the core concepts, algorithms, and theoretical foundations covered for the mid-semester examination.

---

## Module 1: Foundations of Unsupervised Learning and Representation

### 1.1. Goal of Unsupervised Learning
- **Core Task:** To find meaningful patterns and structure in unlabeled data.
- **Representation Learning:** The process of transforming raw data into a more useful representation.
- **Theme:** *Comprehension is Compression* — understanding data by finding a simpler, more compact representation.

### 1.2. Data Compression
- **Concept:** Representing a dataset `{x₁, ..., xₙ}` in a compressed format using a **Representation** (basis vectors) and **Coefficients** (coordinates).
- **Geometric View:** Projecting high-dimensional data onto a lower-dimensional subspace.

---

## Module 2: Principal Component Analysis (PCA)

### 2.1. Motivation and Core Duality
- **Goal:** To find the subspace that minimizes the reconstruction error.
- **PCA Duality:** The proof that **Minimizing Reconstruction Error** is equivalent to **Maximizing the Variance** of the projected data.
  - *Key Derivation:* Based on the Pythagorean theorem: `||xᵢ||² = ||xᵢ - proj||² + ||proj||²`.

### 2.2. Mathematical Formulation
- **Objective:** `max (over w) Σᵢ (xᵢᵀw)²` subject to `||w||² = 1`.
- **Solution:** The optimal direction `w` (the first principal component) is the eigenvector of the **Covariance Matrix `C`** corresponding to the largest eigenvalue.

### 2.3. The PCA Algorithm
1. Center the data by subtracting the mean.
2. Find the first principal component `w₁` (top eigenvector of `C`).
3. Compute **residues** by subtracting the projected data: `x' = x - (xᵀw₁)w₁`.
4. Find subsequent components (`w₂, w₃, ...`) by repeating the process on the residues.

### 2.4. Properties and Interpretation
- **Principal Components:** Form an orthonormal basis.
- **Eigenvalues:** Represent the amount of variance captured by each component.
- **Dimensionality Reduction:** Using the top `k` components for a compressed representation.
- **Applications:** Image compression, face recognition (Eigenfaces), and signal vs. noise separation.

---

## Module 3: Kernel PCA

### 3.1. Limitations of Standard PCA
- **Computational (`d >> n` problem):** Infeasible to form and diagonalize the `d x d` covariance matrix `C` when features (`d`) vastly outnumber samples (`n`).
- **Modeling (Non-linearity):** PCA fails to capture non-linear relationships in data.

### 3.2. The "Dual" Formulation of PCA
- **Goal:** To solve the `d >> n` problem.
- **Key Insight:** The eigenvectors `w` of `C = (1/n)XXᵀ` can be expressed as a linear combination of data points `w = Xα`.
- **Procedure:** Solve the eigenvalue problem for the `n x n` Gram matrix `K = XᵀX` to find `α`, then recover `w`.
- **Advantage:** Reduces complexity from `O(d³)` to `O(n³)`.

### 3.3. The Kernel Trick
- **Goal:** To address non-linear structures.
- **Core Idea:** Map data `x` to a higher-dimensional feature space `φ(x)` where it becomes linearly separable.
- **The "Trick":** Replace the dot product `φ(xᵢ)ᵀφ(xⱼ)` with a **kernel function `k(xᵢ, xⱼ)`**, avoiding explicit computation of `φ`.

### 3.4. Kernel PCA Algorithm
1. Choose a kernel function (e.g., Polynomial, Gaussian/RBF).
2. Construct the `n x n` Kernel Matrix `K`.
3. Find the eigenvectors `α` of `K`.
4. The new representation is the projection of the data onto the principal components in the feature space.
- **Mercer's Theorem:** Defines the conditions for a function to be a valid kernel.

---

## Module 4: Clustering

### 4.1. The Clustering Problem
- **Goal:** Partition a dataset into `K` groups.
- **K-Means Objective:** Minimize the sum of squared Euclidean distances from each point to its assigned cluster's mean: `min Σᵢ ||xᵢ - μ_{zᵢ}||²`.
- **Complexity:** This problem is NP-HARD.

### 4.2. Lloyd's Algorithm (K-Means)
- An iterative heuristic to find a local minimum for the K-Means objective.
- **Steps:**
  1. **Initialization:** Start with an initial guess for the cluster centroids.
  2. **Reassignment:** Assign each point to its closest centroid.
  3. **Update:** Recalculate centroids as the mean of assigned points.
- **Convergence:** The algorithm is guaranteed to converge.

### 4.3. Properties and Limitations of K-Means
- **Cluster Shape:** Partitions space into convex **Voronoi cells** with linear boundaries.
- **Limitations:** Fails on non-globular clusters and is sensitive to initialization.
- **Practical Issues:** How to choose `K` and initialize centroids.

### 4.4. Improving K-Means
- **K-Means++:** A smart, probabilistic initialization method.
- **Choosing K:** Using model selection criteria like **AIC** or **BIC** to balance fit and complexity.

---

## Module 5: Spectral Clustering (Kernel K-Means)

### 5.1. Motivation
- To overcome the linear limitations of K-Means by applying the kernel trick.

### 5.2. Mathematical Derivation and Relaxation
- The K-Means objective is rewritten in a high-dimensional feature space and expressed in terms of the kernel matrix `K`.
- **Relaxation:** The discrete assignment problem is relaxed into a continuous one: `maximize trace(Hᵀ K H)`.
- **Connection to PCA:** This relaxed objective is mathematically identical to the Kernel PCA problem.
- **Solution:** The optimal embedding `H` is formed by the top `K` eigenvectors of the kernel matrix `K`.

### 5.3. The Spectral Clustering Algorithm
1. Construct a similarity (kernel) matrix `K`.
2. Compute the top `K` eigenvectors to form the embedding matrix `H`.
3. **Crucial Step:** Normalize the rows of `H`.
4. Run standard K-Means on the `n` normalized row vectors.

---

## Module 6: Parameter Estimation and Maximum Likelihood (MLE)

### 6.1. The Probabilistic Framework
- **Core Assumption:** Data is generated by a probabilistic model with unknown parameters.
- **i.i.d. Assumption:** Data points are *independent and identically distributed*.

### 6.2. The Principle of Maximum Likelihood
- **Likelihood Function `L(parameters | data)`:** The probability of observing the collected data given a choice of parameters.
- **Goal:** Find the parameters that maximize this likelihood.
- **Log-Likelihood:** Used for mathematical convenience, transforming products into sums.

### 6.3. Key Derivations
- **Bernoulli MLE:** Deriving the estimator for a coin flip probability `p`.
- **Gaussian MLE:** Deriving the estimator for the mean `μ`.
- **Connection to K-Means:** Proving that the K-Means objective is the MLE under a Gaussian Mixture Model assumption.
