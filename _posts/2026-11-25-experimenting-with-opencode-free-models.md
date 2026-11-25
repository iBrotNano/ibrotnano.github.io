---
title: Experimenting with OpenCode Free Models
date: 2026-11-25 12:54:00 +0200
categories: [ai, agentic coding, coding, development, opencode, models]
---

In this post, I will share my experiences experimenting with OpenCode's free models. OpenCode offers a range of AI models that can be used for various coding and development tasks. The free tier provides an excellent opportunity to explore these models without any financial commitment.

I wrote an article lately about using OpenCode in the requirement engineering process [Using OpenCode for Requirement Engineering](/_posts/2026-11-23-using-opencode-for-requirement-engineering.md). Meanwhile the subagent has evolved a little. It has the advice to split the requirements into smaller, more manageable sub-issues and ask me to create them on GitHub. So the models really needs some kind of understanding to fullfil the tasks.

```markdown
---
description: Interviews the developer about the change's requirements
mode: all
permissions:
  - action: read
    resource: "*"
    effect: deny
  - action: edit
    resource: "*"
    effect: deny
---

You are not allowed to read, inspect, modify, or write anything in the local workspace or repository except to write the final requirement document. Do not list files, read source files, open directories, or change any file on disk. The only allowed data source is the GitHub issue itself, fetched via the GitHub CLI (`gh`) in the command line.

Use `gh issue view`, `gh issue list`, or `gh api` to retrieve the issue details. Do not use any other shell commands for repository inspection and do not access the codebase. If you need clarification, ask the user only about the issue and the requirement details; never infer implementation details from local files.

Analyse the GitHub issue and ask me anything unclear. Try to figure out what really is needed as functionality. Be critical about the change's requirements. Focus on understanding the requirements, complete any missing details and clarifying any ambiguities. Don't hallucinate or assume implementation details.

Try to clear the following questions:

- [ ] What is the main goal of this change?
- [ ] What problem is it trying to solve?
- [ ] What business goals does this change aim to achieve?
- [ ] How do we know if this change is successful?
- [ ] Are there any alternative solutions considered?
- [ ] Is the main goal clearly defined?
- [ ] What are the expected outcomes or benefits?
- [ ] What is the budget and timeline for this change?
- [ ] What does the change cost in terms of resources, time, and effort?
- [ ] Is the goal clearly distinct from others, or should the issue be broken down into smaller sub-issues?
- [ ] Are there any dependencies or related issues?
- [ ] Who is using the system?
- [ ] Who are the users affected by this change?
- [ ] What user roles are involved or affected by this change?
- [ ] How does the process work at the moment?
- [ ] What systems or tools are currently being used in the process?
- [ ] What are the current pain points or challenges in the process?
- [ ] What manual steps are involved in the current process?
- [ ] What should the system capabilities be after the change?
- [ ] What information or data is required for the change?
- [ ] What business rules or logic are involved in the change?
- [ ] Are there any exceptional cases or scenarios that need special handling?
- [ ] How fast does the system need to respond or perform after the change?
- [ ] How scalable does the system need to be after the change?
- [ ] Are there any accessibility requirements for the system after the change?
- [ ] What availability requirements are there for the system after the change?
- [ ] What security requirements are there for the system after the change?
- [ ] What data privacy or protection requirements are there for the system after the change?
- [ ] Are there any regulatory or compliance requirements?
- [ ] With what other systems or processes does this change need to integrate or interact? 
- [ ] What data needs to be exchanged or shared with other systems or processes?
- [ ] Who owns the interface or integration with other systems?
- [ ] Are there existing APIs or interfaces for integration with other systems?
- [ ] Are there any data format or protocol requirements for integration with other systems?
- [ ] Are there any error handling or retry mechanisms required for integration with other systems?
- [ ] Are there any monitoring or alerting requirements for integration with other systems?
- [ ] Are there any technical constraints or requirements?
- [ ] What technical standards or guidelines need to be followed?
- [ ] Are there any architectural constraints or requirements?

After the interview or information gathering finalize checking:

- [ ] Did we understand the issue correctly?
- [ ] Did we gather all necessary information?
- [ ] Are there any ambiguities?

Check if the requirements can be separated into multiple smaller, manageable issues. If so, recommend smaller issues, formulate them clearly, show me the content one by one and ask to create them on GitHub.

If only one issue remains and everything is clear and understood create a final requirement document under docs/issues/#<issue-id>/requirements-<issue-id>.md. The format must be markdown.
```

I used the subagent for my test directly as an agent with `/agents` and select them. The only prompt i gave them was `#1`, the issue number on GitHub.

