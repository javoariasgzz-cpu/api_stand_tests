# Urban Grocers — API Test Automation

## What is this?
Automated test suite for the Urban Grocers REST API, focused on validating the kit creation endpoint.
The goal was to verify that the `name` field enforces business rules correctly across valid, invalid, and edge case inputs.

## Product Under Test
Urban Grocers API — kit creation endpoint (`POST /api/v1/kits`).

## Objective
Verify that the API accepts valid `name` values with status 201 and rejects invalid ones with status 400, covering boundary values, data types, and special inputs.

## Scope
**Positive cases (expect 201):**
- 1 character
- 511 characters (upper boundary)
- Special characters (`"№%@",`)
- Spaces within the name
- Numbers as string type (`"123"`)

**Negative cases (expect 400):**
- Empty string
- 512 characters (over boundary)
- Missing `name` parameter entirely
- Integer type instead of string (`123`)

**Out of scope:** Other kit fields, authentication edge cases, performance testing.

## Tech Stack
- Python 3
- Pytest
- Requests library
- REST API / JSON validation

## Project Structure
├── configuration.py              # Base URL and endpoint paths
├── data.py                       # Reusable headers, user body, kit body
├── sender_stand_request.py       # HTTP functions: post_new_user(), post_new_client_kit()
├── create_kit_name_kit_test.py   # 9 automated test cases
└── README.md

## How to Run
**Prerequisites:**
- Python 3.8+

**Install dependencies:**
```bash
pip install pytest requests
```

**Run all tests:**
```bash
pytest create_kit_name_kit_test.py
```

## Key Decisions
- **Modular HTTP layer** — `sender_stand_request.py` centralizes all API calls, keeping test logic clean and reusable.
- **Dynamic request body** — `get_kit_body()` generates the kit body on the fly per test, avoiding hardcoded values.
- **Boundary value analysis** — test cases were deliberately designed around the field limits (1, 511, 512 characters) to catch edge case failures.
- **Separate assertion helpers** — `positive_assert()` and `negative_assert()` standardize validation logic and reduce duplication across tests.
- **Type validation included** — tested both string `"123"` (valid) and integer `123` (invalid) to verify the API enforces correct data types.

## Results
- 9 automated test cases covering boundary values, special inputs, null, missing parameters, and wrong data types
- API correctly accepted all valid inputs with status 201
- API correctly rejected all invalid inputs with status 400
- Notable finding: the API accepts numeric strings (`"123"`) as valid kit names

## What I Would Improve Next
- Expand coverage to other kit fields beyond `name`
- Add response body validation for error message content
- Test authentication edge cases (expired token, missing token)
