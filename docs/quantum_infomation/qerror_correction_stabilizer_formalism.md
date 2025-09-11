# The Stbailizer formalism

## Pauli operation basics
### Properties of Pauli matrices
In quantum computing, the **Pauli matrices** form the foundation of single-qubit operations.  
They are small, simple $2 \times 2$ matrices, yet capture the essential kinds of quantum “errors” and transformations.  

- **Pauli matrices (2×2):**

$$
I = \begin{pmatrix}1 & 0 \\ 0 & 1\end{pmatrix}
\quad
X = \begin{pmatrix}0 & 1 \\ 1 & 0\end{pmatrix}
\quad
Y = \begin{pmatrix}0 & -i \\ i & 0\end{pmatrix}
\quad
Z = \begin{pmatrix}1 & 0 \\ 0 & -1\end{pmatrix}
$$

## Properties of Pauli Matrices

- **Unitary & Hermitian**
  - All Pauli matrices are both unitary and Hermitian.
  - Each squares to identity:  
    $$
    X^2 = Y^2 = Z^2 = I
    $$

- **Anti-commutation (different Paulis):**
  - $XY = -YX$  
  - $XZ = -ZX$  
  - $YZ = -ZY$

- **Multiplication rules (key identities):**
  - $XY = iZ$  
  - $YZ = iX$  
  - $ZX = iY$

## Intuition

- **X:** flips the bit ($|0\rangle \leftrightarrow |1\rangle$).  
- **Z:** flips the phase ($|1\rangle$ picks up a - sign).  
- **Y:** combination of X and Z → both bit- and phase-flip (up to a phase factor).  
- **I:** does nothing, serves as the identity.

??? question "Why care about the minus signs?"
    - Anti-commutation is the backbone of stabilizer codes.  
    - Detecting sign flips = detecting quantum errors.  

### Pauli operations on multiple qubits

- A single-qubit Pauli operation = one of $\{I, X, Y, Z\}$.  
- By taking tensor products, we define **n-qubit Pauli operations**.  
  - Example ($n=9$):  

    $$
    I \otimes I \otimes I \otimes I \otimes I \otimes I \otimes I \otimes I \otimes I
    $$

    $$
    X \otimes X \otimes I \otimes I \otimes I \otimes I \otimes I \otimes I \otimes I
    $$
    
    $$
    X \otimes Y \otimes Z \otimes I \otimes I \otimes X \otimes Y \otimes Z
    $$

#### Phase Factors
-   Sometimes a **Pauli operation** is defined as Pauli matrices *with a phase factor*, e.g. $\pm 1, \pm i$.  
-   For simplicity, here we drop global phases and just use the bare tensor products.  
-   This keeps focus on the structure of errors rather than bookkeeping of phases.  

#### Weight of a Pauli Operation

- **Weight** = number of non-identity matrices in the tensor product.

    -   Example 1: $I^{\otimes 9}$ → weight = 0  
    -   Example 2: $X \otimes X \otimes I^{\otimes 7}$ → weight = 2  
    -   Example 3: $X \otimes Y \otimes Z \otimes I \otimes I \otimes X \otimes Y \otimes Z$ $\rightarrow$ weight = 6  

- **Intuition:**  

    -   Weight tells us how many qubits are “touched” non-trivially.  
    -   In quantum error correction, lower-weight errors are easier to detect and correct.  
    -   Codes are designed so that as long as the error weight is below a threshold, it can be corrected reliably.

### Pauli operation as generators
Pauli matrices, and $n$-qubit Pauli operations more generally, are unitary, and therefore they describe unitary operations on qubits. But they're also Hermitian matrices, and for this reason they describe measurements.

# Pauli Operations as Generators

-   **Idea:** Pauli operators can be treated as *generators* of sets (groups). Start with a few matrices, multiply them in any order, include possible phase factors → you generate a whole set. This “generated set” is written as $\langle P_1, \dots, P_r \rangle$.

-   **Full Pauli group (1 qubit):**  

    $$
    \langle X, Y, Z \rangle = \{\alpha P : \alpha \in \{1, i, -1, -i\},\; P \in \{I, X, Y, Z\}\}
    $$

    Gives 16 elements total.  

-   **Half the Pauli group:**  
  
    If we only take $\langle X, Z \rangle$, multiplication gives  

    $$
    \{I, X, Z, -iY, -I, -X, -Z, iY\}
    $$  
    
    Still closed under multiplication, but only 8 elements. Note: $Y$ appears indirectly since $XZ = \pm iY$.  

-   **Two-qubit case:**  
    With generators $\langle X \otimes X, Z \otimes Z \rangle$:  

    $$
    \{ I \otimes I,\; X \otimes X,\; Z \otimes Z,\; -Y \otimes Y \}
    $$  

    Only 4 elements, because $XX$ and $ZZ$ commute and their product gives $-YY$.  

Let $g_1 = XX$ and $g_2 = ZZ$.

