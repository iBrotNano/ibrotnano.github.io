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

You act as a business analyst and requirements engineer.

Your role ends with the business requirements.

You are not allowed to read, inspect, modify, or write anything in the local workspace or repository except to write the final requirement document. Do not list files, read source files, open directories, or change any file on disk. The only allowed data source is the GitHub issue itself, fetched via the GitHub CLI (`gh`) in the command line.

Use `gh issue view`, `gh issue list`, or `gh api` to retrieve the issue details. Do not use any other shell commands for repository inspection and do not access the codebase. If you need clarification, ask the user only about the issue and the requirement details; never infer implementation details from local files.

Analyse the GitHub issue and ask me anything unclear. Try to figure out what really is needed as functionality. Be critical about the change's requirements. Focus on understanding the requirements, complete any missing details and clarifying any ambiguities.

The following are prohibited:

- Architectural decisions
- Technical solutions
- Data models
- APIs
- Frameworks
- Databases
- Programming languages
- Implementation details

If a technical decision appears necessary, simply note: “Technical implementation deliberately left open.”

Describe requirements exclusively in the language of a product manager, business unit or end user.

Focus on:

- Objectives
- Motivation
- User value
- Business rules
- Constraints
- Acceptance criteria

Not on the implementation.

Every statement must describe the ‘what’. If the statement describes the ‘how’, it must be discarded and reworded.

Examples:

BAD: “The system stores user preferences.”
BETTER: “Users should retain their settings permanently.”

BAD: “A notification API is provided.”
BETTER: “Users are informed of relevant changes.”

BAD: “The system uses OAuth.”
BETTER: “Users can log in with existing accounts.”

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

Check if the requirements can be separated into multiple smaller, manageable issues. If so, recommend smaller issues, formulate them clearly, show me the content one by one and ask to create them on GitHub. Rewrite the initial issues as an epic and create a list of the smaller issues in the order they should be addressed.

Use the following template for creating issues on GitHub:

# Issue Title

## User Story

Only if applicable, describe the user story context or scenario. Write only a story if a real user story exists. Not one a developer does or needs something.

## Goal

What the change aims to achieve or the problem it intends to solve.

## Scope

What is included in the change.

## Out of Scope

What is explicitly excluded from the change.

## Acceptance Criteria

- [ ] Given <context>, when <action>, then <expected outcome>.

## Dependencies

Dependencies on other issues, systems, or components required for this change.

## Notes

Additional notes or context relevant to the change.

If only one issue remains and everything is clear and understood create a final requirement document under docs/issues/#<issue-id>/requirements-<issue-id>.md. The format must be markdown. The documents structure should be a hybrid of PRD + User Stories + ISO 29148.

1. Business Goal
2. Stakeholders
3. Scope / Out of Scope
4. Functional Requirements
5. Non Functional Requirements
6. User Stories
7. Acceptance Criteria
8. Constraints
9. Risks
10. Traceability Matrix
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

The final result is pretty good, but it still requires user intervention to ensure that all details are accurate and complete. Anyway it is a great starting point.

Here is an example requirement document:

