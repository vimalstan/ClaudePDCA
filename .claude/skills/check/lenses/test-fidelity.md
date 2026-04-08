# Test Fidelity Lens

- Do test mocks match the real class/model definitions, not just the code under test?
- Are permissive mock types used where strict mocks should be?
- For relationship-traversal code, is there at least one test with real model instances?
- Do integration tests mock away the exact function that could contain the bug?
- If a mock's shape perfectly mirrors the code under test, does it also match the model definition?
- Are tests testing behavior or implementation details?
- Is test coverage proportional to risk?
