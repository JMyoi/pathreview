## Week 7 — Issue selection

**Issue link:** [(https://github.com/ascherj/pathreview/issues/147)]

**Issue title:** [Resume section detection fails on text with leading whitespace]

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
[`_detect_sections()` in resume_parser.py looks for section headers (Education, Skills, Experience, etc.) using patterns that require the header to sit at the very start of a line, with zero tolerance for leading spaces/tabs. PDF text extraction commonly preserves indentation, so real-world resume text almost always has some leading whitespace before headers — meaning detection silently returns an empty list instead of erroring, even though the sections are clearly there.]

What is "Section Detection" for?
when someone uploads a resume, the app scans the text looking for common section headers like Experience, Education, Skills, Projects, etc. It does this so it knows what sections to chunk based on different headers, "this chunk is skills, this is Experience". useful for context for later steps in the pipeline. 
The "whitespace" problem
Sections headers with literal leading whitespace, spaces, tabs, indentation, 

Education:              <- no leading spaces
- B.S. Computer Science

vs.

    Education:              <- 4 leading spaces before "Education"
    - B.S. Computer Science
we see that the second example has whitespace. 

Why whitespace breaks down and fails.
The code that looks for headers does something like "check if a line starts with exactly the word Education". That way of checing is strict and fails on whitespace. so even if there is a valid section header it will not detect it becaus of whitespace. 

**Branch name:** [fix/147-Resume-detection-fails-leading-whitespace]

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [https://github.com/JMyoi/pathreview/commit/3a9a188]

**Reproduction summary:**
Ran the exact repro snippet from the issue against `ResumeParser.parse()` on indented text and confirmed `detected_sections` comes back as `[]` instead of `['Education', 'Skills']`. Also ran the three named failing tests (`test_parse_single_column_resume_text`, `test_parse_resume_no_work_experience`, `test_detect_sections`) via `pytest tests/unit/test_resume_parser.py` and confirmed all three fail for the same reason: the section-header regexes in `_detect_sections()` (ingestion/parsers/resume_parser.py) anchor on `^`/`\n` with no allowance for leading whitespace, so indented section headers (common in both the test fixtures and real PDF-extracted text) never match.

**PLAN.md link:** [https://github.com/JMyoi/pathreview/blob/fix/147-Resume-detection-fails-leading-whitespace/PLAN.md]

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]