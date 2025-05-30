<h1>
  <span class="headline">TDD Fundamentals</span>
  <span class="subhead">Understanding Test-Driven Development</span>
</h1>

**Learning objective:** Describe the purpose and key benefits of Test-Driven Development (TDD) in software development.

## What is test-driven development (TDD)?

Test-Driven Development, or TDD, is a development approach that uses automated tests as a guide for writing clean, functional code. In TDD, you begin by writing a test that describes the behavior you want. At first, this test will fail—because you have not implemented the feature yet. You then write the minimal code needed to make the test pass. This process is repeated in small cycles, allowing you to incrementally build your software while staying confident that your features work as expected.

> 💡 TDD flips the usual order of coding and testing. Instead of coding first and testing later, you set the expectations before writing your code.

## The Red-Green-Refactor cycle: The heart of TDD

The core of TDD is a repeating three-step cycle:

1. **Red:** Write a new test for the feature or behavior you want. The test should fail, showing that the desired feature does not exist yet.
2. **Green:** Write the simplest code possible to make the test pass. Do not spend time on code style or efficiency at this stage.
3. **Refactor:** Tidy up your code while making sure your test (and all previous ones) still pass. Make improvements for readability, maintainability, or performance.

You repeat this for each new feature, keeping the feedback loop quick and purposeful.

## TDD in practice

Let's walk through TDD with a simple function in Python using `pytest`.

Suppose you need a function to check if a number is even.

### Step 1: Write the test first (Red)

<code class="filepath">tests/test_utils.py</code>

```python
from utils import is_even

def test_is_even_for_even_number():
    assert is_even(4) is True

def test_is_even_for_odd_number():
    assert is_even(5) is False
```

When you run these tests now, they fail—because `is_even` doesn't exist yet.

### Step 2: Add minimal code to pass the test (Green)

<code class="filepath">utils.py</code>

```python
def is_even(n):
    return n % 2 == 0
```

Run the tests again. They now pass!

### Step 3: Refactor (if needed)

In this case, the code is already clear and concise. No refactor is necessary at this time.

## Why use TDD? Key benefits for your projects

### 1. Fewer bugs and faster debugging

Writing tests first helps catch mistakes early, before they become hidden issues that create headaches later.

> ♻️ By breaking code into small, testable steps, TDD acts like an ongoing debugging tool. You spot problems the moment they appear.

### 2. Designing with intention

TDD encourages you to plan the behavior of your code upfront. Each function, method, or API endpoint has clear expectations, promoting better design.

This often leads to:

- Smaller, well-named functions
- Clear interfaces between components

### 3. Safe refactoring

Refactoring means improving code without changing its behavior. With a comprehensive set of tests from TDD, you can confidently modify your code, knowing your tests will catch any accidental breakages.

### 4. Speed and maintainability in real-world projects

Some teams worry TDD might slow them down. However, TDD is an up-front investment that saves time in the long run. It reduces the time spent on debugging and helps document system behavior for future team members.

In the next few sections we're going to review the most common testing types you will encounter
