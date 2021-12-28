# Philosophy of tests

## What makes a good test

### Run fast

Quick signal allow developer to iterate quicker. Also save resource when running in CI. Unit tests should less than 1 minute, E2E tests should less than 5 minutes. Cache huge tests assets if needed. &#x20;

### Reproduce locally

Isolate environment related flakiness. If a failure is not reproducible locally it will cause confusion and others no longer trust its fidelity. &#x20;

### Don't break often, not flaky

Choose a stable framework and improve your testing infra. Reduce unnecessary dependencies in your tests.

### Easy to read/understand

Help others understand and iterate on your tests when making changes. Give meaningful naming to your test, follow Arrange-Act-Assert pattern.

### Clear signal when failed

Use customized message in your assertion. Vague error message like "Key can not be found in Map XXX" will scare people away. Print stacktrace in actual code being tests is also helpful.

### Catch actual bugs

Don't only aim at code coverage. You test should catch real bugs. Think about what might went wrong in real scenario. &#x20;

### Survive refactoring&#x20;

Test against behavior not intermediate values. Ideally it should not block people from doing proper refactoring.

## Best practices&#x20;

### Add test to catch it in the future after fixing bug

Every time you found a bug is not catch by automated tests, think about how to write test to catch it next time. You test coverage will increase and future maintenance overhead is avoided.

### Share your effective testing strategy to the team

Strategy are different in per different project. Good strategy take iterations to be found. Share your own effective strategy to you team proactively and let other learn.

### Avoid too much isolation and mocking&#x20;

Excessive isolation or mocking may reduce your test quality, making them not catching actual bugs especially in integration level&#x20;

### Careful with generated tests

Generated tests are useful to enforce behavior and increase coverage. On the other hand, no matter how clever the test generator is, the generated tests will always be more naive than a human-written tests. Also think twice if your behavior can be catch within a single generic test or a linter rule to enforce test case. Duplicated tests are likely to happen with generated tests, it not only waste resource but also provides redundant signals when things went wrong, which cause extra distraction. Also test generation logic can get complicated and hard to know what tests are being generated.

### Keep tack of you tests

Maintain your tests, have a dashboard to track all your test's healthiness. Remove tests when it is no longer necessary. Tests should be updated as part of feature iteration .&#x20;

## Learning resource

#### Testing patterns at Facebook:

{% embed url="https://www.youtube.com/watch?v=_pnW-JjmyXE" %}

