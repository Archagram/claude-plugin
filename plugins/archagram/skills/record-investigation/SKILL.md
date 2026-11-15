---
name: record-investigation
description: Record in Archagram what a running system was observed to do, as an investigation, which pages, services, queues and databases call which, how often, over which protocol and with which results, with notes for the work that follows. Use when the user gives network captures, logs, traces or code to map in Archagram, asks which page calls which endpoint, or runs /archagram:investigate.
user-invocable: false
---

# Record an investigation

An investigation is the observed picture of a running system: what calls what, how often, over which protocol and with which results, as deep as the evidence goes. It may cover a frontend (pages and the endpoints they call), a backend (services, jobs, queues, databases), a network or an infrastructure. It is its own file beside the architecture and never part of the model. It holds what the evidence shows and nothing that was guessed.

Tools used: `list_projects`, `list_investigations`, `get_notation_reference`, `read_notation`, `list_datamodels`, `create_investigation`, `read_investigation`, `validate_investigation`, `apply_investigation_edits`, `write_investigation`, `write_file`, and `get_guide` where the connector offers it.

## 1. Gather the evidence

Work from what the user provides or the files at hand: network captures such as HAR files, logs, traces, query statistics, source code, configuration and documents. Ask where each came from, for which environment and when; those go in the investigation's header.

With file access, as in Claude Code, read the evidence on the machine and count there: group the calls by caller and callee, and add up how often each happened and with which results. Only the map goes to Archagram, never the capture.

## 2. Read what exists

1. `list_investigations` shows the investigations the project has. Extend one that covers the same system instead of starting a second.
2. `get_notation_reference` with `topic: "investigation"` has the grammar and an example. Follow it rather than memory.
3. `read_notation` gives the component ids an object can link to, and `list_datamodels` the data models a database can name.
4. Where the connector offers `get_guide`, read the `investigations` guide as well. It is the server's current word on this job, and it wins where this skill differs.

## 3. Write what the evidence shows

- An object is anything the evidence names: a page, an API and its endpoints, a service, a queue, a database and its tables, a host. Parts nest inside their parent: an API holds its endpoints, a page its sections and actions.
- Name an endpoint by its route, such as `GET /orders/{id}`, never by an address that holds a real id, an email address or a token.
- One line per pair of caller and callee, with the counts added together. Write the call from the part that makes it.
- The number of calls is how many times it happened. Leave it out when nothing counted it, as for a dependency read from code or documents; writing 1 would read as a measurement.
- Say the connection when it is not plain HTTP, such as a SQL statement, a Kafka publish or a gRPC method, then the results with their counts.
- Link an object to the architecture when the project holds that component, and a database to its data model when the project has one.
- Example request, response and message bodies are project files: write them with `write_file` under `payloads/` first, with made-up values, then name them.
- Totals, calls per visit, cache hit ratios and the findings are worked out by Archagram. Never write them.

## 4. Leave notes for the work that follows

A note records a question, a decision, a todo or a warning, on an object or on a call. Mark the notes you write as written by AI, and give a topic such as migration, security, data or performance where one fits.

## 5. Check and save

1. `validate_investigation` with `project_id`. Fix every error, then read the warnings about links and payload files that do not resolve.
2. Save a new investigation with `create_investigation`, with its title and its text as `notation`. Change one that exists with `apply_investigation_edits` for a small change, or `write_investigation` for a rewrite, each with the `base_hash` of `read_investigation`. A stale `base_hash` means it was saved since the read and nothing was written: read again and redo the change.

## 6. Report

Say what the investigation covers and from which evidence, what stands out (failed calls, chatty pages, calls the architecture does not show, data fetched and never used), and the questions left open as notes. Point the user to the investigation's map and matrix in the Archagram editor.

## What leaves the machine

A capture or a log holds cookies, authorization headers, tokens, session ids and personal data in addresses and bodies. None of that goes to Archagram. Send only the map: the objects by name, the calls between them with their counts, protocols and results, your notes, and example bodies written with made-up values. Keep source code, secret values, internal host names the user has not asked to record, and anything that identifies a person out of the project.
