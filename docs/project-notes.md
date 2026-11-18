# Murshid AI – Technical Project Notes

## 1. System Overview

Murshid Mini is a fictional, educational service assistant developed for LLM Application Engineering (SDA-AIE-213).

The core application uses a deterministic `FakeClient` and Python's standard library. It demonstrates application engineering without relying on a live language model or external inference API.

## 2. Application Pipeline

The application follows this workflow:

1. Receive and validate the user's question.
2. Apply input guardrails.
3. Extract a structured ticket.
4. Validate the ticket schema.
5. Route the request to the appropriate service.
6. Check authorization before accessing a fictional application record.
7. Validate the response.
8. Return the answer with a trace ID and latency measurement.

The prompt template is an educational artifact and is not executed by the `FakeClient`.

## 3. Synthetic Dataset

The dataset contains predefined Arabic and English service FAQ responses and two fictional application records.

Each application record has:
- `owner`: fictional user identifier.
- `status`: fictional application status.

Each structured ticket contains:
- `service`: requested service category.
- `city`: extracted city or `unknown`.
- `request_id`: application reference.
- `needs_clarification`: whether additional information is required.

No real citizen data or government service records are used.

## 4. Validation and Authorization

Ticket validation checks required fields, data types, accepted values, and reference formats.

The read-only `check_status()` tool checks whether an application exists and whether the trusted session user owns it.

Unauthorized requests are rejected. Missing and unauthorized records receive the same outward answer to reduce information disclosure.

The session identity is a predefined testing fixture, not a production authentication mechanism.

## 5. Guardrails

The application includes educational input guards for:
- Empty or invalid inputs.
- Inputs exceeding 500 characters.
- Selected prompt-injection phrases.
- Selected patterns of unnecessary personal data.

Output validation checks that required response fields have appropriate types.

These checks are limited demonstrations and do not constitute comprehensive security controls.

## 6. Caching

Caching applies only to the two predefined public FAQ questions.

Cache entries use a time-to-live of 300 seconds and keys containing the question, language, model, prompt version, and directory version.

Application-status requests are not cached.

The controlled experiment recorded three mock-client calls without caching and one with caching, with a 66.7% cache hit rate.

## 7. Testing and Evaluation

The core notebook contains ten golden test cases covering FAQ responses, application-status lookup, clarification, unsupported actions, unauthorized access, blocked instructions, invalid input, and invalid schemas.

Results from the completed project:
- Day 1: Baseline checks passed.
- Day 2: Five routing cases passed.
- Day 3: Ten golden cases passed.
- Day 4: Ten regression cases passed.
- Cache experiment: 66.7% hit rate.
- Actual external inference API calls: zero.

The independent real-model exercises use `google/flan-t5-small`. The Day 4 homework achieved three keyword hits out of three test questions, with manual review confirming that the answers used the supplied reference-number information.

## 8. Limitations

The core application is a deterministic educational simulation, not a production AI service.

It has no real user authentication, live government-service integration, or comprehensive security guarantees.

The routing logic is rule-based, the evaluation dataset is small, and the hypothetical cost example is not an actual API charge.

## 9. Reproducibility

Open `notebooks/murshid.ipynb` in Google Colab with a Python 3 CPU runtime.

Restart the runtime and execute all 14 code cells in order. Review the printed checks and generated files in `murshid_outputs`.

The independent homework notebooks are located in `homework/` and must be run separately.

## 10. Possible Improvements

Future development could introduce real authentication, controlled LLM tool calling, stronger privacy safeguards, more comprehensive adversarial testing, and semantic evaluation on a larger dataset.

## 11. Final Evaluation and Recommendation

### Evaluation Setup

**Evaluation Date:** 2026-11-17

The final evaluation uses the fictional `synthetic-v1` dataset, a deterministic `FakeClient`, and the `mock` execution mode. The prompt version is `v1`, and the service directory version is `dir-v1`.

The evaluation includes ten golden test cases and a controlled before-and-after caching experiment.

### Results

All ten golden test cases passed (100%). The caching experiment reduced mock-client calls from 3 to 1, achieving a 66.7% cache hit rate.

The hypothetical cost is $0.0036 per request. This is an illustrative calculation, not actual API spending.

### Recommendation

Murshid successfully demonstrates structured validation, authorized read-only access, input guardrails, and exact-response caching in a controlled training environment.

Before broader use, the application would require real authentication, stronger privacy and security controls, a larger evaluation dataset, semantic answer-quality testing, and validation with a real language model.

The current results demonstrate correct behavior for the defined synthetic test cases only and do not establish production readiness.

