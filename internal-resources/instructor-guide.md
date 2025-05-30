<h1>
  <span class="headline">TDD Fundamentals</span>
  <span class="subhead">Instructor Guide</span>
</h1>

## Understanding TDD

### Delivering the microlesson

- **Kick off** by asking learners if they have ever caught a mistake because they double-checked their work before considering something “done.” Link this to the core idea of TDD.
- Use the trip preparation or recipe analogies as you introduce Red-Green-Refactor.
- Walk through the hands-on Python example. Invite learners to predict what will happen at each phase (Will the test pass? What happens if we forget to implement a branch?).
- Encourage discussion in the mini-challenge. In pairs or small groups, have learners respond to the specific analysis prompts. Support them as needed to connect TDD cycles to tangible benefits.

---

## Creating Effective Unit Tests

**Key talking points:**

- Remind students they’re testing a pure Python function, not the route or HTML.
- Help troubleshoot import issues if students don’t structure the path to main.py correctly.
- Encourage descriptive test names (test_valid_tracking_number_returns_status) to support self-documenting code.
- Push students to write one test per behavior.

---

## Implementing Integration Tests

**Prep:**
Make sure students have their `main.py` and `test_main.py` files in the same directory.

**Key talking points:**

- Integration tests = behavior + flow
- Highlight how `TestClient` mimics a real user
- Emphasize that these are not end-to-end browser tests, but they do test across backend layers

**Optional Extension:**
Add a test that uses a `GET` endpoint or builds on query parameters.

---

## End-to-End Testing

- Ensure students have a working `track.html` + FastAPI app before starting.
- For support: help students update paths, port numbers, or fix common Selenium/ChromeDriver issues.
- Encourage explicit waits instead of fixed delays.
- Debrief by comparing failures from the browser vs. backend.

---

## Comparing Testing Strategies

---

🏗️ **Under Construction**

We are constantly working to improve our resources for instructors and students.

**Have something to contribute to this Instructor Guide?** [Let us know](https://pages.git.generalassemb.ly/modular-curriculum-all-courses/universal-resources-internal/module-feedback.html).
