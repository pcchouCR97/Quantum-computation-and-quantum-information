## Definition of stabilizer codes

An $n$- qubit stabilizer code is specified by a list of $n$-qubit Pauli operations, $P_{1},...,P_{r}$. These operations are called *stabilizer generators* in this context, and they must satisfy the following three properties.

1.  The stabilizer generators all *commute* with one another.

$$
P_{j}P_{k} = P_{k}P_{j} \ (\text{for all} j,k \in \{1,...,r\})
$$

2.  The stabilizer generators form a *minimal generating set.*

$$
P_{k} \notin <P_{1},...,P_{k-1},P_{k+1},...,P_{r}> (\text{for all} \k \in \{1,...,r\})
$$


3.  At leasr one quantum state vector is fiexed by all of the stabilizer generators.

$$
-I^{\otimes n} \notin <P_{1},...,P_{r}>
$$