```markdown
# Requirements - Issue #36: Core Beat 'em Up Movement and Combat

| Field      | Value                                                                    |
| ---------- | ------------------------------------------------------------------------ |
| Issue      | #36                                                                      |
| Epic       | #35 - Cyberpunk beat 'em up prototype with endless run-based progression |
| Status     | Final draft for review                                                   |
| Date       | 2026-11-26                                                               |
| Owner      | Marcel Melzig                                                            |
| Scope type | Foundation / playable combat slice                                       |

## 1. Business Goal

Deliver the testable foundation of the game: a player can move through a scrolling street in the classic beat 'em up manner and fight hand-to-hand with a moveset that already feels responsive, readable and impactful. This issue proves the core combat feel before enemy variety, progression, loot, level generation and final presentation are added.

Success means a player can walk the street, attack, block, dodge, grab, throw, deal and receive damage, and clearly feel that combat responds immediately and reads well, using placeholder visuals.

## 2. Stakeholders

| Stakeholder                          | Interest                                                                                                         |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| Project owner (Marcel Melzig)        | Defines scope, accepts the result, decides whether development continues.                                        |
| Player / playtester                  | Plays the foundation build and judges whether movement and combat feel good.                                     |
| Game design and balancing            | Tunes timings, damage, guard break and feedback; owns parameter values.                                          |
| Downstream issue owners (#37 to #45) | Build enemy variety, bosses, progression, loot, level generation, UI and presentation on top of these mechanics. |
| Art and audio (#44)                  | Replaces placeholder visuals and sounds later.                                                                   |

## 3. Scope / Out of Scope

### In Scope

- A scrolling test street with a defined beginning and end and placeholder visuals.
- Lane-based movement left and right plus into and out of the screen within a limited walkable depth band, with a following camera.
- Facing toward movement or the nearest opponent.
- A three-hit basic attack chain with a stronger knockdown finisher.
- A separate heavy attack with no resource cost and longer recovery.
- Front-facing block with reduced damage, chip damage from heavy attacks and guard break.
- Directional dodge with brief invulnerability and no cooldown.
- Grab and throw of any adjacent opponent.
- Hit reactions: flinch, knockback and knockdown for player and opponents.
- Health, damage and defeat; player defeat leads to a quick restart of the test street.
- Impact feedback through hit-stop and a small screen shake.
- At least one placeholder opponent that can take damage, be grabbed and thrown, and damage the player.
- Keyboard and gamepad support with default bindings.

### Out of Scope

- Jumping, aerial combat, platforms or any vertical platforming.
- Enemy variety and full enemy behavior (owned by #37).
- Boss encounters (owned by #39).
- XP, levels, stats and enemy scaling (owned by #40).
- Loot, implants and inventory (owned by #41).
- Procedural level generation (owned by #38).
- Run lifecycle, death reset, high score and persistence (owned by #42).
- Main menu, HUD, pause, settings, control remapping and run summary (owned by #43).
- Final art, animation, audio and music (owned by #44).
- Balancing and full playtest validation (owned by #45).
- Multiplayer, narrative, localization, saving and online features.

## 4. Functional Requirements

| ID       | Requirement                                                                                                                                                                                                                                                          |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-36-01 | The player shall move left and right and into and out of the screen within a limited walkable depth band. The player shall not leave the walkable band.                                                                                                              |
| FR-36-02 | The camera shall follow the player along the scrolling street. The street shall have a defined beginning and end that constrain movement and camera travel.                                                                                                          |
| FR-36-03 | The player shall face the direction of movement or the nearest opponent. Attacks shall affect only the side the player faces.                                                                                                                                        |
| FR-36-04 | The player shall execute a three-hit basic attack chain on repeated attack input. The third hit shall be stronger than the first two and shall be able to knock an opponent down.                                                                                    |
| FR-36-05 | The player shall have a separate heavy attack. It shall not consume a resource and shall have a longer recovery than any basic chain hit.                                                                                                                            |
| FR-36-06 | The player shall block attacks from the front. Blocking shall reduce most incoming damage. Heavy attacks shall cause chip damage through the block, and sustained blocking shall be guard-breakable.                                                                 |
| FR-36-07 | The player shall perform a directional dodge with brief invulnerability. The dodge shall have no cooldown.                                                                                                                                                           |
| FR-36-08 | The player shall grab any adjacent opponent. A grabbed opponent shall be restrained until thrown or until the player is interrupted. The player shall throw the grabbed opponent, knocking it down. Grabbed opponents shall not free themselves.                     |
| FR-36-09 | Player and opponents shall react to hits with flinch, knockback and, where the attack warrants it, knockdown. A knocked-down character shall recover after a short time.                                                                                             |
| FR-36-10 | Player and opponents shall have health and shall deal damage. Reaching zero health shall defeat the character. On player defeat, a clear defeated state shall be shown and the player shall restart the test street quickly with restored health and reset position. |
| FR-36-11 | Every successful hit shall produce hit-stop and a small screen shake. Feedback shall distinguish normal hits, heavy hits and knockdowns.                                                                                                                             |
| FR-36-12 | The test street shall contain at least one placeholder opponent that can take damage, be grabbed and thrown, and damage the player. Minimal behavior is sufficient in this issue.                                                                                    |
| FR-36-13 | All combat actions shall be performable with keyboard and gamepad using default bindings. Remapping is not part of this issue.                                                                                                                                       |
| FR-36-14 | The player and opponents shall remain within the street boundaries and shall not leave the playable area.                                                                                                                                                            |
| FR-36-15 | The test street shall allow repeated combat testing without reloading or leaving the build, including recovery after player defeat.                                                                                                                                  |

## 5. Non-Functional Requirements

| ID        | Requirement                                                                                                                                                                                       |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NFR-36-01 | Combat shall respond immediately to input, with no perceptible delay between command and action.                                                                                                  |
| NFR-36-02 | With placeholder visuals, all combat states shall remain distinguishable by pose and animation alone: blocking, dodging, stunned, knocked down and defeated. Color alone shall not carry meaning. |
| NFR-36-03 | A ten-minute combat session with repeated movement, attacks, blocks, dodges, grabs and hits shall run without crashes or blocking defects.                                                        |
| NFR-36-04 | Combat shall run at a smooth frame rate on a typical Windows desktop without stutter during multi-hit exchanges. The exact numeric target is left to validation.                                  |
| NFR-36-05 | The foundation build shall run on Windows desktop and support both keyboard and gamepad.                                                                                                          |
| NFR-36-06 | Any visible placeholder text shall be in English. Final audio and music are not required in this issue.                                                                                           |
| NFR-36-07 | The build shall be fully offline and shall collect no personal data.                                                                                                                              |
| NFR-36-08 | Combat timings, damage, guard break and dodge behavior shall remain adjustable without redesign, so that #45 can tune them.                                                                       |

## 6. User Stories

### US-36-01 - Move through the street

As a player, I want to walk left, right, toward and away from the camera within a bounded street so that I can approach and evade opponents like in a classic arcade beat 'em up.

### US-36-02 - Fight hand-to-hand

As a player, I want a fast three-hit combo, a heavy blow, a block and a dodge so that I can express skill and feel powerful in close combat.

### US-36-03 - Grab and throw

As a player, I want to grab an opponent and throw them so that I can control the crowd and create space.

### US-36-04 - Impact and defeat

As a player, I want hits to feel impactful and defeat to be clear, with a quick restart, so that I can keep testing and improving.

## 7. Acceptance Criteria

- [ ] AC-36-01: Given the player is on the test street, when moving left, right, into and out of the screen, then the player stays within the walkable band, the camera follows along the street, and movement stops at the street ends.
- [ ] AC-36-02: Given the player attacks repeatedly, when the chain executes, then three hits occur in sequence and the third hit is stronger and can knock an opponent down.
- [ ] AC-36-03: Given an opponent is adjacent, when the player grabs, then the opponent is restrained until thrown or until the player is interrupted; when thrown, the opponent is knocked down.
- [ ] AC-36-04: Given the player blocks a frontal attack, when it lands, then damage is reduced; heavy attacks cause chip damage; sustained blocking can be guard-broken.
- [ ] AC-36-05: Given the player dodges, when an attack would connect during the dodge, then no damage is taken; the dodge can be repeated without waiting for a cooldown.
- [ ] AC-36-06: Given a hit lands, when impact occurs, then the target flinches or is knocked back; heavy hits and the chain finisher can knock the target down, and knocked-down characters recover after a short time.
- [ ] AC-36-07: Given player health reaches zero, when defeat occurs, then a defeated state is shown and the player can quickly restart the test street with restored health and reset position.
- [ ] AC-36-08: Given the player uses the heavy attack, when it executes, then it costs no resource and has a longer recovery than any basic chain hit.
- [ ] AC-36-09: Given the player moves or an opponent is nearby, when the player acts, then the player faces the relevant direction and attacks affect only that side.
- [ ] AC-36-10: Given a hit lands, when the hit connects, then hit-stop and a small screen shake occur, and normal hits, heavy hits and knockdowns are distinguishable.
- [ ] AC-36-11: Given the test street, when the player engages, then at least one placeholder opponent can take damage, be grabbed and thrown, and damage the player.
- [ ] AC-36-12: Given keyboard or gamepad, when the player plays, then all combat actions are performable with default bindings.
- [ ] AC-36-13: Given a ten-minute combat session, when played repeatedly, then no crash or blocking defect occurs and controls remain responsive.
- [ ] AC-36-14: Given placeholder visuals, when a character blocks, dodges, is stunned, is knocked down or is defeated, then the state is recognizable from pose or animation alone.

## 8. Constraints

| ID      | Constraint                                                                                 |
| ------- | ------------------------------------------------------------------------------------------ |
| C-36-01 | No jumping, aerial attacks or platforming are part of this issue.                          |
| C-36-02 | Single player, offline, Windows desktop; keyboard and gamepad.                             |
| C-36-03 | Placeholder visuals only; no final art, animation, audio or music.                         |
| C-36-04 | Exactly one scrolling test street is sufficient; no procedural generation.                 |
| C-36-05 | Only minimal placeholder opponent behavior is allowed; full enemy behavior belongs to #37. |
| C-36-06 | No progression, loot, boss, UI shell or persistence in this issue.                         |
| C-36-07 | Combat values are provisional and shall be tuned later in #45.                             |
| C-36-08 | Technical implementation deliberately left open.                                           |

## 9. Risks

| Risk                                                                        | Impact                                                      | Mitigation                                                                                      |
| --------------------------------------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Combat feel cannot be judged with placeholder visuals.                      | The foundation may look technically correct but feel wrong. | Conduct an early playtest with placeholders; treat feel as the primary acceptance signal.       |
| A dodge without cooldown may allow permanent evasion.                       | Combat could become trivial or degenerate.                  | Keep dodge timing as a tuning parameter; validate in #45; revisit the rule if it breaks combat. |
| Guard break and chip damage may frustrate players if mistuned.              | Blocking may feel unusable or too strong.                   | Treat values as provisional; tune in #45; verify in playtests.                                  |
| Grabbing all enemies, including heavy robots, may look or feel implausible. | Visual and gameplay dissonance.                             | Revisit enemy-specific grab rules in #37 while keeping the current foundation requirement.      |
| The placeholder opponent may expand into full enemy AI.                     | Scope creep delays the foundation.                          | Keep behavior minimal and hand enemy roster work to #37.                                        |
| Street and lane boundaries may have edge cases.                             | Player or opponents can leave the playable area.            | Explicit boundary acceptance criterion; test at both street ends and both depth edges.          |
| Responsiveness is subjective.                                               | Acceptance may be disputed.                                 | Validate through playtest in this issue and formalize criteria in #45.                          |
| Scope creep into HUD, settings or progression.                              | Foundation delivery slips.                                  | Out-of-scope list is binding; those items belong to #40, #42 and #43.                           |
| Numeric tuning may conflict with later scaling.                             | Rework in progression.                                      | Keep all values adjustable and documented as provisional.                                       |

## 10. Traceability Matrix

| Source                                                 | Requirement(s)               | Acceptance Criterion |
| ------------------------------------------------------ | ---------------------------- | -------------------- |
| Issue #36 AC 1 - movement and camera                   | FR-36-01, FR-36-02, FR-36-14 | AC-36-01             |
| Issue #36 AC 2 - attack chain                          | FR-36-04                     | AC-36-02             |
| Issue #36 AC 3 - grab and throw                        | FR-36-08                     | AC-36-03             |
| Issue #36 AC 4 - block                                 | FR-36-06                     | AC-36-04             |
| Issue #36 AC 5 - dodge                                 | FR-36-07                     | AC-36-05             |
| Issue #36 AC 6 - hit reactions                         | FR-36-09                     | AC-36-06             |
| Issue #36 AC 7 - health and defeat                     | FR-36-10                     | AC-36-07             |
| Interview - heavy attack                               | FR-36-05                     | AC-36-08             |
| Interview - facing                                     | FR-36-03                     | AC-36-09             |
| Interview - hit feedback                               | FR-36-11                     | AC-36-10             |
| Issue #36 scope - test opponent                        | FR-36-12                     | AC-36-11             |
| Issue #36 scope - controls                             | FR-36-13                     | AC-36-12             |
| NFR-36-01 and NFR-36-03 - responsiveness and stability | NFR-36-01, NFR-36-03         | AC-36-13             |
| NFR-36-02 - state readability                          | NFR-36-02                    | AC-36-14             |
| Issue #36 scope - repeat testing                       | FR-36-15                     | AC-36-07             |
| Constraints C-36-01 to C-36-08                         | Scope boundaries             | Verified by review   |
```