# Murshid Mini — Technical Documentation

## 1. System Design

Murshid Mini demonstrates a controlled application pipeline using Python, predefined responses, and synthetic data.

The main workflow is:

**User Input → Input Validation → Ticket Extraction → Ticket Validation → Routing → Tool Execution → Response Validation → Output**

The core uses a deterministic FakeClient rather than a real language model.

## 2. Structured Data

The application creates a ticket containing four fields:

| Field | Description |
|---|---|
| service | Requested service category |
| city | Riyadh, Jeddah, or unknown |
| request_id | Fictional reference number or null |
| needs_clarification | Boolean indicating a missing reference |

The `validate_ticket()` function checks field names, types, allowed values, and business rules.

## 3. Read-Only Tool and Authorization

The `check_status()` function retrieves application information from predefined synthetic records.

A reference must follow the `REF-0000` format. The application verifies record ownership using a separate demo session before returning status information.

Unauthorized users cannot see another user's application status.

The tool does not modify records, cancel applications, or access external systems.

## 4. Guardrails

The application includes input and output checks.

Input checks handle empty questions, excessive input length, selected instruction-override phrases, and certain unnecessary numeric data patterns.

Output validation verifies the response structure and expected field types.

These educational checks are limited and do not provide comprehensive security protection.

## 5. Cache Design

The application caches exact matches for supported general-service questions.

- Cache duration: 300 seconds.
- Cache key includes the question, language, model, prompt version, and directory version.
- Application-status queries are excluded from shared caching.
- Changing a version or reaching expiration prevents reuse of the previous cache entry.

## 6. Testing and Evaluation

The project includes ten predefined golden test cases covering normal requests, clarification, unauthorized access, unsupported requests, and invalid inputs.

The recorded final evaluation reported **10/10 passing cases** in mock mode.

The controlled cache experiment produced three mock-client calls without caching and one with caching, giving a 66.7% cache-hit rate.

Execution time is recorded in milliseconds. The core application makes no external model API calls.

## 7. Reproducibility

Open `notebooks/murshid.ipynb` in Google Colab with a Python 3 CPU runtime.

Run Cells 1–14 in order. Restart the runtime and rerun all cells to verify that execution does not depend on previous session state.

The generated reports are stored in `results/core/`.

## 8. Limitations and Future Work

The application relies on predefined responses, simulated authentication, and a small synthetic dataset. It is not production-ready.

Future improvements could include stronger authentication, comprehensive validation, additional test scenarios, and integration with a real language model.

**Design rationale:** Application-status questions are excluded from shared caching to avoid reusing user-specific information across sessions.
