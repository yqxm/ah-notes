- #+BEGIN_PINNED
  Every _nonempty_ set of _nonnegative integers_ has a _smallest_ element.
  #+END_PINNED
- **Template for WOP Proofs**
	- To prove that "$P(n)$ is true for all $n \in  \mathbb{N}$"
		- Define the set $C$ *conterexamples* to $P$ being true. Specially, define
			- $$C ::= \{n \in \mathbb{N}| NOT(P(n))\}$$
				- $\{n|Q(n)\}$ means "the set of all elements $n$ for which $Q(n)$ is true.
		- Assume for proof by contradiction that $C$ is nonempty.
		- By WOP, there will be a smallest element $n$ in $C$.
		- Reach a contradiction somehow---often by showing that $P(n)$ is actually true by showing that there is another member of $C$ that is smaller than $n$. This is the open-ended part of proof task.
		- Conclude that $C$ must be empty, that is no counter example exist.
- **Prime Factorization Theorem**
	- **Theorem.** _Every positive integer greater than one can be factored as product of primes_
- ## Well Ordered Sets
	- A set of real numbers is _well ordered_ when each of its nonempty subsets has a minimum element.
	- #+BEGIN_NOTE
	  Notice that a set may have a minimum element but not be well ordered. For example: The set of nonnegative rational numbers.
	  #+END_NOTE
	- **Theorem.** _For any nonnegative integer_ $n$ _the set of integers greater than or equal to_ $-n$ _is well ordered._
	- **Corollary.** _Any set of integers with a lower bound is well ordered._
	- **Corollary.** _Any nonempty set of integers with an upper bound has a maximum element._
	- **lemma.** _Every nonempty  finite set of real numbers is well ordered._
	-