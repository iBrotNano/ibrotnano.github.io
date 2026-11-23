---
title: Using OpenCode for Requirement Engineering
date: 2026-11-23 23:40:00 +0200
categories: [software, ai, agentic coding, requirement engineering, development, opencode]
---

Sure Coding Agents are incredible tools for code generation. I ask myself if I could use it to improve the requirement engineering process. What if it could help me gather, analyze, and refine requirements more efficiently, ensuring that nothing is overlooked and that the final specifications are precise and comprehensive?

The process should start with some basic information like the one defined in an issue in GitHub. The issue only need to be an high level idea. Briefly summarized, but containing all information known at that point. From there, OpenCode should act as an interviewer to point out gaps by riddle me with questions, challenging assumptions, and prompting deeper thinking to uncover hidden requirements and potential issues. I currently experimenting with a configuration that denies all access to the codebase, forcing the agent to rely solely on the information provided in the issue. I hope that the answer be more focus on the requirements themselves rather than the implementation details. After the interview is complete, OpenCode should generate a markdown file with the refined and complete set of requirements.

Here is the agent's definition:

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

You are not allowed to read, inspect, modify, or write anything in the local workspace or repository. Do not list files, read source files, open directories, or change any file on disk. The only allowed data source is the GitHub issue itself, fetched via the GitHub CLI (`gh`) in the command line.

Use `gh issue view`, `gh issue list`, or `gh api` to retrieve the issue details. Do not use any other shell commands for repository inspection and do not access the codebase. If you need clarification, ask the user only about the issue and the requirement details; never infer implementation details from local files.

Analyse the GitHub issue and ask me anything unclear. Try to figure out what really is needed as functionality. Be critical about the change's requirements. Focus on understanding the requirements, complete any missing details and clarifying any ambiguities.

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

If everything is clear and understood create a final requirement document under /docs/requirements/#issue-id.md. The format must be markdown.
```

The access to the repository is denied by the configured `permissions`. The agent is advised to use GitHub.Cli to get the issue. It must be installed on the local machine and a user must be authenticated with sufficient permissions to access the repository.

Select the agent by `/agents` and start it with `Work on issue <issue-id>`.

I made a test run with a very minimalistic issue:

```markdown
### :spiral_notepad: Description

A user can ask the app about products and for alternatives in natural language. The user can also talk with the chatbot about recipes and the ingredients in them. The user can ask for alternatives for all ingredients to optimize the recipes.

### :inbox_tray: Input

User questions and input in the command line.

### :outbox_tray: Output

Information about the ingredients and recipes.

### :warning: Side effects

LLM usage costs money.

### Acceptance Tests

- [ ] I can ask questions about nutrition and ask for alternatives.
- [ ] I can ask questions about recipes and ask for alternatives to optimize the recipes.
```

The agent performs an interview and finally generates a very detailed requirement document based on the gathered information.

```markdown
# Requirements: ChatBot for nutritional information and recipes

- Issue: [#53](https://github.com/iBrotNano/python_workshop_project/issues/53)
- Milestone: 0.2.0
- Status: Draft - requirements clarified, ready for implementation
- Labels: enhancement
- Branch: `feature` branch created (see issue checklist)

## 1. Summary

Add a command-line chatbot to the application that answers natural-language
questions about products and their nutritional values, proposes alternative
products for a given nutritional goal, and discusses recipes and their
ingredients - including proposing ingredient substitutions to optimize a recipe.
The chat is grounded in the application's own SQLite data (products, ingredients,
recipes). Confirmed recipe optimizations are written back to the database.

## 2. Goals

1. Let a user ask questions about nutrients/products in natural language and get
   answers grounded in the application's data.
2. Let a user state a nutritional goal (e.g. lower sugar, fewer calories) and get
   alternative products/ingredients that exist in the application's dataset.
3. Let a user discuss a recipe, see its ingredients, and ask for optimizations
   (substitutions) - with an explicit confirmation step before anything is saved.
4. Fail gracefully and cheaply when the configured local LLM is missing,
   incompatible, or cannot be loaded.

### Problem being solved

