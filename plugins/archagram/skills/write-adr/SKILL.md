---
name: write-adr
description: Write an architecture decision record (ADR) in an Archagram project from Archagram's ADR templates, numbered and linked to the components it concerns. Use when the user wants to record, document or write up an architecture decision in Archagram, or runs /archagram:adr.
user-invocable: false
---

# Write an architecture decision record

In Archagram an ADR has two parts: an `adr` entry in the project's notation (title, status, scope and the components it concerns) and one Markdown document under `docs/adr/` that holds the record itself. The modeling skill covers the safe way to write.

Tools used: `list_projects`, `read_project_context`, `get_project_stats`, `read_notation`, `list_artifact_templates`, `get_artifact_template`, `write_file`, `apply_notation_edits`, `get_notation_reference`.

## 1. Gather the decision

From the conversation, or by asking, collect: the decision, its context and the forces behind it, the options considered, the outcome, its consequences, and its status. The status is `proposed` unless the user says otherwise; Archagram also knows `accepted`, `rejected`, `deprecated` and `superseded`. Don't invent drivers or consequences the user hasn't given: write open points as questions in the record.

## 2. Read the project

1. `read_project_context` for the team's conventions.
2. For a decision about a future state, `get_project_stats`: its `targets` list gives the target's `id`, which the notation doesn't show.
3. `read_notation` with `sections: ["adrs", "structure"]` for the existing ADRs with their numbers and the ids of the components involved, plus `"targets"` when the decision concerns components a target adds. Keep the `base_hash`.

## 3. Write the document

1. Call `list_artifact_templates` with `kind: "adr"` and pick a template: the standard one for a choice between options, the lightweight one for a narrow, reversible decision, or the one for superseding an earlier ADR. `get_artifact_template` returns its body.
2. Number the ADR one above the highest existing number, padded to four digits: the third ADR is `ADR-0003`, stored at `docs/adr/0003.md`.
3. Fill the template's `{{...}}` placeholders (the ADR label, the title and today's date as YYYY-MM-DD) and write each section from what the user gave you.
4. Start the file with the front matter the Archagram editor reads, carrying the ADR's status, then a blank line and the record:

   ```markdown
   ---
   archagram-kind: adr
   status: proposed
   ---
   ```

5. Save it with `write_file` at that path.

## 4. Register it in the notation

Add an `adr` entry with one `apply_notation_edits` call, in the form `get_notation_reference` shows: an id in kebab-case from the title, the title, the same status as the front matter, `scope` set to `baseline` or to the target `id` from `get_project_stats`, the components it concerns, and its document, `@adr/0003.md` in the example above.

## 5. Report

Give the ADR number and title, its status and the components it is linked to; it appears on the editor's Decisions page. If the repository also keeps ADRs, for example in `docs/adr/` or `docs/decisions/`, offer to save a copy there.
