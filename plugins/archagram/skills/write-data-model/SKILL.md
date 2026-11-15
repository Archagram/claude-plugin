---
name: write-data-model
description: Write an Archagram data model, the tables, columns and keys of a database, or the business concepts of a domain, with their relations and the state machines of a status. Use when the user asks to document a database in Archagram from its scripts, migrations or mapping code, to model the concepts of a domain, to describe the states something moves through, or runs /archagram:data-model.
user-invocable: false
---

# Write a data model

A data model holds the schemas, tables, views, columns and keys of a database, or, as a concept model, the business concepts of a domain and how they relate. An entity may carry state machines: the statuses it takes, the permitted changes and who may make each one. It is its own file beside the architecture and never part of the model.

Tools used: `list_projects`, `list_datamodels`, `get_notation_reference`, `read_notation`, `create_datamodel`, `read_datamodel`, `validate_datamodel`, `apply_datamodel_edits`, `write_datamodel`, and `get_guide` where the connector offers it.

## 1. Choose the level

- A data model (the default) describes a real database. Write it from its source, never from memory.
- A concept model (level `concept`) describes what the business talks about: product, article, order. Write it from what the user says and the documents of the domain, and keep storage details out of it.

Ask which database or domain it covers. For a very large database, agree which schemas or areas come first.

## 2. Find the schema in the code

With file access, as in Claude Code:

1. **Find the source of the structure**: create scripts, migrations, a schema export, or the mapping code, such as a Prisma schema, Entity Framework configurations, Django models or Hibernate entities. Where migrations build the schema step by step, work out the tables as the latest one leaves them.
2. **Read the keys**: primary keys, unique columns and the column each foreign key points at.
3. **Find the statuses**: an enum, a lookup table or a set of constants behind a status column gives the states and their stored values; the code that changes the status gives the permitted changes and who may make each.

Read files only. Connect to a live database only when the user asks for it, and then read its structure, never its rows. In a chat without file access, work from the scripts or the export the user shares.

## 3. Read what exists

1. `list_datamodels` shows the data models the project has. Extend the one that covers this database or domain instead of starting a second.
2. `get_notation_reference` with `topic: "datamodel"` has the grammar and an example. Follow it rather than memory.
3. `read_notation` gives the id of the component the model describes, such as the database, and of the stakeholders and components a state change can name.
4. Where the connector offers `get_guide`, read the `data-models` guide as well. It is the server's current word on this job, and it wins where this skill differs.

## 4. Write it

- Link the model to the architecture: the component on the header, and on an entity when another component owns it.
- Group tables in schema blocks as the database does. Names keep their case.
- A column is one line: its name, its type without spaces, such as `nvarchar(200)` or `decimal(18,2)`, whether it is a key, unique or nullable, the column a foreign key points at, and a short description where the name does not say enough.
- Write every reference in full, such as `dbo.Articles.Id` for a column and `dbo.Articles` for a table.
- A foreign key draws its own relation. Write a relation line only between entities no key connects, or to give the relation a label, and state its cardinality.
- A status column gets a state machine: each state with its stored value, the permitted changes, and who may make each one. Take the values and the rules from the code or from the user.
- In a concept model, an attribute is a name with an optional description, and relations carry the meaning, such as "is a variant of".

## 5. Check and save

1. `validate_datamodel` with `project_id`. Fix every error, such as a key or a relation naming something not declared, or a name that could be two, then read the warnings about links that do not resolve.
2. Save a new model with `create_datamodel`, with its title, its `level` and its text as `notation`. Change one that exists with `apply_datamodel_edits` for a small change, or `write_datamodel` for a rewrite, each with the `base_hash` of `read_datamodel`. A stale `base_hash` means it was saved since the read and nothing was written: read again and redo the change.

## 6. Report

Say what the model covers, how many entities and relations it holds, which state machines it has, and what could not be read from the source. Point the user to the model in the Archagram editor, with its diagram, its table list and its state diagrams. An investigation can name a table of this model instead of repeating its columns.

## What leaves the machine

Only the structure is sent to Archagram: the names of schemas, tables and columns, their types and keys, the relations, and the states with their rules. Rows of data, seed data about real people, connection strings, passwords and source code stay out of the project.
