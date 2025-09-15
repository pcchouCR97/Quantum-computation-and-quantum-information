# The Stbailizer formalism

## References

1.  [Pauli operations and observables (IBM)](https://quantum.cloud.ibm.com/learning/en/courses/foundations-of-quantum-error-correction/stabilizer-formalism/pauli-operations-and-observables)

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

- A **Z-measurement** is the standard computational basis measurement, while an **X-measurement** is in the $|+\rangle, |-\rangle$ basis. In both cases, the outcomes are eigenvalues $\pm 1$.  

- The same idea extends to **n-qubit Pauli operators**.  
  - Still only two possible outcomes: $\pm 1$.  
  - The Hilbert space splits into two equal subspaces, each of dimension $2^{n-1}$.  
  - Each projector has rank $2^{n-1}$, not rank 1 as in the single-qubit case.  

- Important: this is **not** the same as measuring each qubit’s Pauli operator independently, which would give $2^n$ outcomes.  
  - Instead, the n-qubit Pauli observable defines just two outcomes: $\pm 1$.  

- **Example:** $Z \otimes Z$  

    -   Spectral decomposition:  
        
        $$
        Z \otimes Z = (|00\rangle\langle00| + |11\rangle\langle11|) - (|01\rangle\langle01| + |10\rangle\langle10|)
        $$  

    -   Define projectors:  
    
        $$
        \Pi_0 = |00\rangle\langle00| + |11\rangle\langle11|, \quad
        \Pi_1 = |01\rangle\langle01| + |10\rangle\langle10|
        $$

    -   Outcome $+1$: state lies in the $|00\rangle, |11\rangle$ subspace.  
    -   Outcome $-1$: state lies in the $|01\rangle, |10\rangle$ subspace.  

        So these are the two projections that define the measurement.

-   Example application: measuring the Bell state $|\phi^+\rangle$.  
    -   Always gives outcome $+1$.  
    -   State is left unchanged (no collapse to $|00\rangle$ or $|11\rangle$).  

### Nondestructive implementation through phase estimation

For any $n$-qubit Pauli operation, we can perform the measurement associated with that observable nondestructively using phase estimation.

# Nondestructive Implementation via Phase Estimation

- Any $n$-qubit Pauli observable can be measured **nondestructively** using **phase estimation**.  
- The trick: use a single control qubit prepared in $|+\rangle$.  
- Apply a controlled-$P$ (Pauli operator) and then a Hadamard on the control.  
- Measuring the control qubit in the standard basis gives outcomes:  
  - `0` → eigenvalue $+1$  
  - `1` → eigenvalue $-1$  
- The target qubits are left unchanged, so the measurement does not collapse the system beyond distinguishing the $\pm 1$ eigenspaces.  

<div style="text-align: center;">
    <img src="/quantum_information/images/phase-estimation-Pauli.svg" alt="phase-estimation-Pauli" style="width: auto; height: auto;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
    </p>
</div>

??? question "Why measure the control?"
    -   Pauli operators have eigenvalues $\pm 1$.  
    -   If the system is in an eigenstate of $P$, the controlled-$P$ applies a **phase kickback** onto the control qubit equal to that eigenvalue.  

-   After the Hadamard:  
    -     Eigenvalue $+1 \;\Rightarrow$ control goes to $|0\rangle$.  
    -     Eigenvalue $-1 \;\Rightarrow$ control goes to $|1\rangle$.  
-   Measuring the control reveals the eigenvalue ($\pm 1$), while the system qubits remain unchanged.  

-   `0` on the control → eigenvalue $+1$.  
-   `1` on the control → eigenvalue $-1$.  
-   This way, the observable is measured nondestructively: the eigenvalue is extracted, but the system state within that eigenspace is preserved.  

A similar method works for Pauli operations on multiple qubits. For example, the following circuit diagram shows a nondestructive measurement of the 3-qubit Pauli observable $P_{2}\otimes P_{1}\otimes P_{0}$, for any choice of $P_{0},P_{1},P_{2} \in \{X,Y,Z\}$

<div style="text-align: center;">
    <img src="/quantum_information/images/phase-estimation-3-qubit-Pauli.svg" alt="phase-estimation-3-qubit-Pauli" style="width: auto; height: auto;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
    </p>
</div>

-   Phase estimation works for any $n$-qubit Pauli operator.  
-   Only need controlled gates for the **non-identity** factors; identity parts can be skipped, so lower-weight operators need smaller circuits.  
-   Regardless of $n$, just one control qubit is enough — since Pauli observables only have two outcomes ($\pm 1$), extra controls add no value.  
-   Example: nondestructive measurement of $Z \otimes Z$, useful in stabilizer codes like the 3-bit repetition code. 

Here's a specific example, of a nondestructive implementation of a $Z \otimes Z$ measurement, which is relevant to the description of the 3-bit repetition code as a stabilizer code that we'll see shortly.

<div style="text-align: center;">
    <img src="/quantum_information/images/phase-estimation-ZZ.svg" alt="phase-estimation-ZZ" style="width: auto; height: auto;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
    </p>
</div>

In this case, and for tensor products of more than two $Z$ observables more generally, the circuit can be simplified.

<div style="text-align: center;">
    <img src="/quantum_information/images/simplified-ZZ.svg" alt="simplified-ZZ.svg" style="width: auto; height: auto;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
    </p>
</div>

Thus, this measurement is equivalent to nondestructively measuring the parity (or XOR) of the standard basis states of two qubits.

## Repetition code revisited

### Pauli observables for the repetition code

!!! tips
    $ZZI$ checks if the first two qubits are equal. If $ZZI|\psi\rangle = |\psi\rangle$, then $|\psi\rangle$ is a +1 eigenstate — the state passes the stabilizer check.  

Recall the 3-bit repetition code to qubits, a given qubit state vector $\alpha|0\rangle + \beta|1\rangle$ is encoded as 

$$
|\psi\rangle = \alpha|000\rangle + \beta|111\rangle.
$$

Any state $|\psi\rangle$ of this form is a valid 3-qubit encoding of a qubit state.

If we set two Pauli operations $Z\otimes Z \otimes I$ and $I\otimes Z\otimes Z$ as observables, and measure both ussing the circuits suggested before, then we would be certain to obtain measurement outcomes corresponding to $+1$ eigenvalues, since $|\psi\rangle$ is an eigenvector of both observables with eigenvcalue 1.

The two equations above therefore imply that the parity check circuit outputs $00$, which is the syndrome that indicates that no errors have been detected.

The 3-qubit Pauli operations $Z\otimes Z \otimes I$ and $I \otimes Z \otimes Z$ called stabilizer generators for this code, and the stabilizer of the code is the set generated by the stabilizer generators.

$$
<Z\otimes Z \otimes I, I\otimes Z\otimes Z > = \{I\otimes I\otimes I ,Z\otimes Z\otimes I ,Z\otimes I\otimes Z, I\otimes Z\otimes Z\}
$$

The stabilizer is a fundamentally important mathematical object associated with this code.

### Error detection

Let's consider bit-flip detection for the 3-bit repetition code. Suppose we have encoded a qubit using the 3-bit repetition code, and a bit flip error occurs on the leftmost qubit, we have

$$
|\psi\rangle \rightarrow (X\otimes I \otimes I)|\psi\rangle
$$

The state $|\psi\rangle$ has been affected by an $X$ error on the leftmost qubit, and our goal is to understand how the measurement of this stabilizer generator, as an observable, is influenced by this error. 

$$
\begin{aligned}
(Z\otimes Z \otimes I)(X \otimes I \otimes I) |\psi\rangle & = -(X \otimes I \otimes I)(Z\otimes Z \otimes I)|\psi\rangle \\
 & (X \otimes I \otimes I) |\psi\rangle
\end{aligned}
$$

Therefore, $(X \otimes I \otimes I)$ is an eigenvector of $Z\otimes Z\otimes I$ with eigenvalue -1. When the measurement associated with the observable $Z \otimes Z \otimes I$ is performed on the state $(X \otimes I \otimes I)|\psi\rangle$, the outcome is therefore certain to be the one associated with the eigenvalue -1.

Similiarly,

$$
\begin{aligned}
(I \otimes Z \otimes Z)( X \otimes I \otimes I)|\psi\rangle & = (X \otimes I \otimes I)(I\otimes Z \otimes Z)|\psi\rangle\\
 & = ( X \otimes I \otimes I)|\psi\rangle
\end{aligned}
$$

What we find when considering these equations is that, regardless of our original state ∣ψ⟩, the corrupted state is an eigenvector of both stabilizer generators, and whether the eigenvalue is +1 or -1 is determined by whether the error commutes or anti-commutes with each stabilizer generator.

For this reason, we really don't need to concern ourselves in general with the specific encoded state we're working with. All that matters is whether the error commutes or anti-commutes with each stabilizer generator.

For example,

$$
\begin{aligned}
(Z \otimes Z \otimes I)(X \otimes X \otimes I) & = -(X \otimes X \otimes I)(Z \otimes Z \otimes I)\\
(I \otimes Z \otimes Z)(X \otimes I \otimes I) & = (X \otimes I \otimes I)(I \otimes Z \otimes Z)
\end{aligned}
$$

Here's a table with one row for each stabilizer generator and one column for each error.

| Stabilizer      | $I \otimes I \otimes I$ | $X \otimes I \otimes I$ | $I \otimes X \otimes I$ | $I \otimes I \otimes X$ |
|-----------------|---------------------------|---------------------------|---------------------------|---------------------------|
| $Z \otimes Z \otimes I$ | +1                        | -1                        | -1                        | +1                        |
| $I \otimes Z \otimes Z$ | +1                        | +1                        | -1                        | -1                        |


### Syndromes

Encodings for the 3-bit repetition code are 3-qubit states, so they're unit vectors in an 8-dimensional complex vector space. The four possible syndromes effectively split this 8 dimensional space into four 2-dimensional subspaces.

# Stabilizer Outcomes for 3-Bit Code


<div style="text-align: center;">
    <img src="/quantum_information/images/stabilizer-generator-table.svg" alt="stabilizer-generator-table.svg" style="width: auto; height: auto;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
    </p>
</div>

## 1. State Space Partition

-   The 3-qubit Hilbert space (8 basis states) splits into 4 subspaces, labeled by stabilizer outcomes $(\pm 1, \pm 1)$.  
-   Example: $(+1,+1)$ subspace = span of $|000\rangle, |111\rangle$ = the **codespace**.  
-   Other subspaces correspond to single-qubit bit-flip errors:
    -   $|001\rangle, |110\rangle$: rightmost qubit filpped. 
    -   $|100\rangle, |011\rangle$: leftmost qubit flipped.
    -   $|010\rangle, |101\rangle$: middle qubit flipeed.

## 2. Operator Partition

-   The 64 possible 3-qubit Pauli operators also split into 4 equal groups, based on which syndrome they produce.  
-   Each operator either commutes or anticommutes with each stabilizer.
-   Example: operators that commute with both stabilizers $\rightarrow$ syndrome $(+1,+1)$.  
-   Same rule applies for the other three syndromes.

## 3. Logical Operators

-   Some operators commute with all stabilizers but are **not** stabilizers themselves.  
-   These act as logical gates on the encoded qubit.  
-   Example: $X \otimes X \otimes X$ flips $\alpha|000\rangle+\beta|111\rangle$ to $\alpha|111\rangle+\beta|000\rangle$ → logical $X$.

## Takeaway

-   Stabilizers partition both **states** (valid vs error spaces) and **operators** (which errors map to which syndromes).  
-   Logical gates = operators that commute with stabilizers but are not in the stabilizer group.  

!!! example "Syndrome Detection with 3-Bit Repetition Code"

    Say we have

    $$
    |\psi\rangle = \alpha|000\rangle + \beta|111\rangle
    $$  
    
    Stabilized by $ZZI$ and $IZZ$ (both give eigenvalue +1). Apply $XII$ (bit-flip on the first qubit):  
    
    $$
    XII|\psi\rangle = \alpha|100\rangle + \beta|011\rangle
    $$

    -   $IZZ$ commutes with $XII$ → eigenvalue stays **+1**.  
    -   $ZZI$ anticommutes with $XII$ → eigenvalue flips to **−1**.  
    
    $$
    (ZZI, IZZ) = (-1, +1)
    $$
    
    Syndrome $(-1, +1)$ uniquely identifies an error on the **first qubit**.  
    **Correction: flip the first qubit back.**
