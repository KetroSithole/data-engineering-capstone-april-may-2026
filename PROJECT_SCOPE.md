# Project Scope: Retail Banking Customer 360 Data Warehouse

## 1. Background

You work in the data team of a South African retail bank. The bank's
systems were never designed to talk to each other, and the only thing
anyone has ever managed to get out of them is a single flat export: one
big activity file covering client details, product enrollments, CRM
contact history, and transactions, all mixed together. Nobody has ever
built a proper warehouse for this data. Reporting today is a person
manually pulling spreadsheets.

You have been asked to build the first version of a Customer 360 data
warehouse: a dimensional model that the BI team can connect Power BI or
Tableau to, fed by a repeatable SSIS ETL pipeline.

## 2. Objective

Design and build a star schema data warehouse in SQL Server, loaded by
SSIS, from the single raw extract provided in `data/raw/`. Use it to
answer the business questions in `docs/business_questions.md`.

This is a skills project. The point is not to reach one correct answer, it
is to show that you can:

* Profile a messy, denormalized source file and work out what is actually
  in it
* Recognize the different entities and event types hiding inside a single
  extract, and design sensible keys for each
* Design a sensible dimensional model: grain, dimensions, facts, and how
  they relate
* Build an SSIS package that lands, splits, and transforms the data
* Write SQL against your model to answer real business questions
* Explain and justify the decisions you made

## 3. What you are given

`data/raw/customer_activity_extract.csv` is a single comma delimited file,
exactly as it came out of the source system, not cleaned and not split
into tables. Every row is one event for one client. An `event_type` column
tells you whether the row is a product enrollment, a CRM interaction, or a
transaction, and a different subset of the remaining columns is populated
depending on which type it is. Client details such as name, contact
details, and signup date repeat on every row belonging to that client.

There are no ready made table identifiers in this file. There is a
client_number and an account_number, because a real extract still needs
something to key on, but there is nothing marking out separate
enrollment, interaction, or transaction records. Working out how to key
and deduplicate each entity is part of the exercise.

See `docs/data_dictionary.md` for a full column reference. Treat this as a
real source system handoff. Nobody has told you what is wrong with it.
Profiling it is part of the task.

`sql/00_create_source_tables.sql` gives you a `CREATE TABLE` statement for
a single `source` schema table matching the raw file, so you have a
consistent landing point to import into via SSIS, using a Flat File
Source. You are not required to use it as is. Adjust data types if your
profiling says otherwise, and justify the change.

## 4. What you must build

### 4.1 SSIS ETL

* One or more SSIS packages, built in Visual Studio or SSDT with the SSIS
  extension, that load the raw extract into SQL Server and populate your
  dimensional model.
* Use a staging layer, a raw load with minimal transformation, separate
  from the final dimensional model. Do not transform everything in a
  single monolithic step.
* Use a Conditional Split, or an equivalent approach, to separate the
  three event types out of the single source file before building your
  dimensions and facts from them.
* Handle at least: data type conversion, whitespace and casing cleanup,
  and invalid or missing values. Decide for yourself what counts as
  invalid here, profile the data first.
* Sequence your control flow correctly. A fact cannot load before the
  dimension it depends on exists.
* Your packages must be re runnable without duplicating data. Truncate and
  reload is acceptable for this exercise. Incremental loading is a bonus,
  not a requirement.

### 4.2 Dimensional model

Design a star schema, or a snowflake schema if you can justify it, in a
`dw` schema in SQL Server. At minimum you will need:

* A date dimension. Build it, do not carry raw date strings into your
  facts.
* A client dimension, built by deduplicating the repeated client details
  in the source file.
* A product dimension, built from the enrollment style rows.
* At least two fact tables at different, clearly stated grains, for
  example one fact per transaction and one fact per CRM interaction. You
  may combine them if you can justify the resulting grain.

You decide: how you generate surrogate keys for each dimension, since the
source file does not hand you any, which attributes belong on which
dimension, whether the client dimension needs to track history (SCD Type
1 versus Type 2, pick one and be able to explain why), and how you handle
the data quality issues you find, whether that is rejecting, correcting,
or flagging them with an unknown member row. All are valid approaches,
pick one and justify it.

Deliver an entity relationship diagram of your final model, using any tool
you like, alongside the DDL.

### 4.3 SQL analysis

Write the SQL queries that answer every question in
`docs/business_questions.md`, run against your dimensional model, not the
raw or staging tables. Include the SQL and the result for each.

## 5. Tech stack

* SQL Server, Developer or Express edition, plus SQL Server Management
  Studio
* SQL Server Integration Services, via SSDT or Visual Studio
* Any diagramming tool for the entity relationship diagram

## 6. Deliverables checklist

* `dw` schema DDL, dimensions and facts, with comments explaining key
  design decisions
* Entity relationship diagram of the final dimensional model
* SSIS project, package files and project file
* A short data quality note describing what you found in the raw file and
  how you handled each issue
* SQL scripts answering every business question, with results
* A short README explaining how to run your pipeline end to end

## 7. Suggested timeline

Two weeks, structured roughly as follows.

Days one and two: profile the raw extract, document the data quality
issues you find, and sketch the dimensional model on paper or in a
diagramming tool before writing any DDL.

Days three and four: build the staging load in SSIS, and write the `dw`
schema DDL.

Days five to seven: build the dimension and fact loads in SSIS, and test
the pipeline end to end.

Days eight and nine: write and validate the SQL for every business
question.

Day ten: write up your data quality notes and README, and finish the
entity relationship diagram.

## 8. Evaluation criteria

You will be assessed on the following.

**Correctness.** Does the model actually answer the business questions,
and do the numbers make sense? For example, no double counted transactions
from a bad join.

**Modelling judgment.** Sensible grain, sensible key choices, dimensions
and facts used correctly, and evidence that the design followed from
profiling the data rather than from whichever join happened to work first.

**ETL craftsmanship.** Is the SSIS package readable, does it fail loudly
on bad data rather than silently succeeding, and is it re runnable.

**Data quality handling.** Did you find the real issues in the file, and
did you make and justify a defensible decision about each one, rather than
deleting anything that looked unusual.

**Communication.** Can you explain your design decisions to a non
technical stakeholder in your write up.

There is deliberately no single correct schema. Two interns can both pass
with different, well justified designs.
