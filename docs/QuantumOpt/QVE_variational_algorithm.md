# Variational algorithms

A variational algorithm is a stepping stone of the quantum machines laerning. No matter if it's a Quantum Support Mahcine, Variational circuit, quantum nueral network. 

## The variational algorithm workflow 

Here is the simple workflow of the variational algorithm 

1.  **Initialize Problem**: Variational algorithms start by initializing the quantum computer in a default state $|0\rangle$, then transforming it to some desired (non-parameterized) state $|\rho\rangle$, which we will call *reference state*.

2.  **Prepare ansatz**: To begin iteratively optimizing from the default state $|0\rangle$ to the target state $|\psi(\vec{\theta})\rangle$, we ,ust define a *variational form* $U_{V}(\vec{\theta})$ to represent a collection of parameterized states for out variational algorithm to explore. 

    We refer to any particular compbination of reference state and variational form as an ansatz, such that: $U_{A}(\vec{\theta}) := U_{V}(\vec{\theta})U_{R}$. Ansatze will take the form of parameterized quantum circuits capable of taking the default state $|0\rangle$ to the target state $|\psi(\vec{\theta})\rangle$.

    $$
    \begin{aligned}
    |0\rangle \xrightarrow{U_R} U_R|0\rangle & = |\rho\rangle \xrightarrow{U_V(\vec{\theta})} U_A(\vec{\theta})|0\rangle \\
    & = U_V(\vec{\theta}) U_R |0\rangle\\
    & = U_V(\vec{\theta})|\rho\rangle\\
    & = |\psi(\vec{\theta})\rangle
    \end{aligned}
    $$

3.  **Evaulate cost function**: We encode our problem into a *cost function* $C(\vec{\theta})$ as a linear combination of Pauli operators, run on a quantum system. The cost function can be a physical system such as energy or spin and non-physical system. 

4.  **Optimize Parameters to get results**: Evalutaions are teken to a classical computer, where a classical optimizer analyzes them and chooses the next set of values for the variational parameters. If we have a pre-existing optimal solution, we can set it as an *initial point* $\vec{\theta_{0}}$ to *bootstrap* our optimization, which foster us to find the optimal solution faster.

5.  **Adjust ansatz parameters with results**: The entire process is repeated untill the classical optimizer's finalization criteria are met, and an optimal set of parameter valeus $\vec{\theta^{*}}$ is returned. The proposed solution state for our problem will then be $|\psi(\vec{\theta^{*}})\rangle = U_{A}(\vec{\theta^{*}})|0\rangle$.

--- 

## Variational theorem

A common goal of variational algorithms is to find the quantum state with the lowest or highest eigenvalue of a certain observable. A key insight we'll use is the variational theorem of quantum mechanics.


### Hamiltonian

In quantum mechanics, enery comes in the form of a quantum observable usually referred to as the *Hamiltonian*, $\hat{\mathcal{H}}$. Let's consider its spectral decomposition:

$$
\hat{\mathcal{H}} - \sum_{k=0}^{N-1} \lambda_{k}|\phi_{k}\rangle\langle \phi_{k}|
$$

where $N$ is the dimensionality of the spaec of state, $\lambda_{k}$ is the $k$-th eigencalue or physcially, the $k$-th energy, and $|\phi_{k}\rangle$ is the corresponding eigenstate: $\hat{\mathcal{H}}|\phi_{k}\rangle = \lambda_{k}|\phi_{k}\rangle$. 

The expected energy of a system in the (normalized) state $|\psi\rangle$ is 

$$
\begin{aligned}
\langle \psi|\hat{\mathcal{H}}|\psi\rangle & = \langle \psi|\bigg(\sum_{k=0}^{N-1}\lambda_{k}|\phi_{k}\rangle\langle \phi_{k}| \bigg)|\psi\rangle\\
& = \sum_{k=0}^{N-1}\lambda_{k}\langle \psi|\phi_{k}\rangle \langle \phi_{k}|\psi\rangle\\
& = \sum_{k=0}^{N-1}\lambda_{k}|\langle \psi|\phi_{k}\rangle|^{2}
\end{aligned}
$$

If we take into account that $\lambda_{0} \leq \lambda_{k}, \forall k$, we have:


$$
\begin{aligned}
\langle \psi|\hat{\mathcal{H}}|\psi\rangle & = \sum_{k=0}^{N-1}\lambda_{k}|\langle \psi|\phi_{k}\rangle|^{2} \geq \sum_{k=0}^{N-1}\lambda_{0}|\langle \psi|\phi_{k}\rangle|^{2}\\
& = \lambda_{0}\sum_{k=0}^{N-1}|\langle \psi|\phi_{k}\rangle|^{2}\\
& = \lambda_{0}
\end{aligned}
$$

Since $\{\phi_{k}\}^{N-1}_{k=0}$ is an orthonormal basis, the probability of measuring $|\phi_{k}\rangle$ is $p_{k} = |\langle \psi|\phi_{k}\rangle|^{2}$, and the sum is $\sum_{k=0}^{N-1}|\langle \psi|\phi_{k}\rangle|^{2} = p_{k} = 1$. Thus we expect energy of any systemis higher than the lowest energy or ground state energy:

$$
\langle \psi|\hat{\mathcal{H}}|\psi\rangle \geq \lambda_{0}
$$


This support us that for any normalized quantum state $|\psi\rangle$, we can find its minimal energy state (ground state) by finding its parametrized states $|\psi(\vec{\theta})\rangle$ which depend on a parameter vector $\vec{\theta}$.

We define a cost function that we want to minimize:

$$
\min_{\vec{\theta}} C(\vec{\theta}) = \min_{\vec{\theta}} \langle \psi(\vec{\theta}) | \hat{\mathcal{H}} | \psi(\vec{\theta}) \rangle \geq \lambda_0 .
$$

the minimum energy $\lambda_{0}$ can be found with corrosponding parameter vector $\vec{\theta}^{*}$ and its state $|\psi(\vec{\theta}^{*})\rangle = |\psi_{0}\rangle$.

### Variational theorem of Quantum Mechanics

If the normalized state $|\psi\rangle$ of a quantum system depends on a paramter vector $\vec{\theta}$, then the optimal approximation of the ground state, is the one that minimizes the expectation value of the Hamiltonian $\hat{\mathcal{H}}$:

$$
\langle \hat{\mathcal{H}}\rangle (\vec{\theta}):= \langle \psi(\vec{\theta})|\hat{\mathcal{H}}|\phi(\vec{\theta})\rangle
$$

The reason why the variational theorem is stated in terms of energy minima is that it includes a number of mathematical assumptions:

*   For physical reasons, a finite lower bound to the energy $E \geq \lambda_{0} > -\infty$ needs to exist, even for $N \rightarrow \infty$.
*   Upper bounds do not generally exist.

However, mathematically speaking, there is nothing special about the Hamiltonian $\hat{\mathcal{H}}$ beyond these assumptions, so the theorem can be generalized to other quantum observables and their eigenstates provided they follow the same constraints. 

## References 
[1]. Variational algorithms [https://quantum.cloud.ibm.com/learning/en/courses/variational-algorithm-design/variational-algorithms](https://quantum.cloud.ibm.com/learning/en/courses/variational-algorithm-design/variational-algorithms)