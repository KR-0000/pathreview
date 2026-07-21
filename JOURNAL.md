## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/157

**Issue title:** Relevance scorer "partial overlap" test fixture actually has full query overlap

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The test `test_query_with_partial_overlap` in `tests/unit/test_relevance_scorer.py` uses the query "Python Django web framework" against a chunk that actually contains all four of those terms, then asserts the resulting score is below 0.9. Since the scorer correctly returns 1.0 for full keyword coverage, this test fails against correct behavior rather than catching a real bug. The fix is to rewrite the fixture so the chunk only contains some of the query terms, making it a genuine partial-overlap case. This affects the relevance scoring logic in the RAG pipeline's test suite, not the scorer itself.

**Branch name:** test/157-relevance-scorer-partial-overlap-fixture

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

**Scope check ("Is this right for me?"):**
- I can explain the problem and expected behavior without re-reading the issue: yes, the fixture claims partial overlap but the query terms are all present in the chunk, so the scorer's 1.0 result is correct and the test assertion is wrong.
- Affected area: `tests/unit/test_relevance_scorer.py`, specifically `test_query_with_partial_overlap` and the relevance scorer it exercises.
- Definition of done: the fixture is rewritten so the chunk contains only some of the query terms, the test asserts a score genuinely below 0.9, and `pytest tests/unit/test_relevance_scorer.py -q` passes.
- Tier fit: this is my first open source contribution, so Tier 1 is the right call.
- Codebase readiness: I located and read the test function and reproduced the failure locally with `pytest tests/unit/test_relevance_scorer.py -q`.
- Claims check: 2 other students were on this issue when I claimed it, which felt manageable.
- Time estimate: realistic for Weeks 8–9 given the small, single-file scope.
- Blockers: No blockers yet.