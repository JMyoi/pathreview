## Week 7 — Issue selection

**Issue link:** [(https://github.com/ascherj/pathreview/issues/147)]

**Issue title:** [Resume section detection fails on text with leading whitespace]

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
[in the resume section of the ingestion pipeline, when text is extracted from pdfs, commonly preserves leading intentation or whitespace, causing an error and it fails, no sections are detected at all, comes back as empty]

**Branch name:** [fix/147-Resume-detection-fails-leading-whitespace]

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [link to commit documenting the reproduced issue]

**Reproduction summary:**
Ran the exact repro snippet from the issue against `ResumeParser.parse()` on indented text and confirmed `detected_sections` comes back as `[]` instead of `['Education', 'Skills']`. Also ran the three named failing tests (`test_parse_single_column_resume_text`, `test_parse_resume_no_work_experience`, `test_detect_sections`) via `pytest tests/unit/test_resume_parser.py` and confirmed all three fail for the same reason: the section-header regexes in `_detect_sections()` (ingestion/parsers/resume_parser.py) anchor on `^`/`\n` with no allowance for leading whitespace, so indented section headers (common in both the test fixtures and real PDF-extracted text) never match.

**PLAN.md link:** [link to PLAN.md in your fork]

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]