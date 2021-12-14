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

## Imposter

When the object you have doesn’t behave like you want,make another object with the same external protocol (an Imposter), but a different implementation.

## Polymorphism

Any time you are checking classes explicitly, you should be using polymorphism instead. 
