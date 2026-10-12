# Linear Algebra — Interview

Answers cover the published question list.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Linear Algebra — Interview](./7_algo_linear_algebra_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Linear Algebra](https://devinterview.io/questions/machine-learning-and-data-science/linear-algebra-interview-questions/).

### Linear Algebra Fundamentals

<details>
<summary>What is a vector and how is it used in machine learning?</summary>

### What is a vector and how is it used in machine learning?

**Definition:**

A vector is an ordered list of numbers. In machine learning it is the usual encoding of one example, one weight column, or one direction in feature space.

**How it works:**

- The length of the list is the dimension. Two vectors add only when they have the same dimension, component by component.
- A feature vector might be `[age, income, clicks]`. A word or a user can be an embedding, which is a learned vector whose direction captures similarity.
- Model weights are vectors (or columns of a matrix). A prediction such as a linear score is a dot product of weights and features.
- Geometry still applies: length, angle, and distance between vectors are how nearest-neighbor and cosine search compare items.

**Examples:**

Two shoppers `[1, 0]` and `[0.9, 0.1]` sit near each other in a 2-d feature plane. A third shopper `[0, 1]` points elsewhere, so a nearest-neighbor lookup returns the first pair.
</details>

<details>
<summary>Explain the difference between a scalar and a vector.</summary>

### Explain the difference between a scalar and a vector.

**Definition:**

A scalar is a single number. A vector is an ordered collection of numbers with a dimension greater than one in the usual machine-learning usage, or at least a one-axis array.

**How it works:**

- Scalars have magnitude only. Vectors have magnitude and, once a basis is fixed, a direction.
- Adding a scalar to a vector is not a vector-space operation; broadcasting in code repeats the scalar across components, which is a different convention.
- A 1×1 matrix holds one scalar. A vector of length 1 is still a one-dimensional array, not a bare number, in libraries that track rank.
- Losses and learning rates are scalars. Parameters and activations of a layer are vectors or higher-rank arrays.

**Examples:**

`3` is a scalar. `[3, 0, -1]` is a vector. Scaling that vector by the scalar `2` yields `[6, 0, -2]`.
</details>

<details>
<summary>What is a matrix and why is it central to linear algebra?</summary>

### What is a matrix and why is it central to linear algebra?

**Definition:**

A matrix is a rectangular table of numbers. It represents a linear map, a batch of vectors, or a table of data.

**How it works:**

- Shape `m × n` means m rows and n columns. Applying the matrix to a vector of length n produces a vector of length m.
- In a dataset, one row is often one example and one column one feature. Weights of a linear layer are a matrix.
- Matrix multiplication composes two linear maps. That is why a stack of layers is a product of weight matrices plus nonlinearities between them.
- Special shapes — square, diagonal, triangular, symmetric — make inversion, solving, and eigenvalues cheaper or even possible.

**Examples:**

A 2×3 matrix times a feature vector of length 3 is a 2-d score. Two outputs can be 'spam' and 'not spam' logits before a softmax.
</details>

<details>
<summary>Explain the concept of a tensor in the context of machine learning.</summary>

### Explain the concept of a tensor in the context of machine learning.

**Definition:**

In machine learning a tensor is an array with any number of axes. A scalar is rank 0, a vector rank 1, a matrix rank 2, and an image batch is higher.

**How it works:**

- Rank is the number of axes, not the same word as matrix rank. Shape lists the size of each axis.
- A color image is often rank 3: height, width, channels. A minibatch of images is rank 4: batch, height, width, channels.
- Frameworks store tensors in one contiguous buffer and apply broadcasting rules when shapes differ by size-1 axes.
- The linear-algebra operations still apply on two of the axes: a batched matmul contracts one pair of dimensions and leaves the batch axes alone.

**Examples:**

One RGB photo of 32×32 is shape `(32, 32, 3)`. A training step on 64 photos is shape `(64, 32, 32, 3)`.
</details>

<details>
<summary>How do you perform matrix addition and subtraction?</summary>

### How do you perform matrix addition and subtraction?

**Definition:**

Addition and subtraction are element by element. Both matrices must have the same shape.

**How it works:**

- The entry in row i, column j of the sum is the sum of those two entries. Subtraction is the same with a minus.
- The operations are commutative and associative. Adding a zero matrix changes nothing.
- A shape mismatch is undefined in linear algebra. Code that appears to add a row vector is broadcasting, not matrix addition.
- Cost is proportional to the number of entries, O(mn) for an m×n matrix.

**Examples:**

```text
[1 2] + [5 6] = [6  8]
[3 4]   [7 8]   [10 12]
```
</details>

<details>
<summary>What are the properties of matrix multiplication?</summary>

### What are the properties of matrix multiplication?

**Definition:**

Matrix multiplication combines rows of the left matrix with columns of the right. The inner dimensions must match, and the order matters.

**How it works:**

- An `m × n` matrix times an `n × p` matrix is `m × p`. The n's are the contracted dimension.
- It is associative and distributive over addition. It is not commutative: `AB` and `BA` can differ or one of them can be undefined.
- Multiplying by the identity leaves a matrix unchanged. A zero row or a dependent row can make the product singular even when factors look harmless.
- The naive algorithm is O(mnp). Implementations use blocked, cache-friendly kernels; the algebraic properties stay the same.

**Examples:**

```text
[1 2] [5 6] = [1*5+2*7  1*6+2*8] = [19 22]
[3 4] [7 8]   [3*5+4*7  3*6+4*8]   [43 50]
```
</details>

<details>
<summary>Define the transpose of a matrix.</summary>

### Define the transpose of a matrix.

**Definition:**

The transpose `Aᵀ` swaps rows and columns. The entry that was in row i, column j moves to row j, column i.

**How it works:**

- An `m × n` matrix becomes `n × m`. Transposing twice returns the original.
- `(A + B)ᵀ = Aᵀ + Bᵀ` and `(AB)ᵀ = Bᵀ Aᵀ`. The order of a product reverses.
- A symmetric matrix equals its transpose. A real orthogonal matrix satisfies `Qᵀ Q = I`.
- In data layouts, transpose is also a stride change. A column-major store and a row-major store disagree until one side is transposed.

**Examples:**

```text
[1 2 3]ᵀ = [1 4]
[4 5 6]     [2 5]
            [3 6]
```
</details>

### Vector and Matrix Operations

<details>
<summary>Explain the dot product of two vectors and its significance in machine learning.</summary>

### Explain the dot product of two vectors and its significance in machine learning.

**Definition:**

The dot product of two equal-length vectors is the sum of component-wise products. It measures how much they point the same way.

**How it works:**

- `a · b = Σ aᵢ bᵢ = ‖a‖ ‖b‖ cos θ`. A positive value is an acute angle, zero is a right angle, negative is obtuse.
- A linear model score is `w · x + b`. Attention weights use scaled dot products between query and key vectors.
- Cosine similarity divides the dot product by the two lengths, so magnitude does not dominate.
- Cost is O(d) in the dimension. A batch of them is a matrix product.

**Examples:**

`[1, 0] · [1, 0] = 1` and `[1, 0] · [0, 1] = 0`. The first pair matches; the second is orthogonal.
</details>

<details>
<summary>What is the cross product of vectors and when is it used?</summary>

### What is the cross product of vectors and when is it used?

**Definition:**

The cross product of two vectors in three dimensions is a third vector perpendicular to both. Its length equals the area of the parallelogram they span.

**How it works:**

- `a × b` is orthogonal to `a` and to `b`. The direction follows the right-hand rule. `a × b = −(b × a)`.
- The magnitude is `‖a‖ ‖b‖ sin θ`. Parallel vectors have cross product zero.
- It is a 3-d operation. In 2-d the analogous quantity is the scalar `aₓ bᵧ − aᵧ bₓ`, the signed area.
- Graphics uses it for face normals and lighting. Physics uses it for torque. Typical tabular machine-learning features do not.

**Examples:**

`[1, 0, 0] × [0, 1, 0] = [0, 0, 1]`. The result is the z-axis, and the parallelogram has area 1.
</details>

<details>
<summary>How do you calculate the norm of a vector and what does it represent?</summary>

### How do you calculate the norm of a vector and what does it represent?

**Definition:**

A norm assigns a length to a vector. The usual Euclidean norm is the square root of the sum of squares.

**How it works:**

- L2: `‖x‖₂ = sqrt(Σ xᵢ²)`. L1: `‖x‖₁ = Σ |xᵢ|`. L∞: the largest absolute component.
- A norm is zero only for the zero vector, scales with `|c|` when the vector is multiplied by `c`, and obeys the triangle inequality.
- Dividing a non-zero vector by its L2 norm produces a unit vector. Cosine similarity is the dot product of two unit vectors.
- L2 penalties in training shrink large weights. L1 penalties drive some weights to exactly zero.

**Examples:**

`[3, 4]` has L2 norm 5 and L1 norm 7. The unit vector in that direction is `[0.6, 0.8]`.
</details>

<details>
<summary>Define the concept of orthogonality in linear algebra.</summary>

### Define the concept of orthogonality in linear algebra.

**Definition:**

Two vectors are orthogonal when their dot product is zero. They meet at a right angle in the Euclidean inner product.

**How it works:**

- A set is orthogonal when every distinct pair has dot product zero. It is orthonormal when each vector also has length 1.
- Orthogonal columns do not share a component, so projections onto them can be computed separately and then added.
- An orthogonal matrix has orthonormal columns, and its transpose is its inverse. It preserves lengths and angles.
- In models, orthogonal initializations and constraints keep directions from collapsing into one another.

**Examples:**

`[1, 0]` and `[0, 2]` are orthogonal because the dot product is 0. They are not orthonormal until the second vector is scaled to `[0, 1]`.
</details>

<details>
<summary>What is the determinant of a matrix and what information does it provide?</summary>

### What is the determinant of a matrix and what information does it provide?

**Definition:**

The determinant is a scalar computed from a square matrix. It is the signed factor by which the linear map scales area or volume.

**How it works:**

- A determinant of zero means the matrix is singular: columns are dependent, the map squashes space, and no inverse exists.
- A negative determinant reverses orientation. Swapping two rows flips the sign. Scaling a row by `c` scales the determinant by `c`.
- `det(AB) = det(A) det(B)` and `det(Aᵀ) = det(A)`.
- For a 2×2 matrix `[[a, b], [c, d]]` the value is `ad − bc`. Larger matrices expand along a row or use a factorization.

**Examples:**

`[[2, 0], [0, 3]]` has determinant 6: areas grow by 6. `[[1, 2], [2, 4]]` has determinant 0: the second row is twice the first, and the map collapses the plane to a line.
</details>

<details>
<summary>Can you explain what an eigenvector and eigenvalue are?</summary>

### Can you explain what an eigenvector and eigenvalue are?

**Definition:**

An eigenvector of a square matrix is a non-zero vector that the matrix only stretches or flips. The eigenvalue is that scale factor.

**How it works:**

- `A v = λ v`. The direction of `v` stays on the same line. `λ` can be negative, which reverses the direction, or complex for a real matrix that rotates.
- Eigenvectors for distinct eigenvalues are independent. A symmetric real matrix has real eigenvalues and orthogonal eigenvectors.
- PCA takes the eigenvectors of the covariance matrix. The largest eigenvalues are the directions where the data spreads the most.
- In a linear dynamical system, `|λ| > 1` grows that mode and `|λ| < 1` shrinks it.

**Examples:**

`[[2, 0], [0, 3]]` sends `[1, 0]` to `[2, 0]`, so the eigenvalue is 2. It sends `[0, 1]` to `[0, 3]`, so the eigenvalue is 3.
</details>

<details>
<summary>How is the trace of a matrix defined and what is its relevance?</summary>

### How is the trace of a matrix defined and what is its relevance?

**Definition:**

The trace is the sum of the diagonal entries of a square matrix. It equals the sum of the eigenvalues, counted with multiplicity.

**How it works:**

- `tr(A) = a₁₁ + a₂₂ + … + aₙₙ`.
- `tr(A + B) = tr(A) + tr(B)` and `tr(cA) = c tr(A)`. `tr(AB) = tr(BA)` when both products exist, even if `A` and `B` are not square of the same shape.
- The trace is invariant under a change of basis: `tr(P⁻¹ A P) = tr(A)`.
- For a covariance or a projection, the trace is a total variance or a dimension count that does not depend on rotating the axes.

**Examples:**

`[[2, 9], [0, 3]]` has trace `2 + 3 = 5`, the same as the sum of eigenvalues 2 and 3. The off-diagonal 9 does not enter the trace.
</details>

### Special Matrices and Their Properties

<details>
<summary>What is a diagonal matrix and how is it used in linear algebra?</summary>

### What is a diagonal matrix and how is it used in linear algebra?

**Definition:**

A diagonal matrix is square and every entry off the main diagonal is zero. Multiplying by it scales each axis and does not mix them.

**How it works:**

- The product of a diagonal matrix and a vector multiplies component i by diagonal entry i. Two diagonal matrices commute, and their product is diagonal.
- The inverse, when every diagonal entry is non-zero, is the diagonal of reciprocals. A zero on the diagonal makes the matrix singular.
- An eigendecomposition writes a diagonalizable matrix as `Q D Q⁻¹`, where `D` holds the eigenvalues. Powers of the matrix become powers of those diagonal entries.
- In optimization and whitening, a diagonal matrix is a cheap way to scale features independently.

**Examples:**

```text
[2 0] [x] = [2x]
[0 3] [y]   [3y]
```
</details>

<details>
<summary>Explain the properties of an identity matrix.</summary>

### Explain the properties of an identity matrix.

**Definition:**

The identity is the square matrix with ones on the main diagonal and zeros elsewhere. numpy.eye(N) builds it. Multiplying by it leaves a compatible matrix or vector unchanged.

**How it works:**

- numpy.eye(N, M=None, k=0) defaults to a square array. k shifts the diagonal. The docs also point at numpy.identity as the almost equivalent function.
- It is the multiplicative identity: A @ I = A and I @ A = A when the shapes match. NumPy's matmul implements @.
- Its determinant is 1, so it is invertible, and it is its own inverse. A zero on that diagonal, from a shifted k, is not an identity.
- It is the matrix of the linear map that does nothing. Any other matrix is a change away from that map.

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.eye.html
https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>Define what a symmetric matrix is.</summary>

### Define what a symmetric matrix is.

**Definition:**

A symmetric matrix equals its transpose: A[i, j] = A[j, i]. It is square. Real symmetric matrices are the ones numpy.linalg.eigh is documented to handle, along with complex Hermitian matrices.

**How it works:**

- eigh returns eigenvalues and eigenvectors and takes UPLO to say whether the lower or upper triangle is stored. eigvalsh returns only the eigenvalues.
- The product of a symmetric matrix with a vector is well defined. Two symmetric matrices need not commute.
- A covariance matrix is symmetric because cov(i, j) = cov(j, i). sklearn's PCA centers the data and then uses an SVD, which is the stable way to get those directions.
- If A is symmetric and every eigenvalue is positive, it is positive definite. eigh is the solver you use for that class, not the general eig.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>What is a unit vector and how do you find it?</summary>

### What is a unit vector and how do you find it?

**Definition:**

A unit vector has length 1. Divide a nonzero vector by its Euclidean norm. numpy.linalg.norm computes that length. The default order is the 2-norm for vectors.

**How it works:**

- v / np.linalg.norm(v) is the unit vector in the same direction. A zero vector has no direction. Dividing by its norm fails.
- linalg.vector_norm is the array-API form of the same length. Keepdims is available when you need the norm to broadcast back onto a batch.
- Two unit vectors have dot product equal to the cosine of the angle between them. That is the comparison embeddings use.
- A column of an orthogonal matrix is a unit vector, and different columns have dot product 0.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>Explain the concept of an orthogonal matrix.</summary>

### Explain the concept of an orthogonal matrix.

**Definition:**

A real square matrix is orthogonal when its columns are orthonormal: Q.T @ Q = I. The inverse is the transpose. numpy.linalg.qr factors a matrix into an orthogonal Q and an upper-triangular R.

**How it works:**

- Preserving length is the geometric fact: ||Q @ v|| = ||v||. Angles between vectors are preserved too.
- qr(a, mode) computes that factorization. The default mode returns both factors. A rectangular A still has an orthogonal factor whose columns are an orthonormal basis for the column space.
- The determinant of a square orthogonal matrix is 1 or -1. A reflection is the -1 case. Both are invertible.
- Do not confuse orthogonal with symmetric. A symmetric matrix equals its transpose. An orthogonal matrix's transpose is its inverse.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>Define positive definiteness of a matrix.</summary>

### Define positive definiteness of a matrix.

**Definition:**

A real symmetric matrix A is positive definite when x.T @ A @ x is positive for every nonzero vector x. Equivalently, every eigenvalue is positive. numpy.linalg.eigh is the documented eigensolver for a real symmetric matrix.

**How it works:**

- Positive semi-definite allows a zero eigenvalue, so some nonzero x gives x.T @ A @ x = 0. A covariance is semi-definite. A sample with more features than rows is a typical reason for the zero.
- Cholesky, numpy.linalg.cholesky, factors a positive definite matrix as L @ L.T with L lower triangular. It fails when the matrix is not positive definite.
- sklearn PCA whiten=True scales components to unit variance after the SVD. That step assumes the kept singular values are positive. A zero component is not a direction you can whiten.
- A matrix that is not symmetric is not called positive definite in this sense. Check symmetry before you read the eigenvalues from eigh.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>What is the rank of a matrix and why is it important?</summary>

### What is the rank of a matrix and why is it important?

**Definition:**

The rank is the number of linearly independent rows, which equals the number of independent columns. numpy.linalg.matrix_rank estimates it from the SVD, with a tolerance.

**How it works:**

- A rank below the smaller dimension means the columns are dependent. The linear map squashes space. numpy.linalg.inv will not invert that matrix. numpy.linalg.solve will not return a unique solution.
- matrix_rank(A, tol=..., hermitian=...) uses the SVD. For a Hermitian matrix, pass hermitian so it can take the faster path the docs allow.
- The numerical rank depends on the tolerance. Tiny singular values from rounding can look nonzero. The tolerance is what decides they do not count.
- sklearn PCA keeps components that explain variance, which are the large singular directions. Dropping the rest is a deliberate rank reduction.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

### Systems of Linear Equations

<details>
<summary>How do you represent a system of linear equations using matrices?</summary>

### How do you represent a system of linear equations using matrices?

**Definition:**

A system of m equations in n unknowns is A @ x = b. Each row of A is the coefficients of one equation. x is the unknown vector. b is the right-hand side. numpy.linalg.solve solves the square case.

**How it works:**

- solve(a, b) requires a square full-rank matrix. It accepts a stack of systems. The linalg page says a shape (..., M, M) is a stack of M-by-M matrices.
- A rectangular or rank-deficient system has no unique solution. numpy.linalg.lstsq returns the least-squares solution and takes rcond.
- The @ operator is matmul. Do not use elementwise * for the system.
- A zero row in A with a nonzero entry in b is an inconsistent equation. No x satisfies it.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>What is the method of Gaussian elimination?</summary>

### What is the method of Gaussian elimination?

**Definition:**

Gaussian elimination clears entries below each pivot, turning A into an upper-triangular matrix, and applies the same row operations to b. Back-substitution then solves the triangle. With row swaps this is the LU factorization.

**How it works:**

- scipy.linalg.lu computes A = P @ L @ U with partial pivoting. L is unit lower triangular, U is upper triangular, P is a permutation.
- The docs prefer permutation indices over a dense P, because P is only a reordering of rows. With p_indices=True the 2-d relation is A = L[P, :] @ U.
- A zero pivot that cannot be swapped away means the matrix is singular. solve will not return a unique x.
- The same elimination computes a determinant as the product of the diagonal of U, with a sign from the permutation. numpy.linalg.det is the library call. It does not show the steps.

**Sources:**

https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.lu.html
https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>Explain the concept of linear dependence and independence.</summary>

### Explain the concept of linear dependence and independence.

**Definition:**

Vectors are linearly dependent when one of them is a linear combination of the others. They are independent when the only combination that yields the zero vector has all coefficients zero.

**How it works:**

- The columns of A are dependent exactly when there is a nonzero x with A @ x = 0. The rank is then below the column count. matrix_rank reports that count of independent columns via the SVD.
- Two identical columns are dependent. A zero column is dependent on any set.
- Independent columns mean the map A @ x hits a space of that dimension. Dependent columns mean some direction in the input is flattened.
- A square matrix with dependent columns has determinant zero and is not invertible. numpy.linalg.inv raises LinAlgError on that input.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>What is the meaning of the solution space of a system of linear equations?</summary>

### What is the meaning of the solution space of a system of linear equations?

**Definition:**

The solution space is every x that satisfies A @ x = b. For a square full-rank A it is one point, the value numpy.linalg.solve returns. Otherwise it is empty, or a point plus the null space.

**How it works:**

- If A @ x0 = b and A @ v = 0, then A @ (x0 + v) = b. The general solution is one particular solution plus every vector in the null space.
- lstsq returns a least-squares x when no exact solution exists. That x is not a member of an empty exact solution set. It minimizes ||A @ x - b||.
- A homogeneous system A @ x = 0 always has at least the zero solution. The extra solutions are the null space. Its dimension is columns minus rank.
- solve on a stack of right-hand sides returns one solution per column of b when A is invertible.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>Describe the conditions for consistency in linear equations.</summary>

### Describe the conditions for consistency in linear equations.

**Definition:**

A system A @ x = b is consistent when b is a linear combination of the columns of A. Then at least one x exists. numpy.linalg.solve returns that x when A is square and full rank.

**How it works:**

- If b is not in the column space, no x works. A zero row of A paired with a nonzero entry of b is the textbook inconsistent row.
- Rank tells you. The system is consistent when rank(A) equals rank of the matrix formed by appending b as an extra column.
- lstsq still returns an x when the system is inconsistent. That x minimizes the squared residual. It is not an exact solution. The linear-model page states ordinary least squares as minimizing ||X w - y||^2, which is this case.
- A consistent square singular system has infinitely many solutions. solve will not pick one. It raises LinAlgError.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://scikit-learn.org/stable/modules/linear_model.html
</details>

<details>
<summary>Define and differentiate between homogenous and non-homogenous systems.</summary>

### Define and differentiate between homogenous and non-homogenous systems.

**Definition:**

A homogeneous system is A @ x = 0. A non-homogeneous system is A @ x = b with b not the zero vector. The homogeneous system always has the zero solution.

**How it works:**

- Any extra homogeneous solution is a null-space vector. If the columns are independent, the only solution is zero and matrix_rank equals the column count.
- A non-homogeneous system may have no solution. Consistency is whether b is in the column space.
- When a particular solution x0 exists, every other solution is x0 plus a homogeneous solution.
- solve is for a square invertible system, homogeneous or not. The homogeneous answer is then only zero, so you do not call solve to discover the null space.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

### Advanced Matrix Operations

<details>
<summary>How do you compute the inverse of a matrix and when is it possible?</summary>

### How do you compute the inverse of a matrix and when is it possible?

**Definition:**

A square matrix has an inverse when it is full rank. numpy.linalg.inv computes it. The product of the matrix and its inverse is the identity.

**How it works:**

- inv raises LinAlgError, documented on the linalg page, when the matrix is singular. A determinant of zero is that case.
- Prefer solve(A, b) to inv(A) @ b. solve does not form the inverse explicitly and is the function the linalg page lists for a linear system.
- A rectangular matrix has no two-sided inverse. pinv computes the Moore-Penrose pseudo-inverse. lstsq is the call when you want the least-squares x rather than the pseudo-inverse matrix.
- An orthogonal matrix is inverted by transposing it. qr gives you that factor. You do not call inv on Q.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>Explain the LU decomposition of a matrix.</summary>

### Explain the LU decomposition of a matrix.

**Definition:**

LU writes A as a product of a unit lower-triangular matrix and an upper-triangular matrix, after a row permutation. scipy.linalg.lu returns that factorization with partial pivoting: A = P @ L @ U.

**How it works:**

- L has ones on the diagonal. U is upper triangular. P reorders rows so a usable pivot is chosen.
- permute_l=True folds P into L and returns (P@L, U). p_indices=True returns the permutation as row indices. The docs say a dense permutation matrix is costly and indices are preferred. Then A = L[P, :] @ U.
- Solving A @ x = b becomes forward substitution on L and back substitution on U, after applying P to b.
- A singular matrix can still have an LU form with a zero on the diagonal of U. That zero means you cannot divide in back substitution.

**Sources:**

https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.lu.html
</details>

<details>
<summary>What are singular or ill-conditioned matrices?</summary>

### What are singular or ill-conditioned matrices?

**Definition:**

A square matrix is singular when it is not invertible: its rank is short, its determinant is zero, and a nonzero x satisfies A @ x = 0. An ill-conditioned matrix is invertible but numpy.linalg.cond is large, so a small change in b makes a large change in x.

**How it works:**

- cond(x, p) computes the condition number. A value near 1 is stable. A huge value means the singular values span many orders of magnitude.
- inv and solve raise LinAlgError on a singular matrix. They return an answer on an ill-conditioned one, and that answer can be mostly rounding error.
- lstsq takes rcond to decide which tiny singular values to treat as zero. That is how you refuse to invert the noisy directions.
- The linear-model page says that when columns of X are nearly dependent, the least-squares estimate has high variance. That is ill-conditioning of the design matrix.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://scikit-learn.org/stable/modules/linear_model.html
</details>

<details>
<summary>How do you perform QR decomposition?</summary>

### How do you perform QR decomposition?

**Definition:**

QR factors A as Q @ R, with Q orthogonal and R upper triangular. numpy.linalg.qr(a, mode) computes it.

**How it works:**

- The columns of Q are an orthonormal basis for the column space when A has full column rank. R holds the coefficients of the original columns in that basis.
- Solving least squares becomes solving the triangular system from R, after multiplying by Q.T. That avoids forming A.T @ A, which squares the condition number.
- mode selects what is returned. The linalg page lists qr among the decompositions next to SVD and Cholesky.
- A square orthogonal factor has inverse equal to its transpose. You do not call inv on Q.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>What is the Singular Value Decomposition (SVD) and its applications in machine learning?</summary>

### What is the Singular Value Decomposition (SVD) and its applications in machine learning?

**Definition:**

SVD writes a matrix as U @ S @ Vh, with U and Vh orthogonal and S diagonal of non-negative singular values. numpy.linalg.svd computes it. sklearn PCA is this factorization after centering.

**How it works:**

- svd(a, full_matrices, compute_uv) returns the factors. svdvals returns only the singular values. The rank is the count of singular values above a tolerance. matrix_rank uses that.
- PCA centers each feature and does not scale it, then takes the SVD. The components are directions of maximum variance. whiten=True scales them to unit variance.
- Truncating small singular values is dimensionality reduction. The decomposition page says randomized SVD, svd_solver='randomized', approximates the kept components when you will drop the rest, and is cheaper than the full SVD.
- A recommendation matrix of users by items is the same factorization: keep the large singular values and reconstruct. That is a low-rank approximation, which the decomposition page groups under matrix factorization.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>Explain the concept of matrix factorization.</summary>

### Explain the concept of matrix factorization.

**Definition:**

A factorization writes one matrix as a product of simpler matrices. NumPy and SciPy document several: LU, QR, Cholesky, the eigendecomposition, and the SVD.

**How it works:**

- LU is elimination: P @ L @ U. QR is an orthogonal factor times a triangle. Cholesky is L @ L.T for a positive definite matrix. SVD is U @ S @ Vh.
- The point is to replace a hard operation with operations on the factors. A triangular solve is back substitution. An orthogonal factor is inverted by a transpose.
- sklearn's decomposition module is titled as matrix factorization problems. PCA is the SVD of centered data. IncrementalPCA factorizes in batches when the matrix does not fit in memory.
- Not every matrix has every factorization. Cholesky requires positive definite. A full set of eigenvectors requires a diagonalizable matrix. SVD exists for any real matrix.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.lu.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

### Linear Transformations and Mapping

<details>
<summary>What is a linear transformation in linear algebra?</summary>

### What is a linear transformation in linear algebra?

**Definition:**

A linear transformation sends vectors to vectors and respects addition and scaling: T(u + v) = T(u) + T(v) and T(c u) = c T(u). Once bases are fixed, it is multiplication by a matrix.

**How it works:**

- The zero transformation and the identity are linear. numpy.eye is the matrix of the identity. A translation that adds a constant vector is not linear, because it does not send zero to zero.
- Composition of transformations is matrix multiplication, in the order the maps are applied. NumPy's matmul and @ are that product.
- The columns of the matrix are the images of the basis vectors. Read a column to see where that basis vector goes.
- Nonlinear layers in a network sit between these maps. The weight application itself is the linear piece.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://numpy.org/doc/stable/reference/generated/numpy.eye.html
</details>

<details>
<summary>How can you represent linear transformation using a matrix?</summary>

### How can you represent linear transformation using a matrix?

**Definition:**

Pick an input basis and an output basis. The matrix column j is the coordinate vector of the image of the j-th input basis vector. Applying the transformation is the product A @ x.

**How it works:**

- x holds coordinates, not an abstract arrow. Change the basis and both x and A change, even if the geometric map is the same.
- A map from n-dimensional space to m-dimensional space is an m-by-n matrix. matmul requires the inner dimensions to match.
- The identity map in the same basis is numpy.eye. A scaling of each axis is a diagonal matrix.
- A rotation in the plane is an orthogonal matrix with determinant 1. Its inverse is its transpose.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://numpy.org/doc/stable/reference/generated/numpy.eye.html
</details>

<details>
<summary>Describe the kernel and image of a linear transformation.</summary>

### Describe the kernel and image of a linear transformation.

**Definition:**

The kernel (null space) of a matrix A is every vector x with A @ x = 0. The image (range, column space) is every vector that A can produce, the span of the columns.

**How it works:**

- scipy.linalg.null_space builds an orthonormal basis for the kernel with an SVD. Singular values smaller than rcond * max(s) count as zero. The returned array has shape (N, K), and K is the dimension of that effective null space.
- scipy.linalg.orth builds an orthonormal basis for the range the same way. Its width is the effective rank.
- A wide matrix, more columns than rows, always has a nontrivial numerical kernel once the rank is below the column count. null_space on a random 3-by-5 matrix returns two columns, and the product with those columns is numerically zero.
- Roundoff means a computed singular value is rarely a clean zero. The cutoff, not a visual inspection of the entries, decides which directions count as kernel.

**Examples:**

```text
A = [[1, 1],
     [1, 1]]
null_space(A) is a multiple of [1, -1]
A @ [1, -1] = [0, 0]
orth(A) spans [1, 1], the only column direction
```

**Sources:**

https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.null_space.html
https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.orth.html
</details>

<details>
<summary>How does change of basis affect matrix representation of linear transformations?</summary>

### How does change of basis affect matrix representation of linear transformations?

**Definition:**

The matrix of a linear map is tied to the chosen bases. The same map has a different matrix after the coordinates change. In an eigenbasis, when the eigenvalues are all different, that new matrix is diagonal.

**How it works:**

- numpy.linalg.eig returns eigenvalues w and right eigenvectors whose columns v satisfy A @ v = w * v. The columns are scaled to unit length.
- If those eigenvalues are all different, the eigenvectors are linearly independent, and the similarity inv(eigenvectors) @ A @ eigenvectors is diagonal. That diagonal matrix is the same map written in the eigenbasis.
- The eigenvectors array is not guaranteed to have full rank when eigenvalues repeat, so that similarity is not always a diagonalization. For a real symmetric or Hermitian matrix, numpy.linalg.eigh is the routine that keeps an orthonormal basis.
- A non-Hermitian normal matrix is safer through scipy.linalg.schur: the factor is unitary, and the factor is triangular rather than diagonal.

**Examples:**

```text
A = diag(1, 2, 3)
eig(A) returns eigenvalues 1, 2, 3
and the identity as eigenvectors
inv(V) @ A @ V is already diagonal
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.eig.html
</details>

### Coding Challenges

<details>
<summary>Write code to add, subtract, and multiply two matrices without using external libraries.</summary>

### Write code to add, subtract, and multiply two matrices without using external libraries.

**Definition:**

Addition and subtraction touch matching entries. The product entry (i, j) is the dot product of row i of the left matrix with column j of the right matrix. No library call is required for that arithmetic.

**How it works:**

- The two matrices must share a shape for addition and subtraction. The product needs the inner sizes to match: columns of the left matrix, rows of the right matrix.
- Build the product by three nested loops, or by a sum inside the (i, j) slot. The result has the outer shape: rows of the left matrix, columns of the right matrix.
- This product is the operation NumPy writes as @ and as numpy.matmul. Implementing it by hand makes the index rule visible.

**Examples:**

```text
def add(a, b):
    return [[a[i][j] + b[i][j] for j in range(len(a[0]))] for i in range(len(a))]

def sub(a, b):
    return [[a[i][j] - b[i][j] for j in range(len(a[0]))] for i in range(len(a))]

def mul(a, b):
    rows, k, cols = len(a), len(b), len(b[0])
    out = [[0] * cols for _ in range(rows)]
    for i in range(rows):
        for j in range(cols):
            out[i][j] = sum(a[i][t] * b[t][j] for t in range(k))
    return out

A = [[1, 2], [3, 4]]
B = [[5, 6], [7, 8]]
add(A, B) == [[6, 8], [10, 12]]
mul(A, B) == [[19, 22], [43, 50]]
```

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
</details>

<details>
<summary>Implement a function to calculate the transpose of a given matrix.</summary>

### Implement a function to calculate the transpose of a given matrix.

**Definition:**

The transpose swaps rows and columns. The entry that was in row i and column j moves to row j and column i.

**How it works:**

- An m-by-n matrix becomes n-by-m. A square matrix keeps its shape, but the off-diagonal pair (i, j) and (j, i) still swaps.
- In NumPy the same view-or-copy operation is the attribute .T, used for instance when a left eigenvector is written y.T @ A.
- Transposing twice returns the original arrangement of entries.

**Examples:**

```text
def transpose(a):
    return [[a[i][j] for i in range(len(a))] for j in range(len(a[0]))]

transpose([[1, 2, 3], [4, 5, 6]])
== [[1, 4], [2, 5], [3, 6]]
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.eig.html
</details>

<details>
<summary>Code to find the determinant of a matrix using recursion.</summary>

### Code to find the determinant of a matrix using recursion.

**Definition:**

For a 2-by-2 matrix [[a, b], [c, d]] the determinant is ad - bc. A recursive function uses that identity on every 2-by-2 minor and, for a larger matrix, expands along the first row.

**How it works:**

- The 1-by-1 case returns the single entry. The 2-by-2 case returns ad - bc, which is the value numpy.linalg.det reports for [[1, 2], [3, 4]], namely -2.
- Along row 0, entry j contributes with sign (+1) when j is even and (-1) when j is odd, multiplied by the determinant of the matrix that remains after deleting row 0 and column j.
- numpy.linalg.det does not use this recursion. It computes the determinant from an LU factorization (LAPACK dgetrf / zgetrf). For a large matrix, slogdet avoids overflow by returning the sign and the log of the absolute value.

**Examples:**

```text
def det(a):
    n = len(a)
    if n == 1:
        return a[0][0]
    if n == 2:
        return a[0][0] * a[1][1] - a[0][1] * a[1][0]
    total = 0
    for j in range(n):
        minor = [row[:j] + row[j + 1:] for row in a[1:]]
        sign = 1 if j % 2 == 0 else -1
        total += sign * a[0][j] * det(minor)
    return total

det([[1, 2], [3, 4]]) == -2
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.det.html
</details>

<details>
<summary>Develop a Python function to compute the inverse of a matrix.</summary>

### Develop a Python function to compute the inverse of a matrix.

**Definition:**

The inverse of a square full-rank matrix A is the matrix B with A @ B equal to the identity. numpy.linalg.inv computes it. A matrix that is not square, or not full rank, has no inverse.

**How it works:**

- numpy.linalg.solve is the better call when the real task is A @ x = b. Forming the inverse and then multiplying is an extra factorization with worse rounding behavior. solve uses LAPACK gesv and raises LinAlgError when A is singular or not square.
- If the system is rectangular or rank-deficient, numpy.linalg.lstsq returns the least-squares solution instead of an inverse. numpy.linalg.pinv is the pseudoinverse for that case.
- A quick exact check on a 2-by-2 matrix uses the determinant ad - bc. The inverse exists only when that value is not zero.

**Examples:**

```text
import numpy as np

def invert(a):
    return np.linalg.inv(np.asarray(a, dtype=float))

invert([[1, 2], [3, 4]])
# [[-2, 1], [1.5, -0.5]]
# because det = 1*4 - 2*3 = -2
```

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.solve.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.det.html
</details>

<details>
<summary>Write an algorithm to perform eigenvalue and eigenvector decomposition.</summary>

### Write an algorithm to perform eigenvalue and eigenvector decomposition.

**Definition:**

A number w is an eigenvalue of a square matrix A when some nonzero vector v satisfies A @ v = w * v. numpy.linalg.eig returns both, with the eigenvector stored as a column.

**How it works:**

- The call returns a named tuple: eigenvalues of shape (..., M) and eigenvectors of shape (..., M, M). Column eigenvectors[:, i] belongs to eigenvalues[i], and each column has unit length.
- Eigenvalues are not sorted. A real matrix has real eigenvalues, or complex eigenvalues in conjugate pairs. The result stays real when every imaginary part is zero.
- Repeated eigenvalues can leave the eigenvector matrix short of full rank, so A is not always diagonalized by inv(V) @ A @ V. When the eigenvalues are all different, that similarity is diagonal.
- For a real symmetric or complex Hermitian matrix use numpy.linalg.eigh. For a non-Hermitian normal matrix, scipy.linalg.schur is the more stable factorization. eig itself calls LAPACK geev and raises LinAlgError if the iteration does not converge.

**Examples:**

```text
import numpy as np
w, v = np.linalg.eig(np.diag((1, 2, 3)))
# w == [1, 2, 3]
# v is the identity
# diag(1, 2, 3) @ v[:, i] == w[i] * v[:, i]
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.eig.html
</details>

<details>
<summary>Create a Python script to solve a system of linear equations using NumPy.</summary>

### Create a Python script to solve a system of linear equations using NumPy.

**Definition:**

A square full-rank system is the matrix equation A @ x = b. numpy.linalg.solve returns the solution x computed by LAPACK gesv.

**How it works:**

- A must be square and full rank. Otherwise solve raises LinAlgError. A rectangular system, or a rank-deficient square system, belongs to numpy.linalg.lstsq.
- b may be one right-hand side of shape (M,) or several of shape (..., M, K). Since NumPy 2.0, a right-hand side is treated as a single column vector only when it is exactly one-dimensional.
- Check the result with numpy.allclose(A @ x, b). The comparison absorbs the rounding left by the factorization.

**Examples:**

```text
import numpy as np
# x0 + 2 x1 = 1
# 3 x0 + 5 x1 = 2
a = np.array([[1, 2], [3, 5]])
b = np.array([1, 2])
x = np.linalg.solve(a, b)   # [-1, 1]
np.allclose(a @ x, b)
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.solve.html
</details>

<details>
<summary>Implement a function to calculate the L2 norm of a vector.</summary>

### Implement a function to calculate the L2 norm of a vector.

**Definition:**

The L2 norm of a vector is its Euclidean length, the square root of the sum of squares of the entries. numpy.linalg.norm computes it when ord is 2, and also by default for a one-dimensional array.

**How it works:**

- The general vector formula on the norm page is sum(abs(x)**ord)**(1/ord). For ord 2 that is the ordinary square root of the sum of squares.
- If both axis and ord are omitted and the array is two-dimensional, the same function returns the Frobenius norm, which is the square root of the sum of squares of every entry, not the matrix 2-norm. Pass ord=2 to get the largest singular value of a matrix.
- A hand-written function should square the entries, sum them, and take the square root. For a very long vector, numpy.linalg.norm is the one that uses a stable library implementation.

**Examples:**

```text
def l2(x):
    return sum(t * t for t in x) ** 0.5

l2([3, 4]) == 5
# np.linalg.norm([3, 4]) == 5
# np.linalg.norm([3, 4], ord=2) == 5
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.norm.html
</details>

<details>
<summary>Write a program to verify if a given square matrix is orthogonal.</summary>

### Write a program to verify if a given square matrix is orthogonal.

**Definition:**

A real square matrix Q is orthogonal when its columns are orthonormal, which is the same statement as Q.T @ Q equaling the identity. The inverse is then the transpose.

**How it works:**

- Reject a matrix that is not square before any product. Compare Q.T @ Q with numpy.eye(n) through numpy.allclose, so rounding does not fail an exact == test.
- numpy.linalg.qr produces a factor Q with this property for the column space it spans. numpy.linalg.eig does not promise a unitary eigenvector matrix except in the cases where the eigenvalues are distinct and you still have to check the product.
- For a complex matrix the matching test uses the conjugate transpose, Q.conj().T @ Q, because unitary replaces orthogonal.

**Examples:**

```text
import numpy as np

def is_orthogonal(q, tol=1e-8):
    q = np.asarray(q, dtype=float)
    n, m = q.shape
    if n != m:
        return False
    return np.allclose(q.T @ q, np.eye(n), atol=tol)

is_orthogonal([[0, -1], [1, 0]])   # True
# columns [0, 1] and [-1, 0] each have length 1 and dot to 0
```

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://numpy.org/doc/stable/reference/generated/numpy.eye.html
</details>

### Applications of Linear Algebra in Machine Learning

<details>
<summary>How is linear regression related to linear algebra?</summary>

### How is linear regression related to linear algebra?

**Definition:**

Linear regression predicts a target as an intercept plus a dot product of weights and features. Fitting ordinary least squares is the linear-algebra problem of minimizing ||X @ w - y||^2.

**How it works:**

- sklearn stores the feature weights in coef_ and the intercept in intercept_. The prediction is intercept_ + x @ coef_.
- LinearRegression computes that solution from the SVD of X. When there are at least as many samples as features, the cost is on the order of n_samples * n_features^2.
- If some columns of X are nearly linear combinations of the others, X is close to singular and the least-squares weights have high variance. That is multicollinearity.
- Ridge keeps the same residual and adds alpha * ||w||^2. A larger alpha shrinks the weights and makes them more robust to that collinearity. The least-squares problem itself is still a matrix factorization, not a search over an inverse you form by hand.

**Examples:**

```text
X = [[0, 0], [1, 1], [2, 2]]
y = [0, 1, 2]
LinearRegression().fit(X, y)
coef_ is [0.5, 0.5], intercept_ is 0
prediction at [2, 2] is 2
```

**Sources:**

https://scikit-learn.org/stable/modules/linear_model.html
</details>

<details>
<summary>Describe the role of linear algebra in neural network computations.</summary>

### Describe the role of linear algebra in neural network computations.

**Definition:**

The linear part of a dense layer is a matrix-vector product plus a bias. Everything the layer does before the pointwise nonlinearity is that product.

**How it works:**

- For one sample, the pre-activation is W @ x + b. W has one column per input coordinate and one row per output coordinate, the same shape rule as any linear map.
- A minibatch is a stack of vectors. numpy.matmul applies the same W to every vector when the batch axis follows the linalg broadcasting rules.
- A pointwise function after the product, such as setting negative entries to zero, is not itself a matrix product. Two dense layers with that function between them are not one combined matrix.
- When the weight array has more than two axes, numpy.tensordot contracts the axes you name. axes=1 is a tensor dot product; axes=2 is a double contraction.

**Examples:**

```text
W = [[1, 0],
     [0, 2]]
x = [3, 4]
b = [1, 1]
W @ x + b = [4, 9]
```

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://numpy.org/doc/stable/reference/generated/numpy.tensordot.html
</details>

<details>
<summary>How do eigenvalues and eigenvectors apply to Principal Component Analysis (PCA)?</summary>

### How do eigenvalues and eigenvectors apply to Principal Component Analysis (PCA)?

**Definition:**

PCA replaces the features by successive orthogonal directions that explain as much variance as possible. In scikit-learn that fit is an SVD of the centered data, and the SVD is an eigenvalue factorization of the Gram matrix.

**How it works:**

- PCA centers each feature and does not scale it. whiten=True then rescales each kept component to unit variance. That helps a later model that assumes isotropy, for example an RBF support-vector machine or K-means.
- numpy.linalg.svd writes A = U @ diag(s) @ Vh. The rows of Vh are eigenvectors of A.conj().T @ A, and the matching eigenvalues are s**2. After centering, those directions are the principal axes, and s**2 carries the variance along each axis.
- The iris example projects 4 features onto the 2 components that explain the most variance.
- Use numpy.linalg.eigh on a covariance matrix only when you formed that matrix yourself. PCA in scikit-learn goes through the SVD of the centered data instead.

**Examples:**

```text
iris: 4 measurements per flower
PCA projects them onto 2 components
those 2 directions are the leading right singular vectors
of the centered  features
```

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html
</details>

<details>
<summary>Explain how the SVD is used in recommendation systems.</summary>

### Explain how the SVD is used in recommendation systems.

**Definition:**

A recommendation matrix has one row per user and one column per item. A truncated SVD keeps the k largest singular values and rebuilds a low-rank approximation of the numbers that were actually stored.

**How it works:**

- numpy.linalg.svd returns U, s, Vh with s sorted descending. With full_matrices=False the reconstruction is (U * s) @ Vh.
- Keeping only the first k singular values replaces s with a shorter vector and drops the matching columns of U and rows of Vh. Small singular values are the directions the approximation discards.
- sklearn.decomposition.TruncatedSVD computes only those k values. On a term-document matrix the same truncation is latent semantic analysis. On a user-item matrix it is the same factorization with a different meaning for the rows.
- Both factorizations fit every number in the array they are given. A missing rating stored as 0 is fit as a real zero. The reconstruction does not know which zeros were absent. Subtract column means first when the truncation should match PCA; TruncatedSVD itself does not center.

**Examples:**

```text
observed ratings:
[[5, 4, 1],
 [10, 8, 2]]
row 2 is exactly 2 * row 1, so the matrix has rank 1
(U[:, :1] * s[:1]) @ Vh[:1] rebuilds both rows
an unobserved pair is not a stored 0 in this matrix
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>Discuss the importance of linear algebra in optimization problems, such as gradient descent.</summary>

### Discuss the importance of linear algebra in optimization problems, such as gradient descent.

**Definition:**

A typical training loss is built from a matrix product. Ordinary least squares minimizes ||X @ w - y||^2. Gradient descent moves w to reduce that quantity; a direct SVD solves the same problem in one factorization.

**How it works:**

- The residual is r = X @ w - y. The gradient of ||r||^2 with respect to w is 2 X.T @ r. A descent step subtracts a positive step size times that vector.
- LinearRegression does not iterate that step. It computes the least-squares solution from the SVD of X.
- Ridge minimizes ||X @ w - y||^2 + alpha ||w||^2 with alpha >= 0. Larger alpha shrinks w and makes the weights more robust when columns of X are nearly dependent.
- Lasso adds an alpha ||w||_1 penalty and fits the weights by coordinate descent: one coordinate at a time, with the other columns held fixed. The linear algebra in that inner step is a dot product of one column with the current residual.

**Examples:**

```text
X = [[1, 0], [0, 1], [1, 1]]
w = [0, 0]
y = [1, 1, 2]
r = X @ w - y = [-1, -1, -2]
gradient = 2 X.T @ r = [-6, -6]
a step subtracts a fraction of [-6, -6] from w
```

**Sources:**

https://scikit-learn.org/stable/modules/linear_model.html
</details>

### Practical Implementation and Problem Solving

<details>
<summary>How would you handle large-scale matrix operations efficiently in terms of memory and computation?</summary>

### How would you handle large-scale matrix operations efficiently in terms of memory and computation?

**Definition:**

Keep the matrix in a form that matches the operation, and avoid a factorization whose workspace grows with every sample. Dense LAPACK kernels handle the blocks that do fit; out-of-core and sparse formats handle the ones that do not.

**How it works:**

- numpy.linalg.svd, eig, and solve call LAPACK (gesdd, geev, gesv). They run on a dense array that has to sit in memory, including stacks of shape (..., M, N).
- PCA itself is a batch transform: every row must fit in memory. IncrementalPCA.partial_fit consumes chunks from disk or from a database, and it can also fit a numpy.memmap. It stores component and noise estimates, so memory follows the batch size rather than the full sample count.
- PCA(svd_solver='randomized') computes an approximation of the singular vectors you intend to keep. Its memory is proportional to 2 * n_max * n_components, against n_max * n_min for the exact method.
- A matrix that is mostly zeros belongs in scipy.sparse, with a compressed row format for matrix-vector products. Do not convert it to a dense ndarray just to call a NumPy ufunc.

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html
https://docs.scipy.org/doc/scipy/reference/sparse.html
</details>

<details>
<summary>Explain how you would preprocess data to be used in linear algebra computations.</summary>

### Explain how you would preprocess data to be used in linear algebra computations.

**Definition:**

Put every feature on a comparable scale, then center it, before a factorization that treats large entries as important. PCA centers each feature and does not scale it, so unequal units leak into the singular vectors.

**How it works:**

- Subtract the per-feature mean yourself when you call numpy.linalg.svd or TruncatedSVD. TruncatedSVD does not center. Truncated SVD of a matrix whose columns already have mean zero is the same transformation as PCA.
- Divide by a per-feature scale when units differ (years next to millimeters). Otherwise the column with the largest spread dominates s and the leading singular vector.
- whiten=True, after that centering, rescales the kept PCA components to unit variance for a downstream model that assumes isotropy.
- Drop or combine columns that matrix_rank treats as the same direction. A nearly dependent design matrix makes ordinary-least-squares weights high variance.

**Examples:**

```text
features in mixed units: [1.80 m, 180 cm] is the same height twice
center each column, then divide by its scale
before SVD or PCA
otherwise the centimeter column dominates the leading component
```

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.matrix_rank.html
https://scikit-learn.org/stable/modules/linear_model.html
</details>

<details>
<summary>Describe ways to find the rank of a matrix effectively.</summary>

### Describe ways to find the rank of a matrix effectively.

**Definition:**

Numerical rank is the number of singular values above a tolerance. numpy.linalg.matrix_rank counts them. A determinant test is the wrong tool: a tiny nonzero determinant does not say which columns are independent, and it is undefined for a rectangular matrix.

**How it works:**

- The default tolerance is S.max() * max(M, N) * eps, the same cutoff MATLAB uses. Singular values below it count as zero even when rounding kept them slightly positive. Pass tol or rtol when the uncertainty in the data is larger than machine epsilon. Only one of tol and rtol may be set.
- hermitian=True uses a faster singular-value path when the matrix is Hermitian, or symmetric in the real case.
- scipy.linalg.orth returns a basis whose width is that effective rank. scipy.linalg.null_space returns a basis for the complementary kernel. The two widths are the split of the column count into range and kernel under the same style of cutoff.
- matrix_rank on a stack of matrices returns one rank per matrix. A 1-d vector has rank 1 unless it is all zeros, which has rank 0.

**Examples:**

```text
matrix_rank(np.eye(4)) == 4
I = np.eye(4); I[-1, -1] = 0
matrix_rank(I) == 3
matrix_rank(np.zeros((4,))) == 0
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.matrix_rank.html
https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.orth.html
https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.null_space.html
</details>

<details>
<summary>What would you consider when choosing a library for linear algebra operations?</summary>

### What would you consider when choosing a library for linear algebra operations?

**Definition:**

Pick the library by the matrix shape and by the factorization you actually need. NumPy covers dense stacks, SciPy covers sparse storage and a few factorizations NumPy does not wrap, and scikit-learn covers the machine-learning factorizations that center or truncate.

**How it works:**

- numpy.linalg works on stacked arrays of shape (..., M, N): one call solves or decomposes every matrix in the stack. solve, lstsq, inv, pinv, det, slogdet, matrix_rank, cond, qr, svd, eig, eigh, norm, and cholesky live there, and they raise LinAlgError when a factorization does not exist or does not converge.
- Use scipy.linalg.null_space and scipy.linalg.orth when you want an orthonormal basis for the kernel or the range. Use scipy.linalg.lu for a pivoted LU. Use scipy.linalg.schur for a non-Hermitian normal matrix, where eig does not promise a unitary eigenvector matrix.
- Use sklearn PCA, IncrementalPCA, or TruncatedSVD when the matrix is a data table and you want components, a memory-mapped fit, or a rank-k approximation. Use sklearn LinearRegression or Ridge when the product is a least-squares fit.
- Use scipy.sparse when most entries are zero. Build with a coordinate format, multiply with a compressed row format, and do not pass the sparse matrix through NumPy ufuncs.

**Sources:**

https://numpy.org/doc/stable/reference/routines.linalg.html
https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.null_space.html
https://scikit-learn.org/stable/modules/decomposition.html
https://docs.scipy.org/doc/scipy/reference/sparse.html
</details>

<details>
<summary>How do you ensure numerical stability when performing matrix computations?</summary>

### How do you ensure numerical stability when performing matrix computations?

**Definition:**

Stability means a small change in the input moves the answer by a small amount. Use a tolerance on singular values, avoid forming an inverse, and prefer a factorization that does not square the dependence among columns.

**How it works:**

- numpy.linalg.matrix_rank treats singular values below S.max() * max(M, N) * eps as zero, because an exact linear dependence rarely produces a computed singular value of 0. Raise tol when the measurements are coarser than machine epsilon.
- numpy.linalg.solve factors A once for A @ x = b. Forming inv(A) and multiplying is a second, less stable step, and it fails with the same singular matrices. lstsq is the call when A is rectangular or rank-deficient.
- LinearRegression solves least squares from the SVD of X. Explicitly forming X.T @ X first builds the Gram matrix whose near-dependence is what the linear-model page describes as a nearly singular design matrix and high-variance weights.
- numpy.linalg.eig on [[1+1e-9, 0], [0, 1-1e-9]] returns eigenvalues that both round to 1. Close eigenvalues are not resolved just because the entries differ on paper. For a large determinant, slogdet returns a sign and a log, which avoids overflow. det itself comes from LU.

**Examples:**

```text
A = [[1 + 1e-9, 0],
     [0, 1 - 1e-9]]
eig(A) reports both eigenvalues as 1
the theoretical split 1e-9 is lost to rounding
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.matrix_rank.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.solve.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.eig.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.det.html
https://scikit-learn.org/stable/modules/linear_model.html
</details>

### Real-world Scenarios and Data Handling

<details>
<summary>Propose a method for dimensionality reduction using linear algebra techniques.</summary>

### Propose a method for dimensionality reduction using linear algebra techniques.

**Definition:**

Project the centered data onto the singular vectors that belong to the largest singular values. Those directions keep the most variance, and the rest are dropped.

**How it works:**

- sklearn PCA learns n components in fit and projects new rows onto them. It centers and does not scale. The iris set, 4 features, is the documented projection onto the 2 components that explain the most variance.
- When most components will be discarded, PCA(svd_solver='randomized') approximates only the singular vectors you keep. A 64-by-64 grayscale face is 4096 coordinates, while the faces lie near a much lower-dimensional set; the page uses that as the reason to keep a couple of hundred components instead of 4096.
- TruncatedSVD keeps the k largest singular values without requiring a centered matrix. If you subtract the column means first, that truncation matches PCA.
- whiten=True rescales the kept components to unit variance when the next model assumes isotropy, such as an RBF SVM or K-means.

**Examples:**

```text
64 x 64 grayscale face -> vector of length 4096
PCA(svd_solver='randomized', n_components=200)
projects each face onto 200 components
and drops the singular vectors of the smaller singular values
```

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>Explain how you would use linear algebra to clean and preprocess a dataset.</summary>

### Explain how you would use linear algebra to clean and preprocess a dataset.

**Definition:**

Cleaning here means making the design matrix match what the factorization assumes: finite entries, comparable column scales, a controlled rank, and zeros stored as zeros.

**How it works:**

- Center each feature before PCA or before a raw SVD you want to match PCA. PCA and IncrementalPCA center and do not scale. TruncatedSVD does not center, so subtract the column means yourself when you need that equivalence.
- Scale columns that are not in the same unit. A height stored both in meters and in centimeters is two copies of one direction with different magnitudes, and the larger copy dominates the leading singular vector.
- Count the numerical rank with matrix_rank. Columns that fall under the singular-value tolerance are the ones to drop or merge. Leaving them in makes ordinary least squares high variance.
- If most entries are structural zeros, store the table as a scipy.sparse matrix instead of a dense block of explicit zeros. Build in coordinate format and convert to compressed rows before products.

**Examples:**

```text
raw column: [1.6, 1.8, 1.7] meters
duplicate:  [160, 180, 170] centimeters
after centering and scaling, the two columns match
matrix_rank then reports one direction, not two
```

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.matrix_rank.html
https://docs.scipy.org/doc/scipy/reference/sparse.html
https://scikit-learn.org/stable/modules/linear_model.html
</details>

<details>
<summary>Describe a scenario where linear algebra could be used to improve model accuracy.</summary>

### Describe a scenario where linear algebra could be used to improve model accuracy.

**Definition:**

When two feature columns are nearly the same direction, ordinary least squares gives weights with high variance. Ridge adds alpha ||w||^2 to the same residual and the weights stay finite and smaller.

**How it works:**

- The linear-model page states the failure mode directly: correlated columns make the design matrix close to singular, and the least-squares estimate becomes highly sensitive to noise in y.
- Ridge minimizes ||X @ w - y||^2 + alpha ||w||^2 with alpha >= 0. Larger alpha shrinks w and makes the coefficients more robust to that collinearity. The documented fit Ridge(alpha=0.5) on three rows returns finite coef_.
- A second, separate case is a downstream model that assumes isotropy. PCA(whiten=True) rescales the kept components to unit variance. The decomposition page names RBF support-vector machines and K-means as models that benefit.
- PCA still only centers. Scale the raw columns first when they are different units, or the leading component follows the column with the largest spread rather than the structure you care about.

**Examples:**

```text
X = [[0, 0], [0, 0], [1, 1]]
y = [0, 0.1, 1]
Ridge(alpha=0.5).fit(X, y).coef_
# about [0.345, 0.345]
the two identical columns share one shrunk weight
```

**Sources:**

https://scikit-learn.org/stable/modules/linear_model.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>How would you use matrices to model relational data in databases?</summary>

### How would you use matrices to model relational data in databases?

**Definition:**

A relation between two sets of entities is a matrix: one row per entity of the first set, one column per entity of the second, and the entry is the weight of that pair. Most pairs are absent, so the matrix is sparse.

**How it works:**

- Build it as a coo_array from three parallel lists: row ids, column ids, and values. Duplicate coordinates are summed when you convert to CSR or CSC.
- Convert to csr_array before a product. The matmul A @ v sums, for each row, the values of v at the columns where that row has an entry. That is the aggregate of the relation.
- Do not call NumPy ufuncs on the sparse array. NumPy treats it as a generic object. Convert with toarray only for a block small enough to densify, as the sparse page does before numpy.linalg.solve.
- scipy.sparse.csgraph is the submodule for graph routines on this storage. The matrix itself is still the relation: a 1 means an edge, a rating means a weighted pair.

**Examples:**

```text
csr_array([[1, 2, 0],
           [0, 0, 3],
           [4, 0, 5]]) @ [1, 0, -1]
= [1, -3, -1]
row 0 keeps columns 0 and 1; row 1 keeps column 2
```

**Sources:**

https://docs.scipy.org/doc/scipy/reference/sparse.html
</details>

<details>
<summary>Discuss how to apply linear algebra to image processing tasks.</summary>

### Discuss how to apply linear algebra to image processing tasks.

**Definition:**

A grayscale image is a matrix of intensities. A stack of images is a data matrix whose rows are vectorized pictures. Scaling, low-rank approximation, and PCA are then ordinary matrix operations.

**How it works:**

- Multiplying the matrix by a scalar multiplies every intensity. A factor 1/2 darkens the picture without changing its shape.
- numpy.linalg.svd with full_matrices=False rebuilds the image as (U * s) @ Vh. Keeping the largest singular values drops the small ones and yields a rank-k approximation of the same pixel grid.
- The decomposition page treats a 64-by-64 grayscale face as a vector of length 4096. PCA(svd_solver='randomized') projects many such vectors onto the singular vectors tied to the largest singular values, which is how a gallery is compressed to a few hundred coordinates.
- PCA centers and does not scale. Pixel values that already share one scale, such as 0 to 255, do not need a second rescaling before that projection.

**Examples:**

```text
pixels = [[0, 80],
          [160, 240]]
0.5 * pixels = [[0, 40],
                [80, 120]]
a 64 x 64 face is one row of length 4096 in the data matrix
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

### Advanced Topics and Research

<details>
<summary>What are sparse matrices and how are they efficiently represented and used?</summary>

### What are sparse matrices and how are they efficiently represented and used?

**Definition:**

A sparse array stores only the nonzero entries, plus the indexes needed to place them. scipy.sparse provides seven layouts. Construction and multiplication do not use the same layout.

**How it works:**

- Construct with coo_array, dok_array, or lil_array. COO is the recommended construction format: pass values and coordinate arrays. Indexes need not be sorted. Duplicate coordinates are summed on conversion to CSR or CSC.
- Multiply or factor after converting to csr_array or csc_array. CSR is the layout suited to a fast matrix-vector product. Conversions among CSR, CSC, and COO are linear in the number of stored entries. lil_array converts cheaply to CSR and less cheaply to CSC.
- The seven layouts are csc, csr, bsr, lil, dok, coo, and dia. dia stores diagonals. bsr stores dense blocks. Do not call NumPy functions directly on these objects; NumPy treats them as generic Python objects. Use a SciPy method, or toarray when a dense copy is acceptable.
- scipy.sparse.linalg.spsolve solves A @ x = b while A stays sparse. The page checks it against numpy.linalg.solve on A.toarray().

**Examples:**

```text
from scipy.sparse import csr_array
A = csr_array([[1, 2, 0], [0, 0, 3], [4, 0, 5]])
A @ [1, 0, -1] == [1, -3, -1]
# five stored numbers, not nine
```

**Sources:**

https://docs.scipy.org/doc/scipy/reference/sparse.html
</details>

<details>
<summary>Discuss the role of linear algebra in deep learning, specifically in training convolutional neural networks.</summary>

### Discuss the role of linear algebra in deep learning, specifically in training convolutional neural networks.

**Definition:**

A convolutional layer is a linear map with a small shared kernel, followed by a pointwise function that is not linear. Training changes the kernel entries. The forward value at each window is a contraction, the same kind of sum of products as numpy.tensordot.

**How it works:**

- At one window the output entry is the sum of kernel values times the matching patch values, plus a bias. tensordot with an explicit list of axes is that sum when the kernel and the patch are arrays. The same kernel is reused at every window, so the linear piece is not a free dense matrix with an independent weight per output pixel.
- A grayscale picture is already a matrix. The decomposition page flattens a 64-by-64 face into a vector of length 4096 when the next step is a dense SVD. A convolution never forms that 4096-by-4096 matrix; it applies the kernel to each window.
- The pointwise function after the contraction, such as setting negative entries to zero, is outside the matrix product. Stacking a second convolution does not collapse into one kernel.
- The gradient of a sum of those window contractions with respect to one kernel entry is the sum over windows of (the upstream residual at that window) times (the patch entry that multiplied the kernel entry). That is the same pattern as the least-squares gradient X.T @ r: each weight is updated from the inputs that multiplied it.

**Examples:**

```text
patch = [[1, 2],
         [3, 4]]
kernel = [[1, 0],
          [0, 1]]
window sum = 1*1 + 2*0 + 3*0 + 4*1 = 5
the same kernel is applied at the next window
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.tensordot.html
https://scikit-learn.org/stable/modules/linear_model.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>Explain how tensor operations are vital in algorithms working with higher-dimensional data.</summary>

### Explain how tensor operations are vital in algorithms working with higher-dimensional data.

**Definition:**

A tensor here is an array with more than two axes. The useful operations contract some of those axes and leave the others, which is what numpy.tensordot does. A stack of matrices is the same idea inside numpy.linalg: the last two axes are the matrix, and the leading axes are the batch.

**How it works:**

- tensordot(a, b, axes=N) sums over the last N axes of a and the first N axes of b. axes=0 is an outer product, axes=1 is a dot product, and the default axes=2 is a double contraction.
- When the contracted axes are not in that trailing-leading position, pass two sequences. The documented case a.shape == (2, 3, 4) and b.shape == (3, 4, 5) with axes=([1, 2], [0, 1]) sums the shared (3, 4) axes and returns shape (2, 5).
- numpy.linalg.svd on an array with more than two dimensions runs independently on each trailing matrix. A batch of pictures stored as (batch, height, width) can be factored one picture at a time without a Python loop, as long as the picture occupies the last two axes.
- An explicit Python loop that multiplies matching entries and sums them is the same contraction. The page shows that loop next to tensordot and the results match.

**Examples:**

```text
a shape (2, 3, 4)
b shape (3, 4, 5)
tensordot(a, b, axes=([1, 2], [0, 1])).shape == (2, 5)
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.tensordot.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html
</details>

<details>
<summary>What is the role of linear algebra in time series analysis?</summary>

### What is the role of linear algebra in time series analysis?

**Definition:**

A linear model of a series turns lags into columns of a design matrix. Predicting the next value is then ordinary least squares, the same minimization of ||X @ w - y||^2.

**How it works:**

- Each row of X holds a window of past values. y holds the value that followed that window. LinearRegression stores the lag weights in coef_ and an intercept_ .
- The solution is the SVD of that X, with cost on the order of n_samples * n_features^2 when there are at least as many windows as lags.
- If two lag columns are nearly dependent, the design matrix is close to singular and the weights have high variance. Ridge adds alpha ||w||^2 and shrinks them.
- This construction uses only the observed numbers you placed in X and y. It does not by itself choose how many lags to keep or fill gaps in the series.

**Examples:**

```text
series = [2, 4, 6, 8, 10]
X = [[2], [4], [6], [8]]
y = [4, 6, 8, 10]
one lag column; the next value is the previous value plus 2
LinearRegression minimizes ||X @ w - y||^2 on these rows
```

**Sources:**

https://scikit-learn.org/stable/modules/linear_model.html
</details>

<details>
<summary>How do graph theory and linear algebra intersect in machine learning?</summary>

### How do graph theory and linear algebra intersect in machine learning?

**Definition:**

A graph is stored as a matrix. The entry (i, j) is the weight of the relation between node i and node j, and a matrix-vector product aggregates neighbor values.

**How it works:**

- Most graphs are sparse: each node touches a few others. Store the adjacency as a csr_array. The product A @ x sums, along each row, the entries of x at the neighboring columns.
- Build with coo_array from edge endpoints and weights, then convert to CSR. Duplicate edges sum on that conversion.
- scipy.sparse.csgraph is the compressed-graph submodule on top of this storage. scipy.sparse.linalg holds the sparse factorizations. Neither step requires a dense V-by-V matrix.
- A user-item relation is the same object with two node types. Truncated SVD of that matrix is the low-rank approximation already used for recommendations, not a different graph algorithm.

**Examples:**

```text
edges (0,0)=1, (0,1)=2, (1,2)=3, (2,0)=4, (2,2)=5
A @ [1, 0, -1] = [1, -3, -1]
node 1 only sees column 2, so it contributes 3 * -1
```

**Sources:**

https://docs.scipy.org/doc/scipy/reference/sparse.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html
</details>

### Case Studies and Scenario-Based Challenges

<details>
<summary>Given a dataset, determine if PCA would be beneficial and justify your approach.</summary>

### Given a dataset, determine if PCA would be beneficial and justify your approach.

**Definition:**

PCA helps when many numeric columns repeat the same directions and a later step wants fewer coordinates or isotropic ones. It does not help when the columns are already few, on a shared scale, and the model needs those original coordinates.

**How it works:**

- Check the numerical rank and the spread of singular values of the centered data. matrix_rank near the column count, with singular values of similar size, means there is little to drop. A large gap after the first few singular values means a short projection keeps most of the variance.
- Scale columns that use different units before the fit. PCA centers and does not scale, so an unscaled column with a huge spread becomes the leading component.
- Use the projection when the next model assumes isotropy. whiten=True rescales components to unit variance; the page names RBF SVM and K-means. The iris example is the small case: 4 features projected to the 2 components that explain the most variance. A 64-by-64 face, 4096 coordinates, is the large case, where the page says the faces lie near a much lower-dimensional set.
- If the table does not fit in memory, exact PCA cannot run: it is a batch method. IncrementalPCA.partial_fit or a memmap fit is the version that still applies. Skip PCA when you have a handful of already independent features and the downstream model is ordinary least squares on those features.

**Examples:**

```text
iris: 4 features -> 2 PCA components that explain the most variance
faces: 64*64 = 4096 coordinates -> on the order of 200 components
a table of 3 independent features for least squares: keep the columns
```

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
https://numpy.org/doc/stable/reference/generated/numpy.linalg.matrix_rank.html
</details>

<details>
<summary>How would you implement a linear algebra-based algorithm to classify text documents?</summary>

### How would you implement a linear algebra-based algorithm to classify text documents?

**Definition:**

Turn documents into a term-document matrix, replace it by a rank-k SVD, and fit a linear classifier on those k coordinates. On that matrix the truncated SVD is latent semantic analysis.

**How it works:**

- TruncatedSVD keeps the k largest singular values. The transformed training set is U_k * Σ_k, and a new document row x is sent to x @ V_k. The page recommends a tf-idf matrix with sublinear_tf=True and use_idf=True rather than raw counts, because those options pull the entries closer to a Gaussian, which matches the assumptions LSA is making.
- TruncatedSVD does not center. Subtract column means only when you want the truncation to match PCA.
- RidgeClassifier then treats the reduced coordinates as a regression problem. Binary labels are mapped to {-1, 1}, the model minimizes the ridge residual, and the predicted class is the sign of the prediction. Multiclass is multi-output regression, and the class is the output with the largest value.
- The term matrix is sparse. Keep it sparse through the vectorization, and do not densify it with a NumPy ufunc.

**Examples:**

```text
documents x terms, tf-idf with sublinear_tf and use_idf
TruncatedSVD(n_components=k) -> k coordinates per document
RidgeClassifier: label +1 or -1, predict by the sign of X @ w
```

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
https://scikit-learn.org/stable/modules/linear_model.html
https://docs.scipy.org/doc/scipy/reference/sparse.html
</details>

<details>
<summary>Design a linear algebra solution for a collaborative filtering problem in a movie recommendation system.</summary>

### Design a linear algebra solution for a collaborative filtering problem in a movie recommendation system.

**Definition:**

Store observed ratings as a user-by-movie matrix and replace that matrix by a rank-k SVD. The score for a pair is the corresponding entry of U_k @ diag(s_k) @ Vh_k.

**How it works:**

- numpy.linalg.svd with full_matrices=False returns the reconstruction (U * s) @ Vh. TruncatedSVD computes only the k largest singular values and exposes the same split: the training projection is U_k Σ_k, and V_k maps a row into those coordinates. The low-rank matrix is X ≈ U_k Σ_k V_k.T.
- The factorization fits every stored number. Do not write a missing rating as 0 and then treat the rebuilt 0 as a prediction the algorithm invented. Hold out a known entry, factor the remaining observed entries only if they still form the matrix you intend, or factor a complete low-rank demo and read the rebuilt entry.
- TruncatedSVD does not center. Subtract each movie's mean from the observed ratings first when the truncation should match PCA, then add that mean back onto the rebuilt score.
- Choose k by how fast the singular values decay. Directions with tiny singular values are the ones the approximation drops.

**Examples:**

```text
users x movies, fully observed and rank 1:
[[5, 4, 1],
 [10, 8, 2]]
row 2 = 2 * row 1
k = 1 rebuilds both rows, including movie 3
a pair that was never stored is not a 0 in this matrix
```

**Sources:**

https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html
https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>Propose strategies to visualize high-dimensional data using linear algebra techniques.</summary>

### Propose strategies to visualize high-dimensional data using linear algebra techniques.

**Definition:**

Plot the first two PCA coordinates. Those two directions are the orthogonal components that explain the most variance of the centered data, which is what a scatter plot of the projection shows.

**How it works:**

- The documented iris figure projects 4 features onto the 2 components that explain the most variance. The same call with n_components=2 is the picture for any numeric table.
- Scale columns that are not in the same unit before fit. PCA centers and does not scale, so an unscaled column dominates the horizontal axis of the plot.
- whiten=True rescales each plotted component to unit variance. Use it when the picture should not stretch along the component with the larger singular value. Leave it off when the axis length is supposed to show that variance.
- A 64-by-64 face is 4096 coordinates. PCA(svd_solver='randomized', n_components=2) approximates only the two singular vectors you will plot, instead of an exact SVD of the full width.

**Examples:**

```text
iris rows: 4 measurements
PCA(n_components=2).fit_transform(X) -> two coordinates per flower
the scatter plot is those two columns
```

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
</details>

<details>
<summary>Discuss an approach for optimizing memory usage in matrix computations for a large-scale machine learning application.</summary>

### Discuss an approach for optimizing memory usage in matrix computations for a large-scale machine learning application.

**Definition:**

Do not hold every sample and a dense Gram matrix at the same time. Stream the rows, keep a rank-k factorization, and store structural zeros in a sparse layout.

**How it works:**

- Exact PCA needs the whole table in memory. IncrementalPCA.partial_fit reads chunks, or fit reads a numpy.memmap. It stores component and noise estimates, so memory follows the batch size rather than the full sample count.
- PCA(svd_solver='randomized') uses memory proportional to 2 * n_max * n_components, against n_max * n_min for the exact method. That is the right call when you will drop most singular vectors, as with 4096-dimensional faces kept at a few hundred components.
- Do not form X.T @ X as an explicit n_features-by-n_features matrix. LinearRegression already solves the least-squares problem from the SVD of X. The Gram matrix is also the object that becomes nearly singular when columns are dependent.
- A user-item or term matrix that is mostly zeros stays a csr_array. Products use that layout. toarray is reserved for a block you have already decided is small enough to densify.

**Examples:**

```text
4096 face features, keep 200 components
randomized PCA memory scales as 2 * n_max * 200
exact PCA memory scales as n_max * min(n_samples, 4096)
IncrementalPCA.partial_fit keeps one batch, not the whole gallery
```

**Sources:**

https://scikit-learn.org/stable/modules/decomposition.html
https://scikit-learn.org/stable/modules/linear_model.html
https://docs.scipy.org/doc/scipy/reference/sparse.html
</details>
