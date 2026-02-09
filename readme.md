# Simple idea

## What we are checking (concepts)

We generate 2D points that lie on a rotated ellipse (plus small noise) and verify:

- how a point cloud becomes a **covariance matrix**
- how **eigenvectors of the covariance matrix** describe the dominant directions (axes) of that cloud
- how **eigenvalues** quantify variance along those axes

In other words: covariance captures a linear summary of the cloud shape, and eigendecomposition turns it into “principal axes”.

## Data: ellipse → point cloud

Parametric ellipse (before rotation) in \(\mathbb{R}^2\):

\[
\begin{bmatrix} x(t) \\ y(t) \end{bmatrix}
=
\begin{bmatrix} a\cos t \\ b\sin t \end{bmatrix}, \quad t \sim \mathrm{Unif}(0, 2\pi)
\]

Rotation by angle \(\theta\):

\[
R(\theta)=
\begin{bmatrix}
\cos\theta & -\sin\theta \\
\sin\theta & \cos\theta
\end{bmatrix},
\quad
\mathbf{p}(t)=R(\theta)\begin{bmatrix} a\cos t \\ b\sin t \end{bmatrix} + \boldsymbol{\varepsilon}
\]

where \(\boldsymbol{\varepsilon}\) is small isotropic noise.

## Point cloud → covariance matrix

Let the dataset be \(\{\mathbf{x}_i\}_{i=1}^n\), \(\mathbf{x}_i \in \mathbb{R}^2\).
First compute the mean:

\[
\boldsymbol{\mu}=\frac{1}{n}\sum_{i=1}^n \mathbf{x}_i
\]

Center the data:

\[
\mathbf{z}_i = \mathbf{x}_i - \boldsymbol{\mu}
\]

Stack centered points into a matrix \(Z \in \mathbb{R}^{2 \times n}\) (each column is \(\mathbf{z}_i\)).
Then the sample covariance matrix is:

\[
\Sigma = \frac{1}{n-1}ZZ^\top
=
\begin{bmatrix}
\mathrm{Var}(x) & \mathrm{Cov}(x,y)\\
\mathrm{Cov}(x,y) & \mathrm{Var}(y)
\end{bmatrix}
\]

## Covariance → eigenvectors (principal directions)

We compute the eigendecomposition:

\[
\Sigma \mathbf{v}_k = \lambda_k \mathbf{v}_k,\quad k\in\{1,2\}
\]

For a real symmetric \(\Sigma\), eigenvectors are orthonormal and eigenvalues are real.
Sorted \(\lambda_1 \ge \lambda_2 \ge 0\):

- \(\mathbf{v}_1\) is the direction with **maximum variance**
- \(\mathbf{v}_2\) is orthogonal to \(\mathbf{v}_1\) and has the **remaining variance**

## Why eigenvectors align with the ellipse axes

Any unit direction \(\mathbf{u}\) has projected variance:

\[
\mathrm{Var}(\mathbf{u}^\top \mathbf{x}) = \mathbf{u}^\top \Sigma \mathbf{u}
\]

Maximizing \(\mathbf{u}^\top \Sigma \mathbf{u}\) under \(\|\mathbf{u}\|=1\) yields \(\mathbf{u}=\mathbf{v}_1\) (Rayleigh quotient), and the maximum value is \(\lambda_1\).
So the eigenvectors give the directions of maximal/minimal spread of the cloud — visually the ellipse’s axes.

In the notebook we also draw an ellipse implied by \(\Sigma\):

\[
\{\ \boldsymbol{\mu} + V \,\mathrm{diag}(\sqrt{\lambda_1}, \sqrt{\lambda_2})\, \mathbf{s}
\ :\ \mathbf{s} \in \mathbb{R}^2,\ \|\mathbf{s}\|=1\ \}
\]

where \(V=[\mathbf{v}_1\ \mathbf{v}_2]\). This ellipse shares the same axes directions \(\mathbf{v}_1,\mathbf{v}_2\).