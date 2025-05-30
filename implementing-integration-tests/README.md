<h1>
  <span class="headline">TDD Fundamentals</span>
  <span class="subhead">Implementing Integration Tests</span>
</h1>

**Learning objective:** Implement an integration test to check the interaction between two or more components in a system.

## Why integration tests matter: Seeing the full system in motion

Unit tests help confirm individual parts of your system work in isolation. But when building real applications—like our **Package Tracker App**—it’s not enough to verify each piece works on its own. We also need to ensure they work correctly **together**.

> 💡 A unit test says the “tracker lookup function works.” An integration test says, “When a user enters a tracking number in the form, the system fetches and displays the correct status.”

Integration tests:

- Simulate how real users and services interact with your app
- Reveal connection problems across layers (UI → API → logic)
- Validate workflows, not just functions

## Unit vs Integration: Quick Comparison

| Test Type        | What It Tests                   | When to Use                                |
| ---------------- | ------------------------------- | ------------------------------------------ |
| Unit Test        | One function or method          | To check logic in isolation                |
| Integration Test | Multiple parts working together | To test real data flow and system behavior |

## Testing the `/track` endpoint

The **Package Tracker App** uses:

- A static `mock_db` file for tracking numbers (simulated database)
- A `/track` route that checks the number and returns a status

In `main.py`, we have:

```python
from fastapi import FastAPI, HTTPException
from fastapi.responses import HTMLResponse
from fastapi.staticfiles import StaticFiles
from mock_db import TRACKING_DATA

app = FastAPI()

app.mount("/static", StaticFiles(directory=".", html=True), name="static")

@app.get("/track/{tracking_number}")
def track_package(tracking_number: str):
    if tracking_number in TRACKING_DATA:
        return {"status": TRACKING_DATA[tracking_number]}
    else:
        raise HTTPException(status_code=404, detail="Tracking number not found.")
```

We want to verify:

- A `GET` request to `/track` with a known tracking number returns the correct status
- The endpoint behaves correctly with unknown values

<div class="activity guided-walkthrough">
  <h2 class="title">Integration Test for the Package Tracker</h2>
  <span class="minutes">15 min</span>
</div>

Let's verify that the `/track` endpoint correctly returns statuses for valid tracking numbers and a fallback message for invalid ones.

### Step 1: Open the `main.py` file

You should already have:

- A FastAPI app
- A static `mock_db` file (simulated database)
- A `/track` `GET` endpoint

```python
from fastapi import FastAPI, HTTPException
from fastapi.responses import HTMLResponse
from fastapi.staticfiles import StaticFiles
from mock_db import TRACKING_DATA

app = FastAPI()

app.mount("/static", StaticFiles(directory=".", html=True), name="static")

@app.get("/track/{tracking_number}")
def track_package(tracking_number: str):
    if tracking_number in TRACKING_DATA:
        return {"status": TRACKING_DATA[tracking_number]}
    else:
        raise HTTPException(status_code=404, detail="Tracking number not found.")
```

### Step 2: Create a new test file

Create a file called 📄 `test_main.py` in the same directory.

Let's add the following test code:

```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_valid_number():
    response = client.get("/track/GB789")
    assert response.status_code == 200
    assert response.json() == {"status": "Customs clearance - UK"}
```

### Step 3: Add another test for an invalid number

```python
def test_invalid_number_returns_404():
    response = client.get("/track/BAD999")
    assert response.status_code == 404
    assert response.json()["detail"] == "Tracking number not found."
```

### Step 4: Run your integration tests

Activate your virtual environment and run:

```bash
pytest test_main.py
```

️✅ You should see both tests pass. If not, check for typos or case mismatches.

## Recap

- Integration tests confirm that different parts of your app (HTML form, FastAPI route, and logic) work together
- You used `TestClient` to simulate a browser `GET` request
- You wrote clear assertions against the real HTML returned by the app

<div class="activity knowledge-check">
  <h2 class="title">Let's Discuss</h2>
  <span class="minutes">2 min</span>
</div>

- Why would a unit test alone not catch a broken form-to-endpoint flow?
- What happens if the form changes but your backend test still passes?
- What’s one new integration test you might add for the Package Tracker App?
