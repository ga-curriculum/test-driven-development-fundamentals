<h1>
  <span class="headline">TDD Fundamentals</span>
  <span class="subhead">Creating Effective Unit Tests</span>
</h1>

**Learning objective:** Write a unit test to verify the behavior of a small, isolated function or component.

## Why Unit Tests Matter in Real Projects

In the Package Tracker app, your backend contains logic that checks if a tracking number exists and returns the shipment status. That logic is a perfect candidate for a unit test—a test that checks a single, focused behavior in your code.

Good unit tests make your code:

- Easier to maintain
- Easier to debug
- Easier to trust as your project grows

> 💡 Think of unit tests like testing the brakes on a car: you want to confirm each part works on its own before going on the road.

## What Makes a Good Unit Test?

In this project, strong unit tests should be:

- **Single-purpose**: Only test one thing at a time
- **Repeatable**: Always give the same result
- **Independent**: Don’t rely on live APIs or real databases
- **Clear**: Easy for your teammates to read and understand

## The AAA Pattern: Arrange, Act, Assert

To keep unit tests clean and readable, we recommend using the AAA pattern which stands for:

- **Arrange** – Prepare your inputs (a sample tracking number)
- **Act** – Call the function you want to test
- **Assert** – Check that the output is what you expected

> You'll see this demonstrated in the examples below.

<div class="activity guided-walkthrough">
  <h2 class="title">Write Unit Tests for the Package Tracker</h2>
  <span class="minutes">15 min</span>
</div>

In this exercise you'll be using the starter code for GlobalTrack - a simulated package tracking application.

You’ll create a test file and write 1–2 unit tests for the `lookup_tracking_number` function.

### Step 1: Create a test file

Create a new Python file called: 📄 `test_tracker.py`

### Step 2: Write your first unit test

```python
import pytest
from fastapi import HTTPException
from main import track_package

def test_valid_tracking_number_returns_status():
    # Arrange
    tracking_number = "USA999"
    # Act
    result = track_package(tracking_number)
    # Assert
    assert result == {"status": "Delivered - New York"}
```

> We can verify the correct data response by looking at the `mock_db.py` file.

### Step 3: Add another test for a missing number

```python
def test_invalid_tracking_number_raises_http_exception():
    # Arrange
    tracking_number = "ZZZ999"
    # Act & Assert
    with pytest.raises(HTTPException) as exc_info:
        track_package(tracking_number)
    assert exc_info.value.status_code == 404
    assert exc_info.value.detail == "Tracking number not found."
```

### Step 4: Run your tests

With pytest installed, run:

```bash
pytest test_tracker.py
```

✅ You should see two passing tests.

## Recap

In this exercise we:

- Isolated a single function (lookup_tracking_number)
- Tested two possible behaviors (valid and invalid input)
- Used clear test names to describe what the function is supposed to do

<div class="activity discussion">
  <h2 class="title">How did it go?</h2>
  <span class="minutes">2 min</span>
</div>

How did using the Arrange-Act-Assert pattern help you structure your tests?
How would you test for edge cases (like lowercase tracking numbers or empty strings)?
What are some benefits of testing logic separately from the FastAPI route itself?
