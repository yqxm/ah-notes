# TDD

## rules

- Write new code only if you first have a failing automated test
- Eliminate duplication

## steps

1. Add a little test
2. Run all tests and fail
3. Make a little change
4. Run the tests and succeed
5. Refactor to remove duplication

TDD is not  about taking teensy tiny steps, it's about being able to take teensy tiny steps

## Triangulation

- Translated a design objection(side effects) into a test case that failed because of the objection.
- Got the code to compile quickly with a stub implementation.
- Made the test work by typing in what seemed like right code.
