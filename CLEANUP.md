# Cleanup

This note is the record of what changed between the first portfolio list and this public repository.

## What was broken

The first project list linked to private repositories and to folders that existed only on a local disk. A reviewer without access got a 404, and a local path was not something they could open.

These showcase folders had not been pushed:

- ZIP code sale-price analysis
- Telecom customer churn
- Tariff announcement market reactions
- American childcare affordability
- North Carolina school finance
- Movie recommendation
- TSA complaint trends

The combined-cycle power-plant final was already in a private repository, next to a textbook clone that is not part of this work.

The housing listing file is about 171 MB. That is over GitHub's 100 MB file limit, so it cannot be committed.

The school-finance notebook had a Census Bureau API key hardcoded in a cell.

## What stayed out

The original course repositories stay private. They still hold weekly assignments, draft milestones, the textbook clone, and an untracked key file. None of that was copied here.

The Census key was removed from the public school-finance notebook. That notebook now stops until `CENSUS_API_KEY` is set in the environment. Request a free key at https://api.census.gov/data/key_signup.html.

The housing listings file stays untracked. Download steps are in `projects/zip-code-sale-price-prediction/DATA.md`.

## What was renamed

Finished projects were copied into this repository under subject folder names. The portfolio page, the project list, and the README reproduction sections link here. They do not cite course codes or a local disk path.

Notebook title cells, and some PDFs, may still name the course. Those are the original write-ups. The path a reviewer follows does not.
