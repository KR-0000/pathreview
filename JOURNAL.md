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

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/KR-0000/pathreview/commit/8cd16b891866c9f212dd898db30d8ff513a7f570


**Reproduction summary:**
Ran `pytest tests/unit/test_relevance_scorer.py -q` and confirmed `test_query_with_partial_overlap` fails with `assert 1.0 < 0.9`. The chunk fixture contains all 4 query tokens ("python", "django", "web", "framework"), so `RelevanceScorer.score()` correctly returns 1.0 for full coverage, but the test asserts the score must be below 0.9 — confirming the issue is a bad test fixture, not a scorer bug.

**PLAN.md link:** https://github.com/KR-0000/pathreview/blob/test/157-relevance-scorer-partial-overlap-fixture/PLAN.md

**Walkthrough video (recommended):** (not recorded)

**Blockers or open questions:**
Still deciding exact new chunk wording for the fixture and whether to also spot-check `test_multiple_keyword_matches` and `test_score_ranges_from_zero_to_one` for the same kind of overlap-math mistake — not in scope for issue #157 but noticed while reading the file.