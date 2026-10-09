---
name: update-after-change
description: Update an Archagram diagram after a code change by reading the git diff and applying small targeted edits to the project's notation. Use when the user asks to sync, refresh or update an Archagram diagram after changing code, or runs /archagram:sync.
user-invocable: false
---

# Update the diagram after a change

Keep an Archagram project in step with the code, one targeted edit at a time. The modeling skill covers the notation conventions.

Tools used: `list_projects`, `read_project_context`, `read_notation`, `apply_notation_edits`, and `get_notation_reference` when the change needs a construct the project doesn't use yet.

## 1. Find the change

- With a shell, as in Claude Code, run `git status` and `git diff HEAD` for staged and unstaged changes, and read the new files `git status` lists as untracked, since the diff leaves them out. When the user names a range, use it instead, for example `git diff main...HEAD`, or `git show` for the last commit.
- Without a shell, ask the user to paste the diff or describe the change.

## 2. Find the project

Use the project the user names. Otherwise call `list_projects` and pick the one that matches the repository; ask when more than one fits or none does.

## 3. Read before editing

1. `read_project_context` for the team's conventions.
2. `read_notation` for the current text and its `base_hash`:
   - When the change removes a component or changes an id, read the full text, without `sections`. Views, sequences, targets, processes and ADRs refer to components by id, and a sectioned read leaves them out.
   - Otherwise `sections: ["structure", "targets"]` is enough: the structure to edit, and the targets to see whether the change completes a work package. Its `base_hash` is the same one a full read returns.

## 4. Decide what is architectural

Model a change when it alters what runs or how the parts connect:

- a service, app, function, worker or job added, removed, renamed or split
- a data store, cache, queue, topic or bucket added or dropped
- a call between services added or dropped, or its protocol changed
- an external integration added or removed
- a component moved to another deployment or ownership boundary

Skip refactors inside one component, tests, styling, dependency bumps and configuration values. When nothing architectural changed, say so and stop without writing.

## 5. Edit

Build the smallest set of find/replace pairs, each `old` copied exactly from the text you read:

- Add a component or a relationship next to related lines, anchored on a line that occurs once.
- Rename by changing the label and keep the id. If the user asks for a new id, change it on every line of the full text that names it.
- Remove a component together with every line of the full text that names its id, in the same call: its relationships and its references in views, sequences, targets, processes and ADRs. A reference left behind can bring the component back, with a warning that its id "is never declared", or make the server refuse the call. After a removal, treat that warning as a leftover reference and remove it with a follow-up edit on the returned `base_hash`.
- Update a `description` or a `data` entry that the change made wrong.

Send all pairs in one `apply_notation_edits` call with the `base_hash`. Check the warnings it returns. If it reports a stale `base_hash`, read again and rebuild the edits once on the new text. If the change clearly completes a work package in one of the project's targets, ask whether to mark it done.

## 6. Report

List each architectural change you found, the edit it produced and what you skipped. Keep the new `base_hash` for further edits in this conversation.
