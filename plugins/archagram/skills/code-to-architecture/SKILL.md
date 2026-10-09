---
name: code-to-architecture
description: Turn a code repository into an Archagram architecture diagram, built from its deployable units, data stores, integrations and the connections between them. Use when the user asks to diagram, map or visualize this repository or codebase in Archagram, or runs /archagram:diagram-repo.
user-invocable: false
---

# Diagram a repository in Archagram

Build a first, accurate architecture of the repository at the level a new team member needs: what runs, what it stores, what it talks to and who uses it. The modeling skill covers the notation conventions and the safe way to write.

Tools used: `list_projects`, `list_contexts`, `get_notation_reference`, `validate_notation`, `create_project`, `write_file`, and for an existing project `read_project_context`, `read_notation` and `apply_notation_edits`.

## 1. Survey the repository

Read structure and configuration before code. Useful sources, where they exist:

- README files and architecture notes
- Build manifests, usually one per deployable unit: `package.json`, `pom.xml`, `build.gradle`, `go.mod`, `pyproject.toml`, `*.csproj`, `Cargo.toml`
- Container and deployment files: Dockerfiles, Compose files, Kubernetes manifests, Helm charts, Terraform, CloudFormation, Bicep, serverless configuration
- CI and deploy pipelines, which name what gets shipped and where
- Configuration keys and environment variable names (not their values), which reveal databases, queues, caches and external APIs
- Database migrations, API route definitions, message topic names, and HTTP clients that call other services

With file access, as in Claude Code, list the directories first and open only the files that identify a deployable unit or a connection. In a chat without file access, work from what the user shares and ask for the files above.

## 2. Decide what becomes an element

- Each deployable unit becomes a component: a service, web or mobile app, function group, worker or scheduled job.
- Each data store, cache, queue, topic or bucket becomes a component named after what it holds.
- Each third-party service the code calls, such as payments, email or identity, becomes a component, grouped in a boundary for external systems.
- The people who use or operate the system become user components.
- Boundaries follow deployment or ownership: a cluster, a cloud account, a domain or a team.
- Relationships come from evidence: a client pointed at another service, a connection string, a producer or consumer of a topic. Label each with its protocol or purpose.
- Libraries, shared modules, tests and build tooling stay out of the top level. The modules of one service can go one level down, inside that service's block, when they matter to a reader.

Aim for about 5 to 15 components per level; a larger system gets boundaries and drill-in levels. Record where each component lives in its `data` block, for example `repo: services/orders`.

## 3. Check for an existing project

Call `list_projects`. If a project already covers this repository, ask whether to update that one instead. To update it, read it with `read_project_context` and `read_notation`, compare it with what you found, and apply only the differences with `apply_notation_edits`, keeping existing ids. Otherwise propose a project name based on the repository name and confirm it with the user.

## 4. Write and create

1. Call `get_notation_reference`, then draft the full notation: a title, boundaries, components with descriptions, and relationships.
2. Run `validate_notation`, then fix the errors and the warnings that point at a mistake, as the modeling skill describes.
3. If `list_contexts` shows guidance that fits, ask whether to select it.
4. Call `create_project` with the name, the notation and any chosen `contexts`.
5. Offer to write `docs/overview.md` with `write_file`: a short description of the system and how to read the diagram, linked from the component it describes best.

## 5. Report

Give the project name and id, the number of components, boundaries and relationships, the assumptions you made, and what you left out on purpose. Mention that the layout can be arranged in the Archagram editor and stays in place through later updates.

## What leaves the machine

Only the notation and documents you write are sent to Archagram. Describe the system in your own words, and keep source code, secret values, credentials and personal data out of the project.
