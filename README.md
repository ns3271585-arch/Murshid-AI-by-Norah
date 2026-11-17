Murshid AI — LLM Application Engineering
Developed by: Norah
Course: LLM Application Engineering (SDA-AIE-213)
Instructor: Meaad Al-Marri
Training context: SDAIA Academy
Project Overview
Murshid Mini is a fictional service assistant built as a four-day educational project. The core application demonstrates engineering around a model-shaped interface using a deterministic Python FakeClient—not a live language model. Independent homework notebooks demonstrate structured tool validation, error handling, and real local model inference with google/flan-t5-small.

This is a learning exercise, not a deployed government service, and it does not use real application records.
Problem, Users, and Scope
Murshid demonstrates how a service assistant can answer a small public FAQ and check fictional application statuses while validating requests and applying access controls. It is intended for students learning LLM application engineering.

The core notebook uses fixed responses and rules; it cannot understand arbitrary requests, access real government systems, authenticate real users, or provide production-grade security.
Four-Day Development
Day	Core application milestone
1	Synthetic FAQ, FakeClient, response contract, baseline tests
2	Structured tickets, schema validation, read-only status lookup and ownership checks
3	Educational input/output guardrails and ten golden tests
4	Cache for public FAQ, regression checks, metrics, and final report


All four core labs are in one cumulative notebook, not four separate projects.
Repository Contents
Core project: [murshid.ipynb](notebooks/murshid.ipynb)

Independent homework:
- [Day 1: Real-model inference](homework/day1_real_model.ipynb)
- [Day 2: Structured tool dispatch](homework/day2_tools.ipynb)
- [Day 3: Error handling](homework/day3_errors.ipynb)
- [Day 4: Real-model evaluation](homework/day4_evaluation.ipynb)
Results and documentation:
- [Core results and final report](results/core/)
- [Day 1 real-model report](results/day1/live_demo_result.json)
- [Day 4 real-model evaluation report](results/day4/evaluation_result.json)
- [Technical project notes](docs/project-notes.md)
Environment and How to Run
Core project
1. Open [the cumulative notebook](notebooks/murshid.ipynb) in Google Colab.
2. Select a Python 3 CPU runtime. No GPU, paid account, model download, or API key is required for the core notebook.
3. Restart the runtime, then use Runtime → Run all to execute its 14 cells in order.
4. Check the printed tests and inspect the generated JSON files under murshid_outputs/.
5. Download murshid.ipynb separately from the optional murshid_submission.zip, which contains the JSON results only.
The core project uses only the Python standard library. The separate real-model homework notebooks require package and model downloads, internet access, and a CPU runtime; follow their individual setup cells. Some Python environments may need an updated compatible version of SentencePiece.
Synthetic Data and Ticket Fields
The dataset uses hand-written fictional FAQ answers in Arabic and English, and these two synthetic application records:
Reference	Fictional owner	Status
REF-1001	demo-user-1	under_review
REF-1002	demo-user-2	approved


REF-9999 intentionally does not exist. Structured tickets contain service, city, request_id, and needs_clarification. The session is a trusted test fixture, not production authentication.
Example Requests
Input	Expected behavior
How can I check my application status?	Returns the predefined public FAQ response
Status inquiry with REF-1001	Returns under_review for demo-user-1
Status inquiry with REF-9999	Returns not_found
Status inquiry with REF-1002	Returns access_denied for demo-user-1
Cancellation request for REF-1001	Returns unsupported_action; no record changes


Testing and Observed Results
Test	Observed result
Day 1 baseline	All assertions passed
Day 2 request routing	5/5 cases passed
Day 3 golden cases	10/10 passed
Day 4 regression	10/10 passed
Mock calls without caching (three identical public questions)	3
Mock calls with caching (same test)	1
Cache hit rate	66.7%
Actual external inference API calls	0
Day 2 homework tool cases	7/7 passed
Day 3 homework caught errors	3 expected rejections; valid corrected input accepted
Day 4 real-model homework	3/3 reference-number keyword hits; all three answers manually reviewed


The cache is restricted to the two exact public FAQ questions, uses a 300-second time-to-live, and includes model, prompt, and directory versions in its key. Status inquiries are never cached. Mock-call counts are not real LLM provider savings or inference-latency benchmarks.

The real-model homework keyword score is only a proxy for the presence of the words reference and number—it is not semantic accuracy. Inference time varies across runtimes. The cost estimate in the core report is illustrative, not actual expenditure.
Limitations and Future Work
The core assistant has finite rule-based behavior, limited example guardrails, a synthetic session instead of real authentication, and no real service integration. Passing fixed test cases does not establish general reliability or production readiness. Improvements could include real authentication, controlled model tool-calling, more comprehensive privacy and adversarial testing, and semantic evaluation on a larger dataset.
Attribution
Developed as learner work for LLM Application Engineering — SDA-AIE-213, delivered in the SDAIA Academy training context, under instructor Meaad Al-Marri. SDAIA Academy on GitHub.

This educational repository does not imply official SDAIA endorsement, certification, or operation of a government service.