The issue contains only a very brief description and lacks detailed requirements or context.

```plain
I want to create an beat em up sidescroller with Godot. I want to use GDScript to implement the game. The game should be located in a cyberpunk world in the near future. Humankind is living in a anarcho capitalism. Drugs, Weapons and crime is everywhere. Poeple using implants to modify themselves. The user plays a muscle man fighting his way through street gangs, robots. The game should be a mixture of old arcade sidescrolling beat em ups and diablo like leveling in 2D. The levels should be generated. Leveling the char should be endlessly. Enemies scale with the strength of the player.
```

With `/models` I selected the model I wanted to use for the task.

This was the whole setup for my test. I got some very interesting results. What you get depends heavy on the model you use. Not quite surprising.

Fist I used Tidepool Alpha Free. It asks me a few questions to understand the scope of the MVP first and then it offered me to create sub issues. It didn't create a requirement document. So it perfectly does what I expected it to do. It created the issues on GitHub and here is one of them as an example:

```Markdown
# Summary
Prototype the core beat em up loop in Godot 4 (GDScript 2.0) for a cyberpunk side-scroller. The player moves through procedurally generated levels, attacks enemies, defeats all enemies, then exits the level.

## Background
Based on issue #1 (Create a first prototype of a beat em up sidescroller). Cyberpunk near future, outlandish world, street gangs and robots. This issue covers the MVP core loop; leveling and enemy scaling are separate issues (B and C).

## Requirements
- Movement: run/strafe in the 2D plane. Jumping is out of scope for v1.
- Combat: at least one attack action. Enemies have HP and basic AI (patrol/chase/attack).
- Procedural levels: generate levels at runtime with a seedable generator. Win condition: kill all enemies, then exit the level.
- Player and enemies rendered as placeholder primitives (no imported art).
- Godot 4, GDScript 2.0.
- Controls: keyboard mapping, optional gamepad mapping.

## Acceptance criteria (prototype demo)
- [ ] Run the game, spawn into a generated level.
- [ ] Player can run/strafe; the screen scrolls with the player (sidescroll).
- [ ] Player attacks; enemies react and can damage the player.
- [ ] All enemies in the level are killable; once the last enemy dies the exit is available.
- [ ] Scripts are clean GDScript modules ready for reuse (movement, combat, level generator, enemy).

## Non-goals for this issue
- Leveling / skill points (issue C).
- Enemy scaling (issue B).
- Real art, sound, menus.
- Multiplayer.

## Notes
- Level generation: procedural corridors/rooms with enemy spawn points and an exit marker.
- This is the base for issues B and C.
```

It created the issue very briefly, clear. Just a summary, some requirements and acceptance criteria. It didn't go into too much detail. After I run the agent for the newly created issue, it produces the document, but also in this very briefly style. The interview questions does not show a lot of options. Overall, the model seems to have some kind of understanding, but not a deep one. Some more detail and a more structured document would be a better result as a requirement engineering result.

I had the same experience with other models as well. They tend to create issues in a concise manner, focusing on the summary, requirements or scope, and acceptance criteria without delving into extensive detail. The generated documents are useful for quickly setting up tasks, but they may require additional refinement for comprehensive documentation. Some models did't even follow the agents guidelines and asked me questions or create issues or documents. I proofed that they can do tool calling and they could.

Everything changes when the models get more complex. They ask more questions to me and the questions are more detailed and context-aware, often considering edge cases and dependencies that simpler models might overlook. This leads to more thorough and robust issue creation, as well as more comprehensive documentation and planning for the development process. DeepSeek 4.1 Flash Free did a very fine job creating the sub issues. It asked me also to rewrite the initial issue as an epic. It does not create a requirements document. This behavior exactly reflects what I would expect from the model. Nice. Here is the [epic](https://github.com/iBrotNano/battle_agent/issues/1).

Nemotron 3.5 Super VL Free or Beam Free also made really good jobs. So it is worth to experiment with different models.

There are also downsides. More complex models seems to make much more assumptions if the initial issue is vague. This is not good or bad in itself. The just had more knowledge memorized about game mechanics and tried to apply it. The briefly answers from simpler models might be more dependent on the explizit information provided by the interview. Both might be what you want dependent on what you expect as a resulting document. It must be proofed, refined, and possibly corrected by the user to ensure accuracy and completeness anyway.

For example:

```markdown
- Partial enemy scaling: enemy level follows ~70% of player level growth plus a depth ramp.
- Sustain: room-clear heal (15% max HP) and ammo refill.
```

I didn't tell the model to add those features as a requirement.