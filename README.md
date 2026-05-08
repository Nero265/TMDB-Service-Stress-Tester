# TMDB Service Stress Tester

A Python script for load testing a local TMDB-based REST API service — simulating concurrent requests and measuring response times under stress conditions.

## Overview

This tool was built to test the reliability and performance of a locally running TMDB search service. It spawns multiple threads that simultaneously fire search queries at the API, then reports success/failure counts and total execution time.

This is useful for:
- Identifying how a service behaves under concurrent load
- Detecting race conditions or slow endpoints
- Measuring baseline response times before and after optimization

## How It Works

The script defines a list of movie/book search queries and sends each one **5 times concurrently** using Python's `threading` module — resulting in 35 simultaneous HTTP requests to the local server.

Each thread:
1. Constructs a URL-encoded search request
2. Sends it to `http://localhost:5000/search`
3. Measures and prints the response time
4. Records success or failure in a thread-safe counter

## Usage

Make sure the TMDB service is running locally on port 5000, then:

```bash
python stress_test.py
```

### Example Output

```
Pokrecemo 35 zahteva ..

[OK]     'Inception' -> 0.142s (status: 200)
[OK]     'Interstellar' -> 0.155s (status: 200)
[ERROR]  'Avatar The Way of Water' -> <URLError>
...

======== Rezultati ========
 Uspesnih : 33
 Gresaka  : 2
 Ukupno   : 35
 Vreme    : 0.873s
===========================
```

## Tech Stack

- **Python 3** — standard library only (`threading`, `urllib`, `time`)
- No external dependencies required

## Key Concepts Demonstrated

- **Multithreading** — concurrent request dispatch using `threading.Thread`
- **Thread safety** — shared result counters protected with `threading.Lock`
- **HTTP communication** — using `urllib.request` for GET requests
- **Performance measurement** — per-request and total elapsed time tracking

## Notes

- The target server (`localhost:5000`) must be running before executing the script
- Query list and repetition count can be modified at the top of the script
- Currently configured for a search endpoint that accepts a `?query=` parameter
