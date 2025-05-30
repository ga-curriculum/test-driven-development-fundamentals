<h1>
  <span class="headline">TDD Fundamentals</span>
  <span class="subhead">Setup</span>
</h1>

## Setup

You’ll create a new FastAPI project to build and test a package tracking simulation app. This app includes:

- A FastAPI backend with a mock database
- A static HTML frontend (`tracking.html`)
- Pytest for unit and integration tests
- Optional: Selenium for end-to-end tests

### 1: Create Your Project Folder

Open a terminal and run:

```bash
mkdir package-tracker-app
cd package-tracker-app
```

### 2: Set Up a Virtual Environment

Create and activate a virtual environment:

```bash
python -m venv venv
```

Activate it:

**macOS/Linux:**

```bash
source venv/bin/activate
```

**Windows:**

```bash
venv\Scripts\activate
```

> ✅ You should now see `(venv)` in your terminal prompt.

### 3: Install Dependencies

Use pip to install everything you’ll need:

```bash
pip install fastapi uvicorn pytest httpx selenium
```

This will install:

| Package    | Purpose                                                              |
| ---------- | -------------------------------------------------------------------- |
| `fastapi`  | Web framework for building the API                                   |
| `uvicorn`  | ASGI server to run the FastAPI app                                   |
| `pytest`   | For writing and running unit and integration tests                   |
| `httpx`    | Async test client for integration tests with async FastAPI endpoints |
| `selenium` | Automates browser interactions for end-to-end testing                |

### 4: Add Your Project Files

Create the following files inside your project folder:

- `main.py`
- `mock_db.py`
- `tracking.html`

### 5: Add Starter Code

#### `main.py`

```python
from fastapi import FastAPI, HTTPException
from fastapi.responses import HTMLResponse
from fastapi.staticfiles import StaticFiles
from mock_db import TRACKING_DATA

app = FastAPI()

app.mount("/", StaticFiles(directory=".", html=True), name="static")

@app.get("/track/{tracking_number}")
def track_package(tracking_number: str):
    if tracking_number in TRACKING_DATA:
        return {"status": TRACKING_DATA[tracking_number]}
    else:
        raise HTTPException(status_code=404, detail="Tracking number not found.")
```

#### `mock_db.py`

```python
TRACKING_DATA = {
    "PKG123": "In transit - Bahrain",
    "TRK456": "Delivered - Singapore",
    "GB789": "Customs clearance - UK",
    "MTG001": "Out for delivery - Montenegro",
    "USA999": "Delivered - New York"
}
```

#### `tracking.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>GlobalTrack: Package Tracker</title>
    <style>
      body {
        font-family: 'Segoe UI', sans-serif;
        margin: 0;
        background: #f0f4f8;
        color: #333;
      }

      .header {
        background-color: #0077cc;
        color: white;
        padding: 1rem 2rem;
        text-align: center;
      }

      .tracker {
        max-width: 400px;
        margin: 2rem auto;
        padding: 2rem;
        background: white;
        border-radius: 10px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      }

      .tracker h2 {
        margin-bottom: 1rem;
        color: #0077cc;
      }

      input[type='text'],
      button {
        width: 100%;
        padding: 0.75rem;
        margin: 0.5rem 0;
        font-size: 1rem;
        border: 1px solid #ccc;
        border-radius: 6px;
        box-sizing: border-box;
      }

      input:focus {
        border-color: #0077cc;
        outline: none;
      }

      button {
        background-color: #28a745;
        color: white;
        border: none;
        cursor: pointer;
        transition: background-color 0.2s ease;
      }

      button:hover {
        background-color: #218838;
      }

      #result {
        margin-top: 1rem;
        color: #0077cc;
        font-weight: bold;
      }
    </style>
  </head>
  <body>
    <div class="header">
      <h1>🌍 GlobalTrack</h1>
      <p>Track your international shipments in real time</p>
    </div>

    <div class="tracker">
      <h2>Track Your Package</h2>
      <input
        type="text"
        id="tracking-number"
        placeholder="Enter tracking number"
      />
      <button id="track-btn" onclick="trackPackage()">Track</button>
      <div id="result"></div>
    </div>

    <script>
      function trackPackage() {
        const trackingNumber = document.getElementById('tracking-number').value;
        fetch(`/track/${trackingNumber}`)
          .then((response) => response.json())
          .then((data) => {
            document.getElementById(
              'result'
            ).innerHTML = `<div id="status">Status: ${data.status}</div>`;
          })
          .catch((err) => {
            document.getElementById(
              'result'
            ).innerHTML = `<div id="status">Tracking number not found</div>`;
          });
      }
    </script>
  </body>
</html>
```

### 6: Run the App

Start your FastAPI server with Uvicorn:

```bash
uvicorn main:app --reload
```

Then visit [http://localhost:8000/tracking.html](http://localhost:8000/tracking.html) in your browser to try it out.

You're now ready to begin testing this app with unit, integration, and end-to-end test strategies!
