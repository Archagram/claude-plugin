---
name: plan-target
description: Plan a target architecture in Archagram, a future state written as changes to today's architecture, together with its delivery roadmap of phases, work packages, decisions and dependencies. Use when the user wants to plan a migration, a modernization or another future-state architecture in Archagram, or the work to get there.
---

# Plan a target architecture and its roadmap

In Archagram a target is a future state written as a delta over the current architecture, the baseline: the components and relationships it adds, changes and removes, with its own documents, pros and cons, views and sequences. Its plan is the roadmap: work packages grouped into phases and ordered by their dependencies. The modeling skill covers the notation conventions and the safe way to write.

Tools used: `list_projects`, `read_project_context`, `get_project_stats`, `read_notation`, `get_notation_reference`, `validate_notation`, `apply_notation_edits`, `apply_notation`.

## 1. Understand the goal

Ask for what the user hasn't said: the outcome the target has to reach, the constraints (deadline, budget, team, compliance), what must stay as it is, and which decisions are still open. Each alternative future is its own target, so two options become two targets that the editor's Compare screen shows side by side.

## 2. Read the baseline

Call `read_project_context`, then `get_project_stats` for existing targets, phases and plan progress, then `read_notation` with `sections: ["structure", "targets"]`. Call `get_notation_reference` for the form of targets, phases, plan packages and remarks.

## 3. Draft the target

1. **The delta**: the components and relationships the target adds, changes and removes. Refer to baseline components by their existing ids, and explain the delta in a document linked from the target.
2. **Phases**: a few delivery stages, each with a timeline label, a one-sentence focus a non-technical reader understands, what it includes and what it brings.
3. **Work packages**: one per deliverable piece of work, each with a title, the components it acts on, its phase, a size, a work type and the domain that owns it. Split a large package into sub-items.
4. **Dependencies**: an empty `after` list starts a path, predecessors order the work, and several predecessors join paths.
5. **Decisions**: an open choice becomes a decision package with named outcomes, and the packages that only happen for one outcome depend on that outcome.
6. **Remarks**: open questions and risks become remarks on a package or on the target, so they stay visible until someone resolves them.
7. **Pros and cons** of the target. For a decision already taken, offer an ADR scoped to the target (see the write-adr skill).

Show the user the draft in plain words (phases, main packages, open decisions) before writing it when the plan is large or rests on assumptions.

## 4. Write it

- Usually add the target block with one `apply_notation_edits` call, anchored on a line that occurs once and using the `base_hash` from your read. The server parses the result and refuses it with line-numbered errors when something is wrong, leaving the project untouched. Read the warnings it returns: an id that "is never declared" usually means a typo in a component reference.
- To rework an existing target heavily, read the full text, edit it, run `validate_notation` with `project_id`, and call `apply_notation` with the full text and that read's `base_hash`.
- For a large plan in Claude Code or Cowork, the plugin's operator agent can run the validate and apply loop.

## 5. Report

Summarize in a few lines: what the target changes, the phases with their main packages, the open decisions and remarks, and the longest chain of dependent packages. Point the user to the target's roadmap in the Archagram editor.
