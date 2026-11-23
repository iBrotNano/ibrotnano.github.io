---
title: Using OpenCode As A Sidekick
date: 2026-11-13 11:01:00 +0200
categories: [software, windows, ai, agentic coding, coding, development, opencode]
---

I'm just figuring out how to best use OpenCode as a sidekick in my development workflow. I like the idea to combine multiple Agents into my workflow to hopefully increase the quality of my work (and save some tokens by not burning them by using one subscription for all tasks). OpenCode is free to use and also provide some free to use models. Those models sometimes have limitations or your data is used to improve their learning. So keep in mind that free to use is not the same as private. For my personal projects that does not matter. They are public on GitHub anyway.

What I like is the idea to combine agents and models. To be fully dependent on one provider of these tool make my stomach gurgle. It means you will have to pay the bill whatever it is. The free to use models from OpenCode might be fully acceptable for some tasks and learning and knowing alternatives for a workflow is a good idea. So I started to configure some Agents for two tasks: one for reviewing a code change and another to document it. Both tasks needs to analyze the codebase and show me the insights it gathers. Perfectly suited for assessing what the models are aware of.

Here is what the reviewer looks like:

```markdown
---
description: Reviews changes for correctness and regressions
mode: all
model: opencode/deepseek-v4.1-flash-free#high
permissions:
  - action: edit
    resource: "*"
    effect: deny
  - action: shell
    resource: "*"
    effect: deny
---

Review the current changes. List findings in severity order with file and line references.

Especially check the following topics:

- [ ] Does the new functionality work as intended?
- [ ] Where is the code implemented? Is this fine or does it violate the architecture principles?
- [ ] Are the tests really meaningful and cover all scenarios? Especially Edge cases?
- [ ] Are there any potential regressions introduced by the changes?
- [ ] Is the code readable and maintainable? Is it clean code and follows the projects coding standards?
- [ ] Are there any security concerns with the changes?
- [ ] Are there any performance implications with the changes?
- [ ] Are there any usability or user experience concerns with the changes?
- [ ] Are there any accessibility concerns with the changes?
- [ ] Are there any documentation updates required due to the changes?
```

It is just a markdown file stored in `C:\Users\<YourUserName>\.config\opencode\agents\reviewer.md`. No coding is necessary to set it up. `description` describes what the agent does. `mode` specifies if the agent can run as an own agent, be started as a subagent by an agent or (in this case) both. `model` specifies which model the agent uses and `permissions` define what the agent is allowed to do.

The last part is the prompt used by the agent to review the changes.

The document agent is defined in a similar way.

```markdown
---
description: Documents changes and updates existing documentation
mode: all
model: opencode/deepseek-v4.1-flash-free#high
permissions:
  - action: edit
    resource: "*"
    effect: deny
  - action: shell
    resource: "*"
    effect: deny
---

Review the current changes. Summarize findings and document them und the folder `docs`. Update existing documentation as necessary or add new files where documentation is missing.

Especially check the following topics:

- [ ] What is the purpose of the changes?
- [ ] How is the new functionality implemented?
- [ ] Are there any important details to note for future maintainers?
- [ ] Are there any potential pitfalls or caveats?
- [ ] Are there any related changes or dependencies?
- [ ] Are there any updates required for documentation?
```

Here is what the reviewer shows after it's first run:

```shell
The reviewer subagent ran successfully (I had to pin the variant to #high because the default medium variant is unavailable). Here's its report on your uncommitted changes:
Blocking
1. opencode.jsonc shouldn't be committed with this change — it's personal tooling config (hard-coded username marcel, model anthropic/claude-sonnet-5-5, shell: /pwsh) and is self-contradictory: permissions allows git push → ask, while policies denies shell:git push *. Committing it would block git push for collaborators. Remove it or gitignore it.
Should fix
1. Stale docs — docs/articles/persistence.md lines 90-91, 111, 120 still say compaction/embedding cleanup live in the main menu / recipe menu. They've moved to Settings → Database.
2. "Reachable from main menu" isn't really tested — tests/settings/test_command_line_handler.py:10-29 only checks choice-list contents, never the main.py:49-50 dispatch. A typo in the dispatch would still pass.
3. Missing cancel/None and back-navigation tests — settings.show() and _show_database_menu() both handle None, but nothing tests None/CANCEL returning to the parent menu or that no task executes.
4. Test structure vs repo conventions — test_settings_database_menu_is_reachable_from_main_menu asserts two menus at once, and test_old_menus_no_longer_contain_database_tasks mixes main- and recipe-menu assertions. Split them per the "one aspect, one essential assert" checklist.
Nits
 1. CANCEL_COMMAND overloaded for three meanings — a distinct BACK_COMMAND would read better.
 2. settings/command_line_handler.py:66 constructs a full persistence.CommandLineHandler() just to call compact().
 3. recipes/command_line_handler.py:87-97 — thin public wrappers around private _create_embeddings/_cleanup_embeddings; renaming the private methods would avoid the duplicate API.
 4. Exact-order assertions on menu lists are brittle.
1.  docs/specs/#53.md:270 TODO still unchecked despite implementation.
2.  (Pre-existing) unused e in src/main.py:55.
Looks good
Architecture matches the spec: menu code in settings, functionality still in persistence/recipes; main and recipe menus no longer expose the moved tasks; no leftover references to the old command constants anywhere outside settings and its test.
Want me to fix any of the should-fix items (docs, tests, or the config file)?
```

There are some really interesting spots highlighted by the reviewer. It finds some misconfiguration and explains the consequences to me. It should not be blindly trusted. I found some code smell the AI did not mention. Some were very obvious as redundant code, some were more subtle like an architectural inconsistency. The AI only finds what it is aware of in it's context. Anyway, it provides some useful guidance for my own review. Starting with this output a new prompt can be written to get rid of some technical debt and improve the code quality.

Same with the documentation agent. It also finds some outdated documentation, even the implementing agent has added new documentation. So I guess a second rund by a specialized agent might add some value and help future me keep understanding the application.