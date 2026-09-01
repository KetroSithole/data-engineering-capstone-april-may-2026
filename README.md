# Customer 360 Intern Project

A self contained data engineering exercise: build a star schema data
warehouse from a single raw banking extract using SSIS and SQL Server.

**Start here:** [PROJECT_SCOPE.md](PROJECT_SCOPE.md) (also available as
[PROJECT_SCOPE.docx](PROJECT_SCOPE.docx))

## Folder structure

```
intern-project/
    PROJECT_SCOPE.md              the brief: objective, deliverables, evaluation criteria
    PROJECT_SCOPE.docx            same brief, Word format
    data/raw/
        customer_activity_extract.csv    the one raw source file, comma delimited
    docs/
        data_dictionary.md        column level definitions of the raw extract
        business_questions.md     the questions to answer with SQL against your model
    sql/
        00_create_source_tables.sql   DDL for the landing/source schema
    _instructor_only/             not for interns, see below
```

## For interns

Everything you need is in this folder except `_instructor_only/`. Read
`PROJECT_SCOPE.md` first, then `docs/data_dictionary.md`, then start
profiling `data/raw/customer_activity_extract.csv`.
