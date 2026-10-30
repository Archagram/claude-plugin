---
name: draft-journey
description: Draft an Archagram journey from code, the story of how the architecture delivers one outcome, with the example payload each handoff carries, the action each step performs and the field mappings between payloads. Use when the user asks to draft, map or document a journey, a request path or an end-to-end flow in Archagram from a codebase, or runs /archagram:journey.
user-invocable: false
---

# Draft a journey from code

A journey tells how the architecture delivers one outcome, as a short story of steps on the project's own components. Each activity is performed by one component, and each handoff between two steps says what travels: the example payload, what comes back, how fast or when, and what a converting step hands on. A reader follows the story in the editor's Story lens; a developer switches to the Technical lens and sees the payloads, the actions and the field mappings on the same picture. A journey drafted from the code gives the team something to build from. The modeling skill covers the notation conventions and the safe way to write.

Tools used: `list_projects`, `read_project_context`, `get_project_stats`, `read_notation`, `get_notation_reference`, `list_files`, `read_file`, `write_file`, `validate_notation`, `apply_notation_edits`, `apply_notation`.

## 1. Agree on the outcome

A journey has one outcome, such as "A customer places an order" or "A product change reaches the ERP". Take it from the user's request, or ask for it. Agree on what starts it, where it ends, and which unhappy outcomes are worth showing, such as a check that sends an order back. Ask which project it belongs to when more than one could hold it.

## 2. Read the architecture

1. `read_project_context` for the team's conventions.
2. `get_project_stats` for the targets and the journeys the project already has.
3. `read_notation` with `sections: ["structure", "targets", "processes"]`. The structure holds the components a step can name, the targets hold the components a future state adds, and the processes section holds the existing journeys. When one already tells this outcome, extend it instead of starting a second.
4. `get_notation_reference` for the journey grammar and its complete example. Follow it rather than memory or older projects.

Every step names a component that exists in the architecture, or that a target adds when the journey is told over that target. When the code shows a part the architecture lacks, ask whether to add it first, as the modeling skill describes, or to tell the journey over a target that adds it. Never invent a component for a journey.

## 3. Follow the code

With file access, as in Claude Code:

1. **Find the entry point**: the route, controller, message consumer, scheduled job or UI action where the outcome starts.
2. **Follow the calls** across deployable units. Each call, message, file or event that crosses from one component to another is a handoff, and the component that receives it performs the next step. Work inside one component stays inside its step.
3. **Name each step's action** from what the code does to what it receives, with the action words the reference lists, such as validate, map, enrich, route or store. When the action works against another component, such as validating against a rules database, that component is the step's `supported-by`.
4. **Note how each handoff travels**: request and reply, message, event, file or stream; what comes back on a request and reply; and a timeout or a schedule when the code states one.
5. **Find the bodies**: request and response classes, OpenAPI or AsyncAPI examples, JSON Schemas, XSD samples and test fixtures show what each handoff carries.
6. **Find the decisions**: a branch that changes the outcome becomes a decision, and its unhappy branch is marked as an error.

In a chat without file access, work from what the user shares, and ask for the entry point and the code that calls the next component.

## 4. Write the payloads with made-up values

Each handoff that carries something gets an example body under `payloads/`, written with `write_file` before the journey names it: `payloads/<name>.json` or `payloads/<name>.xml`, well formed and at most 256 KB. The server refuses a body that is not well formed and names its line and column.

- Use made-up values only: invented names, `ORD-1001`, `jane.doe@example.com`, round amounts and placeholder dates.
- Strip anything that looks like real customer data, credentials or tokens from what you find in code, fixtures or logs: names, email addresses, phone numbers, street addresses, account and card numbers, API keys, passwords, bearer tokens, connection strings and internal host names. Never copy a value from a log or a production fixture.
- Keep the shape real: the field names, the nesting and the types the code uses, with one or two entries in a list.
- Name each file after what it is, such as `payloads/order-placed.json`. Check `list_files` first and reuse a payload the project already has; one file serves every handoff that carries the same body.

## 5. Map the fields of a converting step

A handoff marked `(map)` or `(convert)` hands on something other than what it received: its `payload` line names what it takes and its `delivers` line what it hands on. When the code shows which field goes where, write a field mapping under `mappings/` with `write_file`, then name it on the handoff's `mapping` line:

```json
{
  "source": "payloads/order-placed.json",
  "target": "payloads/erp-order.xml",
  "rows": [
    { "from": "/order/id", "to": "/Order/@number" },
    { "from": "/order/lines[]/sku", "to": "/Order/Line/@sku", "rule": "upper case" }
  ]
}
```

A field path starts at the root of its payload: `/` separates fields, `[]` steps into the items of a list, and `@` names an XML attribute. Leave out a field the code does not place; the editor shows it as one without a source.

## 6. Write the journey

Draft the journey block in the shape of the reference's complete example:

- `start`, `task`, `xor`, `and` and `end` steps, joined by `->` handoffs labeled with what travels; a branch that leaves a decision is labeled with its outcome.
- `@component-id` on each task names the component that performs it, and `@component-id:supported-by` the one component its action works against.
- One action in parentheses on a task, such as `(validate)`, and on a handoff for work done on the way, such as `(map)`. After a handoff's label, one style, such as `(request-reply)` or `(event)`, and `(error)` on the unhappy branch of a decision.
- The lines right after a handoff, in this order: `payload`, `reply`, `timing`, `delivers` and `mapping`.
- A `note` right after a step narrates it in one sentence a non-technical reader follows.
- A journey about a future state goes inside its target's block, so its tasks may name the components the target adds.

Keep the story to about 5 to 15 activities, one step per handoff a reader cares about. Deeper detail goes into the payloads and the mappings, not into more steps. Leave out logging, retries, caching and helper calls unless they change the outcome. A journey that grows past 15 activities is usually two journeys.

## 7. Validate, apply and read back

1. Read the full text with `read_notation` and no `sections`, add the journey to it, and run `validate_notation` with `project_id`. Fix every error, and the warnings that point at a mistake: a task on a component that does not exist, a reply without `(request-reply)`, `delivers` or `mapping` without a payload or a `(map)` or `(convert)` action, and a handoff between components the architecture does not connect, which usually means a missing relationship or a skipped step.
2. Add the journey with one `apply_notation_edits` call and the `base_hash` from that read, anchored on a line that occurs once; a journey over a target is anchored on its target's header line. To rework an existing journey heavily, call `apply_notation` with the full text and that read's `base_hash`, as the modeling skill describes.
3. Read it back with `read_notation` and `sections: ["processes"]`, and check with `list_files` that every payload and mapping the journey names exists.

For a large journey in Claude Code or Cowork, the plugin's operator agent can run the validate and apply loop.

## 8. Report

Give the journey's name, its steps in order with the component performing each, the payloads and mappings written, the warnings left and why, and the assumptions you made. Point the user to the journey in the Archagram editor: the Story lens walks through it, and the Technical lens shows the payloads, the actions and the mappings.

## What leaves the machine

Only the notation, payloads and mappings you write are sent to Archagram, and the payloads hold made-up values. Describe the steps in your own words, and keep source code, secret values, credentials and personal data out of the project.
