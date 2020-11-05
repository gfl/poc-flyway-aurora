# Schema Migration Investigation

## Problem to solve

DB schema evolves. How do you:
* Track changes
* Ensure changes are applied
* Check what changes have been already applied

## Options

### Ad-hoc solution

* Keep versioning DDL files with a sequence.
* Keep a table in the database with the metadata.
* Create script that checks the metadata table and runs the pending DDL.

####Advantages:

* Can you adapt it to your custom needs/requirements.

####Disadvantages:

* Need a bit of time to implement the logic.
* Fewer features than the existing solutions.

### Flyway

https://flywaydb.org/documentation

#### Usage Example:

To initalise or migrate the schema of the database to the latest version just run:

```
mvn flyway:migrate
```

It creates/updates the `flyway_schema_history` table will all the metadata regarding the versions that have been applied to the database.

|installed_rank|version|description|type|script|checksum|installed_by|installed_on|execution_time|success|
|--------------|-------|-----------|----|------|--------|------------|------------|--------------|-------|
|1|2.0|<< Flyway Baseline >>|BASELINE|<< Flyway Baseline >>||null|2020-11-05 15:30:00|0|true|
|2|3|create pet table|SQL|V3__create_pet_table.sql|-473798765|postgres|2020-11-05 15:30:10|23|true|

#### Advantages:

* Popular Open Source project (Apache license).
* Supports Redshift.
* Runs with the app at startup or as separate tool.
* Java integrations (MVN and Gradle).
* Use major.minor version for maintenance.
* Environment-specific migrations (by providing additional migration scripts locations)
* Easy setup Flyway on an existing database schema (baseline functionality).
* Squashing migrations automatically: 
    * Define squashed migrations at some point in time on branch
    * On deploy: merge, change table name and baseline to appropriate version.

#### Disadvantages

* No support for undoing migrations on the community edition. 
Feature only available in the paid version.


