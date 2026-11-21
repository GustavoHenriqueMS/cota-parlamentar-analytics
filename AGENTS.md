# AGENTS.md

## Project context
Portfolio project: an end-to-end data pipeline using the open data API of
the Brazilian Chamber of Deputies (https://dadosabertos.camara.leg.br/api/v2).
Focus: parliamentary expense quota (cota parlamentar).

Target audience of the final dashboard: citizens or journalists who want to
understand how the parliamentary quota is used.

Business questions live in `docs/business-questions.md`. That file is the
source of truth. Never invent or change business rules; if a definition is
missing or ambiguous, stop and ask.

## About the developer
The owner is learning data engineering and wants to understand every piece
of code in this repo. Prefer simple, readable solutions over clever ones.
Do not add abstractions, patterns or features that were not requested.

## Stack
- Python, `requests`, Pydantic, pandas, SQL, Streamlit.
- Database engine: NOT decided yet. Do not assume one. Ask before choosing.
- Orchestration and transformation tools (Airflow, dbt, etc.): NOT in scope yet.

## Working rules
- Do only what the task asks. Keep changes small and focused.
- Do not touch files outside the task scope.
- Do not install or add dependencies without asking first. If one is
  needed, explain why and wait for approval. Keep `requirements.txt` updated
  once a dependency is approved.
- Do not run `git commit` or `git push`. The owner makes the commits.
  You may suggest a commit message.
- Never delete files or data without asking.
- If the task is ambiguous or has a real trade-off, ask instead of deciding.

## Code conventions
- Code, comments, docstrings, variable/table/column names: English.
- Use type hints. Use Pydantic models to validate API responses.
- Comments should explain WHY, not WHAT.
- Keep functions small, with one responsibility.
- Raw API field names (e.g. `tipoDespesa`, `valorLiquido`) are kept as-is in
  the raw layer. Translate them to English only in later layers.

## API usage
- Always set a `timeout` on requests and check the response status.
- The API has request limits: add a small delay between calls and bounded
  retries. Never write infinite retry loops.
- Do not re-download historical data that does not change; prefer
  incremental logic when a task involves loading data.
- The API has no authentication key. Do not invent one.

## Data and security
- Never commit secrets, `.env` files or downloaded data files.
- Supplier documents may contain a CPF (personal data). Never print, log or
  display full CPF values. Mask them.
- Do not leave notebook outputs that expose personal data.
- Use `.env` for configuration; document variable names in `.env.example`.

## Notebooks
- Official notebooks go in `notebooks/` and are tracked by Git.
- Throwaway experiments go in `notebooks/scratch/` (ignored by Git).

## Language
- Repository code and data: English.
- README and commit messages: Portuguese.
- Commits follow Conventional Commits: `type(scope): description`
  (type in English, description in Portuguese, lowercase, no final period).

## When you finish a task
Reply with:
1. What you did (files created or changed).
2. What you did NOT do.
3. Decisions you made on your own, and any assumptions.
4. How to run or test it.
5. A suggested commit message.