# 11 · JLPT Word Map

**Stack:** FastAPI, SQLite/PostgreSQL, Alembic, Sudachi NLP, Next.js, Sigma.js + AntV G6, Playwright

## Idea
Mine vocabulary from JLPT-related PDFs (N5–N1), analyse it with Japanese NLP and corpus
statistics, and learn through an interactive knowledge graph instead of isolated word lists.

## Hard parts
- **Scale in the browser.** The home view is a Learning Atlas of ~218k dictionary entries. Clusters
  and layout are precomputed offline (`build-atlas`) and the page loads only the **viewport**
  (level-of-detail), so it stays responsive.
- **Copyright model first.** Exam text is copyrighted, so every source is `REVIEW_REQUIRED` by
  default; only `ALLOWED` sources (with licence evidence) can be downloaded; exports contain
  vocabulary only, never corpus sentences; there is an audit trail.
- **Imperfect NLP.** Furigana, blanks and underlines break segmentation, so there are review screens
  and a scoped override-rule layer applied after tokenization.

## The part I am proudest of: a verification table
`docs/known-limitations.md` lists what was **run and passed** (66 backend tests incl. a real
PostgreSQL 16, 37 unit tests, 10 end-to-end tests, import of 218,165 entries with search under
0.6 s) versus what was **written but never executed** (Docker, Celery path, trigram indexes,
Tesseract, live web discovery). It also lists what is deliberately not built and says the auth is an
MVP that must be replaced before any public deployment.

## Lesson
Saying clearly what you did *not* verify makes everything else in the document believable.
Also: "NLP" in the README is explicitly Natural Language Processing, with no learning-effect claims.
