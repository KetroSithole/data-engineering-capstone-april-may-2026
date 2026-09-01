# Customer 360 Intern Project

A self-contained data engineering capstone for the April–May 2026 intern
intake: build a star-schema data warehouse from a single raw banking
extract, using SSIS and SQL Server.

## What we're building

A retail bank has one messy export — client details, product enrollments,
CRM contact history, and transactions all mixed into one flat file — and
no proper warehouse. Interns take that single extract and deliver:

* an SSIS ETL pipeline that lands, splits, and cleans the raw file
* a dimensional model (`dw` schema: date, client, product dimensions and
  at least two fact tables) with an entity relationship diagram
* SQL that answers the business questions in `docs/business_questions.md`
* a data quality write-up and a README explaining how to run the pipeline

There is no single correct schema — it's a judgment exercise, graded on
correctness, modelling decisions, ETL craftsmanship, data quality
handling, and how well each intern can explain their choices. Full detail,
including the two-week suggested timeline and evaluation criteria, is in
[PROJECT_SCOPE.md](PROJECT_SCOPE.md).

**Start here:** [PROJECT_SCOPE.md](PROJECT_SCOPE.md) (also available as
[PROJECT_SCOPE.docx](PROJECT_SCOPE.docx))

## Folder structure

```
PROJECT_SCOPE.md              the brief: objective, deliverables, evaluation criteria
PROJECT_SCOPE.docx            same brief, Word format
data/raw/
    customer_activity_extract.csv    the one raw source file, comma delimited
docs/
    data_dictionary.md        column level definitions of the raw extract
    business_questions.md     the questions to answer with SQL against your model
sql/
    00_create_source_tables.sql   DDL for the landing/source schema
```

## For interns

Everything you need is in this repo. Read `PROJECT_SCOPE.md` first, then
`docs/data_dictionary.md`, then start profiling
`data/raw/customer_activity_extract.csv` before writing any DDL or SSIS.

Your own deliverables (SSIS project, `dw` schema DDL, ER diagram, SQL
answers, data quality notes) are not part of this starter repo — build
and submit them per your team's process.