Today nutrition and recipe data can only be inspected through fixed/explicit
commands. Exploring it ("which of these has less sugar?", "what can I swap the
sugar with?") requires manual comparison. Users want to ask directly and get
reasoned answers plus concrete, dataset-backed alternatives.

### Success

The two acceptance tests of the issue are demonstrable end-to-end in a fresh demo
environment (see section 11). No measurable latency or cost target is defined in
this change; they are TBD (see section 13).

## 3. Users and current process

- Single local user operating the application from a terminal (no accounts,
  no multi-user, no web UI).
- Current process: user runs CLI commands, reads tabular/text output, and must
  compare values manually. No LLM is involved today.
- After the change: user starts a chat session and converses; the application
  retrieves matching data via semantic search, the LLM decides which tool to call
  and explains results.

## 4. Scope

### In scope

- Interactive chat mode in the CLI (REPL loop; context kept for the session;
  exit via a documented command/`Ctrl+C`).
...

### Out of scope (non-goals)

- Web or HTTP UI.
...

## 5. Functional requirements

### 5.1 Chat session

- FR-1: A CLI entry point starts an interactive chat loop. The user types a
  question per line; the assistant answers in the terminal.
...

### 5.2 Grounded nutrition Q&A (acceptance test 1, part 1)

- FR-6: On a natural-language question about a product/ingredient, the app
  performs semantic search over the local index and passes the best-matching
  records (name, per-100 g nutrition fields available in the DB, category) to
  the LLM as tool results.
...

### 5.3 Alternatives under a constraint (acceptance test 1, part 2)

- FR-9: The user states a goal in natural language (e.g. "lower sugar",
  "fewer calories", "more protein"). The LLM maps this to a structured
  constraint and calls the alternatives tool.
...

### 5.4 Recipe discussion and optimization (acceptance test 2)

- FR-13: The user can reference a recipe by name (or ask what a recipe contains);
  the app retrieves the recipe and its ingredients from the database.
...

### 5.5 Routing

- FR-19: All user input is handled by the LLM; capability selection happens via
  tool/function calling. No slash-command syntax is required from the user.
...

## 6. Data and retrieval

- Storage: existing SQLite database holding products, ingredients and recipes;
  the chatbot must reuse the existing data-access layer rather than introducing a
  parallel one.
...

## 7. LLM integration and configuration

- FR-21: The LLM is loaded locally with `llama_cpp_python` from the configured
  GGUF model file. The configuration contains the model metadata/path, chat
  format, and generation settings.
...

## 8. Error handling and resilience

- FR-26: A missing/incompatible model or model-loading/inference error produces
  a clear single-line error plus a configuration hint; the session exits
  gracefully without a stack trace. Optionally the user may retry the same
  question.
...

## 9. Cost, safety, privacy

- FR-30: Configurable max output tokens and a maximum conversation length
  (context window cap with oldest-turn trimming). Defaults documented.
...

## 10. Non-functional requirements

- Performance: interactive response expected; no numeric latency target defined
  (TBD). Index build time is a one-off; query time is bounded by top-K search.
...

## 11. Acceptance criteria

Derived from the issue's acceptance tests, made verifiable:

1. **Nutrition Q&A** - Given a product/ingredient known to exist in the database,
   when the user asks about its nutrition, then the answer cites the stored values
   and the CLI prints the structured record block. Given an item not in the
   database, then the assistant says it is not found and states no values.
...

Demo: fresh demo environment, run all four scenarios, plus verify the two
original acceptance test checkboxes.

## 12. Dependencies and risks

- Depends on the existing SQLite schema and data-access layer (reuse, no schema
  changes assumed; any needed schema change must be raised separately).
...

## 13. Open questions / TBD

- No measurable latency target and no per-session budget cap were defined; both
  are TBD.
...

## 14. Timeline

- Target milestone: 0.2.0. No fixed date or effort budget beyond the milestone.
- Definition of done: section 11 scenarios pass in a fresh demo environment and
  the issue's acceptance-test checkboxes can be ticked.
```

I had to modify it on some points but it did a quite impressive job overall. (I’ve shortened the document because it’s really long)