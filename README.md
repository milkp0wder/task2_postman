# BITS Academic Course Recommender — Postman Round 2

A single-notebook Google Colab implementation for recommending BITS courses from supplied academic sources and course handouts.

## What it does

The notebook follows this pipeline:

`Supplied handouts / academic PDFs → structured course facts → deterministic academic gate → natural-language preference matching → ranked recommendations`

Academic eligibility is checked before preference matching. The notebook does not let the language/query layer invent programme eligibility, prerequisites, attendance policies, or makeup rules.

## Main features

- Batch parsing of the supplied course-handout PDFs.
- Course-code normalization and cross-listed/duplicate-handout handling.
- Conservative extraction of title, instructor, topics, evaluation components, midsem/compre presence, attendance status, and makeup-policy text.
- Three-state preference facts where appropriate: `yes`, `no`, `unknown`.
- Student profile inputs for campus, admission year, programme, semester, completed/current courses, minor, and interests.
- Deterministic exclusion of completed/current courses.
- Source-backed timetable restriction handling when the supplied timetable is loaded.
- Lightweight TF-IDF-style topic matching with no external model download.
- Interactive notebook dashboard using `ipywidgets` instead of Streamlit/ngrok.
- Final integrity report and exported structured JSON.

## Source-of-truth rule

The supplied BITS Academic Regulations, Bulletin, current-semester timetable, and course handouts are treated as source material. The notebook does not fabricate academic rules when a supplied source does not establish them.

In particular, missing attendance information remains `unknown`; it is not converted into "no attendance".

Programme-specific DEL/HUEL/OPEL roles are only used when supported by a verified rules dataset. The notebook does not guess a course category from its subject area.

## Run in Google Colab

1. Open `BITS_R2_FINAL_COLAB.ipynb` in Google Colab.
2. Run the cells from top to bottom.
3. When prompted, upload the handout archive (`handouts(1).zip` or another ZIP containing the course PDFs).
4. Upload `bulletin.pdf`, `Academic-Regulations-2023.pdf`, and `timetable.pdf` when you want those sources included in the run.
5. In the final dashboard, enter the student's real completed/current course history.
6. Try queries such as:
   - `Suggest courses related to AI`
   - `Suggest courses related to semiconductors`
   - `Suggest courses with no midsem`
   - `I prefer project-based evaluation`
7. Use the final integrity report to confirm parsing and catalog counts.

## Dependencies

The notebook installs the two required packages itself in Colab:

```text
PyMuPDF>=1.24,<2
ipywidgets>=8,<9
```

The same dependencies are also listed in `requirements.txt` for reproducibility.

## Tested input

The final notebook was smoke-tested against the supplied 540-PDF handout archive:

- 540 / 540 PDFs processed
- 0 parser crashes
- 0 missing course codes
- 0 missing titles after timetable/catalog merge
- Duplicate/cross-listed handout cases handled
- Final integrity check passed

## Repository contents

```text
BITS_R2_FINAL_COLAB.ipynb   # main submission / executable notebook
README.md                   # setup, architecture, and usage
requirements.txt            # minimal Python dependencies
.gitignore                  # keeps runtime data and source archives out of Git
```

The large handout ZIP is intentionally not committed to the repository. Upload it when running the notebook. This keeps the repository lightweight and avoids turning the Git history into a large binary-data store.

## Round 2 architecture

```text
Supplied BITS Documents
        ↓
Preprocessing / Extraction
        ↓
Structured Course Data
        ↓
Student Profile
        ↓
Deterministic Academic Gate
        ↓
Eligible Course Set
        ↓
Natural-Language Preference Matching
        ↓
Ranked Recommendations + Source Information
```

## Submission note

The notebook is the executable implementation. The README documents the pipeline and reproducibility. No API key, token, or secret is required by the base notebook.
