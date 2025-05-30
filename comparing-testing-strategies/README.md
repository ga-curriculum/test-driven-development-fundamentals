<h1>
  <span class="headline">TDD Fundamentals</span>
  <span class="subhead">Comparing Testing Strategies</span>
</h1>

**Learning objective:** Compare unit, integration, and end-to-end tests in terms of scope, speed, reliability, and use case.

## Why think in layers?

Each type of test—unit, integration, and end-to-end (E2E)—has its own job. They work together like layers in a strong safety system.

## Comparing test types

Below, let’s explore each layer through the lens of real application development—including FastAPI, pytest, and Selenium, all tools you have hands-on experience with.

| **Test type** | **Scope**             | **Speed** | **Reliability** | **Main use cases**               |
| ------------- | --------------------- | --------- | --------------- | -------------------------------- |
| Unit          | Single function/class | Fast      | Very high       | Logic, utilities, validation     |
| Integration   | Cross-component       | Medium    | High            | APIs, database, service wiring   |
| End-to-end    | Full user journey     | Slowest   | Variable        | Core product flows, user success |

> Integration test reliability is high when test data and environments are tightly controlled.

## Unit tests: `pytest`

- **Purpose:** Test the smallest parts—functions or methods—in total isolation.
- **Typical scope:** Only the code you wrote, no external systems.
- **Reliability:** Almost no flakiness.

### Example: Testing a function

```python
def test_valid_tracking_number_returns_status():
    # Arrange
    tracking_number = "USA999"
    # Act
    result = track_package(tracking_number)
    # Assert
    assert result == {"status": "Delivered - New York"}
```

> 🏆 Unit tests like this give you instant feedback whether a small piece of logic works—ideal for correctness and fast iteration.

## Integration tests: `pytest`

- **Purpose:** Confirm multiple components (like a web API and database) work together as expected.
- **Typical scope:** Two or more interacting pieces, often with real dependencies (such as databases).
- **Reliability:** High, but can fail if setup/teardown is incomplete.

### Example: Verifying your FastAPI endpoint returns the right data

```python
def test_valid_number():
    response = client.get("/track/GB789")
    assert response.status_code == 200
    assert response.json() == {"status": "Customs clearance - UK"}
```

> 😎 Integration tests simulate real requests, catching issues where different parts communicate—much like checking that a relay race team can pass the baton without dropping it.

## End-to-end (E2E) tests: `Selenium`

- **Purpose:** Confirm your application works from start to finish, as a real user would experience it.
- **Typical scope:** All layers—frontend, backend, and database included.
- **Reliability:** Most sensitive to outside factors (slow network, browser quirks).

### Example: Automating a full user journey with Selenium

```python
def test_track_package():
    driver = webdriver.Chrome()
    try:
        driver.get("http://localhost:8000/static/tracking.html")
        search_box = driver.find_element(By.ID, "tracking-number")
        search_box.send_keys("PKG123")
        driver.find_element(By.ID, "track-btn").click()
        status_div = WebDriverWait(driver, 10).until(
            EC.visibility_of_element_located((By.ID, "status"))
        )
        assert "In transit - Bahrain" in status_div.text
    finally:
        driver.quit()
```

> 💡 E2E tests catch the gaps that only appear when everything is in motion—like a “full system” dress rehearsal.

<div class="activity discussion">
  <h2 class="title">Match a Test to a Scenario</h2>
  <span class="minutes">5 min</span>
</div>

### Scenarios

a. You have a `calculate_discount(cart_total, user_status)` function that returns a percentage off based only on the provided arguments.

b. You want to verify that your `/checkout` FastAPI endpoint returns a `200` status code and writes an order record to your test database.

c. You want to confirm that when a user logs in and places an order through your web frontend, the confirmation page displays the correct order ID.

d. You have a helper that formats datetime objects as readable strings for order receipts.

e. You want to ensure your `/orders` endpoint correctly lists only the orders associated with the logged-in user.
