# Murshid Mini — LLM Application Engineering

## 1. Project Overview

**Murshid Mini** is an educational Python application developed as part of the **LLM Application Engineering (SDA-AIE-213)** training program.

The project demonstrates how to design, validate, test, and evaluate an application built around a language-model-like interface.

It uses a deterministic `FakeClient` and synthetic service data to simulate an assistant that provides service information and application-status lookups.

**Important:** The core project does not use a real language model, access government systems, or process real user records. Real-model experiments are conducted separately in optional homework notebooks.

## 2. Problem and Objectives

Applications that integrate language models need more than text generation. They must handle invalid inputs, enforce business rules, protect information, validate responses, and measure behavior.

Murshid Mini demonstrates these engineering principles through:

- Structured input and output validation.
- Controlled request routing.
- Read-only tool execution and authorization checks.
- Basic input guardrails.
- Automated regression testing.
- Response caching and performance measurement.

## 3. Application Architecture

The application follows a controlled workflow:

**User Input → Input Guardrails → Ticket Extraction → Ticket Validation → Request Routing → Response Validation → Final Response**

The main components are:

| Component | Responsibility |
|---|---|
| `FakeClient` | Simulates a model interface using predefined responses |
| `extract_ticket()` | Extracts structured service information |
| `validate_ticket()` | Checks required fields, types, and business rules |
| `check_status()` | Retrieves synthetic application status after checking ownership |
| `route()` | Selects the appropriate application action |
| `guard_input()` | Rejects selected invalid or unsafe inputs |
| `validate_response()` | Verifies the response structure |
| `run_pipeline()` | Connects the validation and routing stages |
| `run_app()` | Adds caching and execution metrics |

## 4. Development Across Four Days

| Day | Implementation |
|---|---|
| Day 1 | Basic application structure, FakeClient, and response contract |
| Day 2 | Structured tickets, validation, routing, and read-only status lookup |
| Day 3 | Input/output guardrails and a ten-case regression test suite |
| Day 4 | Caching, metrics, regression testing, and final evaluation report |

All four stages are implemented in one cumulative notebook.

## 5. Dataset

The core project uses a small, manually created synthetic dataset.

Examples include:

| Reference | Status | Owner |
|---|---|---|
| REF-1001 | under_review | demo-user-1 |
| REF-1002 | approved | demo-user-2 |
| REF-9999 | Not found | — |

The application checks record ownership before returning status information. Unauthorized users cannot view another user's application status.

These identifiers are fictional and do not represent actual people or transactions.

## 6. Environment and How to Run

**Requirements:**

- Google Colab
- Python 3
- CPU runtime

The core project uses Python's standard library. No paid API, API key, GPU, external model download, or additional package installation is required.

**Steps:**

1. Open [murshid.ipynb](notebooks/murshid.ipynb) in Google Colab.
2. Select Python 3 with a CPU runtime.
3. Run all 14 code cells in order.
4. Confirm the tests complete successfully.
5. Inspect the generated JSON reports in `murshid_outputs/`.

To verify reproducibility, restart the runtime and run all cells again.

## 7. Testing and Evaluation

The application was evaluated using ten predefined test cases covering successful requests, missing information, unauthorized access, invalid input, and unsupported requests.

### Golden Test Results

| Metric | Observed result |
|---|---|
| Tests passed | 10 |
| Total tests | 10 |
| Pass rate | 100% |
| Execution mode | mock |

All ten predefined tests passed in the recorded run.

This result applies only to the selected mock test cases. It does not establish general model accuracy, security, or production readiness.

### Cache Experiment

The final experiment repeated the same supported service question three times.

| Metric | Without cache | With cache |
|---|---:|---:|
| Requests | 3 | 3 |
| Mock-client calls | 3 | 1 |
| Cache hits | 0 | 2 |
| Cache hit rate | 0% | 66.7% |

Caching reduced mock-client calls from three to one in the controlled experiment.

The shared cache is limited to predefined general-service questions. Application-status requests are excluded to avoid sharing user-specific status information.

Cache keys include model, prompt, and directory versions, and cached entries expire after a defined time.

### Execution Metrics and Cost

The application records execution time in milliseconds, request trace identifiers, cache hits, and call counts.

The core project made **zero external model API calls**.

Any API cost calculation included in the final report is hypothetical and does not represent actual spending or measured financial savings.

## 8. Optional Homework

Four independent homework exercises demonstrate additional concepts.

| Day | Exercise |
|---|---|
| Day 1 | [Real-model answer generation](homework/day1_real_model.ipynb) |
| Day 2 | [Structured tool-call validation](homework/day2_tools.ipynb) |
| Day 3 | [Python input-error handling](homework/day3_errors.ipynb) |
| Day 4 | [Real-model response evaluation](homework/day4_evaluation.ipynb) |

The real-model exercises use `google/flan-t5-small` locally in Google Colab. They are separate from the core mock application.

## 9. Project Files

| Path | Description |
|---|---|
| [notebooks/murshid.ipynb](notebooks/murshid.ipynb) | Complete four-day core project |
| [homework/](homework/) | Optional homework notebooks |
| [results/core/](results/core/) | Core experiment results and evaluation reports |
| [results/day1/](results/day1/) | Real-model demonstration results |
| [results/day4/](results/day4/) | Real-model evaluation results |

The main final artifacts include:

- [final_report.json](results/core/final_report.json)
- [day4_results.json](results/core/day4_results.json)
- [metrics.json](results/core/metrics.json)
- [synthetic_data.json](results/core/synthetic_data.json)

## 10. Limitations

Murshid Mini is an educational prototype with several limitations:

- The core uses a deterministic mock rather than an actual LLM.
- Authentication is simulated with predefined user identities.
- Request routing depends on limited rules and predefined inputs.
- Guardrails rely on basic checks and cannot prevent every unsafe input.
- The synthetic dataset is small and does not represent real service complexity.
- Successful tests do not demonstrate production readiness.

## 11. Future Improvements

Potential improvements include integrating a real language model through a controlled interface, implementing stronger authentication and authorization, expanding test coverage, and evaluating behavior using more varied Arabic and English questions.

A future implementation could also incorporate secure logging, additional input validation, and production-oriented monitoring.

## 12. Acknowledgments

Developed as part of **LLM Application Engineering — SDA-AIE-213**, delivered in the **SDAIA Academy** training context.

**Trainer:** Meaad Al-Marri

**Training reference:** [Murshid LLM Application Engineering](https://github.com/almiyead-rgb/murshid-llm-application-engineering-sda-aie-213)

**SDAIA Academy:** https://github.com/SDAIAAcademy

This project is an educational implementation based on the supplied training materials and does not claim production deployment or official endorsement.
