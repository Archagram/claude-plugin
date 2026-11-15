---
name: journey-screens
description: Prepare the screens of an Archagram journey, what a person sees at its steps, drawn inside a browser, a phone, a kiosk or another device and walked screen to screen. Use when the user asks for screens, a click-through or a mock-up of a flow in Archagram, wants to show a change to people who are not technical before it is built, or runs /archagram:screens.
user-invocable: false
---

# Prepare the screens of a journey

The screens of a journey show what a person sees at its steps, drawn inside a browser, a phone, a tablet, a kiosk, a card terminal or a handheld scanner, and walked screen to screen in the editor. They let people who are not technical picture a change before it is built. A screen is a small model, never a layout: its sections, its actions and the data it shows. The journey owns the order and the branches.

Tools used: `list_projects`, `get_project_stats`, `read_journey_screens`, `get_notation_reference`, `list_files`, `write_file`, `validate_journey_screens`, `write_journey_screens`, and `get_guide` where the connector offers it.

## 1. Read the journey

1. `get_project_stats` lists the project's journeys. Ask which one when the request does not say. When the outcome has no journey yet, draft it first with the draft-journey skill.
2. `read_journey_screens` with the journey's id returns its steps and handoffs (the ids a screen and a press name), the screens it already has and a `base_hash`.
3. `get_notation_reference` with `topic: "screens"` has the grammar and a complete example. Follow it rather than memory.
4. Where the connector offers `get_guide`, read the `journey-screens` guide as well. It is the server's current word on this job, and it wins where this skill differs.

## 2. Make the journey ready

A screen sits on a step: a task performed by the component the device stands for, such as the web shop or the store's app. Check that the journey has:

- a step for every moment a person looks at something or does something;
- between two screens, the steps the systems take (an API, a database), each with its performer and the payload on its handoff, so the technical reader sees what happens behind a press;
- a handoff leaving a screen's step for every press that moves on.

Add what is missing to the journey itself first, as the draft-journey and modeling skills describe, then read the screens again for the new step ids.

## 3. Find the screens in the code

With file access, as in Claude Code, and when the flow is already built:

1. **Find the pages**: the routes, page components, templates and forms the journey's steps pass through.
2. **Take the words from the product**: the headings, field labels and button texts the code shows a person, in the language the product uses.
3. **Take the data from what the page loads**: the response a page reads gives the shape of what it shows.
4. **Note each state**: a page after a save, with an error or with an empty list is a screen of its own when the story needs it.

For a change that is not built yet, or in a chat without file access, work from what the user describes and ask for the moments a person sees.

## 4. Choose the devices

One device per client component, at the top level of the file, naming that component. Pick the device the person really uses. A browser takes a host, and a navigation written once on the device is drawn on every screen.

## 5. Write the screens

- One screen per step a person sees, placed on that step. A different state of the same page is another screen on its own step.
- Name screens, sections and actions with the words the product uses, as the person would read them.
- Keep a screen to what matters for the story: usually three to six parts, and one main action that moves the journey on.
- A section's type is its look: details for one record, a table or a list for rows, a form for input, a message for a result. Parts are drawn in the order written.
- An action that moves the journey names the step it leads to. An action that leads nowhere is drawn and does nothing, which is right for a control that is only there to be seen.
- To show data, write an example body first with `write_file` under `payloads/`, then name it on the section with the few fields that matter. An object reads as labeled fields, a list of objects as rows. Check `list_files` and reuse a payload the journey's handoffs already carry.
- A note or a warning puts a highlighted line beside the device for the audience. Use them sparingly.

## 6. Check and write

1. `validate_journey_screens` with `project_id`. Fix every error and read the warnings: a screen or a press that names no step, a press no handoff leads to, a step performed by another component than the device's, a payload file that does not exist yet.
2. `write_journey_screens` with the full text and the `base_hash` of the read. Nothing is written when the screens changed since; read again and redo the change.

## 7. Report

Say which devices and screens there are, which presses move the journey and what is still a warning. Point the user to the journey's Screens page in the Archagram editor: a person finishes the screens there by clicking, walks them, and shares them by a link. The questions asked of an audience and the feedback sessions are set up in the editor, by a person.

## What leaves the machine

Only the screens text and the payloads you write are sent to Archagram. Payloads hold made-up, believable values, never real customer data, credentials or tokens found in code, fixtures or logs. Keep source code out of the project.
