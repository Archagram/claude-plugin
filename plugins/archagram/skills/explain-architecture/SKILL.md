---
name: explain-architecture
description: Explain an Archagram project's architecture to a newcomer from its structure, documents, decisions and roadmaps, without changing anything. Use when the user asks what a system in Archagram does, how it fits together or how a request flows through it, wants an onboarding overview of an Archagram project, or runs /archagram:explain.
user-invocable: false
---

# Explain a project's architecture

Give someone new to the system a clear picture they can check against the diagram. This only reads: no write tool is needed.

Tools used: `list_projects`, `read_project_context`, `get_project_stats`, `read_notation`, `read_links`, `read_file`.

## 1. Read

1. `list_projects` to find the project; ask when the name matches more than one.
2. `read_project_context` for the team's vocabulary and conventions.
3. `get_project_stats` for the size, the views and the targets.
4. `read_notation` with `sections: ["structure"]`, then the sections the question needs: `sequences` for flows, `targets` for plans and roadmaps, `adrs` for decisions. Read the full text only for a small project or a question about the whole system.
5. `read_links` with an `object_id` for the documents attached to a central component, and `read_file` for the documents that answer the question.

## 2. Explain

Write for a reader who knows software but not this system:

1. **Purpose**: what the system does and for whom, in two or three sentences.
2. **Main parts**: the boundaries and the most important components, one line each on what they are responsible for. Use the diagram's labels so the reader can find them.
3. **How it works**: walk through one or two typical requests along the relationships, step by step. Prefer a sequence the project already defines.
4. **Data and integrations**: where data is stored and which outside systems it depends on.
5. **Decisions and direction**: the ADRs that shaped it, and any target with its roadmap.
6. **Where to look next**: the views, documents and components worth opening in the editor.

Separate what the project states from what you infer, and say when the diagram is silent on something. Match the depth to the question: a focused question gets a focused answer, not the whole tour.
