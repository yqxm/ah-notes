- **Test-first programming steps**
	- **Spec**: write a specification for the function.
	- **Test**: Write tests that exercise the specification. As you find problems , **iterate** on the spec and tests.
	- **Implement**: Write the implementation. As you find problems, **iterate** on the spec, the tests and the implementation.
- **Plan for iteration**
	- For a large specification, start by writing only one part of the spec, proceed to test and implement that part, then iterate with a more complete spec.
	- For a complex test suite, start by choosing a few important partitions, and create a small test suite for them. Proceed with a simple implementation that passes those tests, and then iterate on the test suite with more partitions.
	- For a tricky implementation, first write a simple brute-force implementation that tests your spec and validates your test suite. Then move on to the harder implementation with confidence that your spec is good and your tests are correct.
- **Choosing test cases by partitioning**
	- Divede the input space into subdomains, each consisting of a set of inputs.
		- Every input lies in exactly one subdomain, we choose one test case from each subdomain, and that's  a test suite.
	- **Include boundaries in the partition**
		- **E.g.**
			- 0 is boundary between positive numbers and negative numbers.
			- the maximum and minimum values of numeric types.
			- emptiness for collection types, like the empty string, empty list.
			- the first and last element of a sequence.
- **Documenting testing strategy**
	- partitions
	- subdomains
	- which subdomains each test case was chosen to cover
- **Black box testing**
	- It means choosing test cases only from the specification, not the implementation of the function.
- **Glass box testing**
	- It means choosing test cases with knowledge of how the function is actually implemented.
		- **E.g.**
			- The implementation select diffrent algorithms depending on the input.
			- The implementation keeps an internal cache that remembers the answers to previous inputs.
	- When doing glass box testing, your test case must not to require specific implementation behavior that isn't specifically called by the spec.
- **Coverage**
	- **_Statement coverage_**: Is every statement run by some test case?
		- 100% statement coverage is common goal, but that is rarely achieved due to unreachable defensive code.
	- **_Branch coverage_**: For every `if` or `while` statement in the program, are both the true and the false direction taken by some testcase?
		- 100% branch coverage is highly desirable.
	- **_Path coverage_**: Is every possible combination of branches -- every path through the program -- taken by some test case?
		- 100% path coverage is infeasible.
- **Unit testing**
	- Test a single module in isolation.
		- Don't let a unit test call other functions which are need to test.
- **Integration testing**
	- Test a combination of mudules, or even the entire program.
	- Use mock object for isolating a integratin test when you have to call other function.