1. **Each squares to identity:**

    -   $g_1^2 = (XX)(XX) = (X^2) \otimes (X^2) = II$  
    -   $g_2^2 = (ZZ)(ZZ) = (Z^2) \otimes (Z^2) = II$

2. **They commute, and their only nontrivial product is:**
   
   $$
   g_1 g_2 = (XX)(ZZ) = (XZ) \otimes (XZ)
   $$

   Using Pauli identities $XZ = -iY$ (equivalently $ZX = +iY$):
   $$
   g_1 g_2 = (-iY) \otimes (-iY) = (-i)^2 YY = -YY
   $$

Since both generators have order 2 and commute, any word in $\langle XX, ZZ \rangle$ reduces to one of:

$$
II, \quad XX, \quad ZZ, \quad (XX)(ZZ) = -YY
$$.



## Pauli observables

### Hermitian matrix observables

# Hermitian Matrix Observables

A Hermitian matrix $A$ represents an observable in quantum mechanics, where the possible measurement outcomes are the distinct real eigenvalues of $A$. By the spectral theorem, we can write $A = \sum_{k=1}^m \lambda_k \Pi_k$, with eigenvalues $\lambda_1, \ldots, \lambda_m$ and corresponding projection operators $\Pi_1, \ldots, \Pi_m$ that satisfy $\Pi_1 + \cdots + \Pi_m = I$. This decomposition is unique up to the ordering of the eigenvalues (e.g., if ordered $\lambda_1 > \lambda_2 > \cdots > \lambda_m$). In practice, the measurement associated with $A$ is the projective measurement defined by the $\Pi_k$, with each eigenvalue $\lambda_k$ being the measurement outcome corresponding to its projector.  


### Measurements from Pauli operations

Pauli operators can also be viewed as **observables** with well-defined spectral decompositions:

$$
X = |+\rangle\langle+| - |-\rangle\langle-|
\quad
Y = |+i\rangle\langle+i| - |-i\rangle\langle-i|
\quad
Z = |0\rangle\langle0| - |1\rangle\langle1|
$$

-   **X-measurement:** projectors $\{|+\rangle\langle+|,\; |-\rangle\langle-|\}$  
-   **Y-measurement:** projectors $\{|+i\rangle\langle+i|,\; |-i\rangle\langle-i|\}$  
-   **Z-measurement:** projectors $\{|0\rangle\langle0|,\; |1\rangle\langle1|\}$  

In all three cases, the measurement outcomes are simply the eigenvalues $\pm 1$. These are referred to as **X-measurements, Y-measurements, and Z-measurements**. They form the basis of many quantum information tasks, including state tomography and error correction, since they directly correspond to fundamental Pauli error checks.

??? question "Pauli matrices eigenvalues and eignvectors"

    - **X:** eigenvalues $\pm 1$, eigenvectors $|+\rangle, |-\rangle$  
    - **Y:** eigenvalues $\pm 1$, eigenvectors $|+i\rangle, |-i\rangle$  
    - **Z:** eigenvalues $\pm 1$, eigenvectors $|0\rangle, |1\rangle$  

- A **Z-measurement** is the standard computational basis measurement, while an **X-measurement** is in the \(|+\rangle, |-\rangle\) basis. In both cases, the outcomes are eigenvalues \(\pm 1\).  

- The same idea extends to **n-qubit Pauli operators**.  
  - Still only two possible outcomes: \(\pm 1\).  
  - The Hilbert space splits into two equal subspaces, each of dimension \(2^{n-1}\).  
  - Each projector has rank \(2^{n-1}\), not rank 1 as in the single-qubit case.  

- Important: this is **not** the same as measuring each qubit’s Pauli operator independently, which would give \(2^n\) outcomes.  
  - Instead, the n-qubit Pauli observable defines just two outcomes: \(\pm 1\).  

- **Example:** \(Z \otimes Z\)  

    -   Spectral decomposition:  
        
        $$
        Z \otimes Z = (|00\rangle\langle00| + |11\rangle\langle11|) - (|01\rangle\langle01| + |10\rangle\langle10|)
        $$  

    -   Define projectors:  
    
        $$
        \Pi_0 = |00\rangle\langle00| + |11\rangle\langle11|, \quad
        \Pi_1 = |01\rangle\langle01| + |10\rangle\langle10|
        $$

    -   Outcome \(+1\): state lies in the \(|00\rangle, |11\rangle\) subspace.  
    -   Outcome \(-1\): state lies in the \(|01\rangle, |10\rangle\) subspace.  

        So these are the two projections that define the measurement.

-   Example application: measuring the Bell state \(|\phi^+\rangle\).  
    -   Always gives outcome \(+1\).  
    -   State is left unchanged (no collapse to \(|00\rangle\) or \(|11\rangle\)).  

### Nondestructive implementation through phase estimation

For any $n$-qubit Pauli operation, we can perform the measurement associated with that observable nondestructively using phase estimation.