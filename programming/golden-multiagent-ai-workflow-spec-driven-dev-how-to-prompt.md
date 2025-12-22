---
date: 2025-11-28T16:51
title: Golden Multiagent AI Workflow (Spec-driven Development) - How to Prompt
tags: 
    - How-to
---
<!-- 2025-11-28-1651 (November 28, 2025 04:51:25 PM) -->

# Golden Multiagent AI Workflow (Spec-driven Development) - How to Prompt

- ref: [Anthropic - Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)  

## Notes

* This is optimized only for full-stack web development, general programming, development, etc.
* Works best with multi-agent architecture (ex. testing agent, QA agent, UI agent, planning agent, etc.)
    * this is best for subtasks, as agents are best at doing one thing excellently.
    * let agents work together (ex. output of testing agent is input -> to QA agent)
* Adopt an incremental workflow for clean working state every session (see using `git logs`, `progress.txt` and `feature-list.json` for this)
* **This mimics effective software development practices that top companies do (with real people), but here its with AI agents.**

> [!IMPORTANT]
>  Your prompts (human) should follow the following structure and technique for reproducible results:
>  ```txt
>  Task: (Make this simple and narrow scoped, ONE PROMPT = ONE GOAL, clear success criteria) - ex. replace "make it engaging" with "include 3 code examples"
>  Input: (Files, text, web results, any context, etc.) - ex. use specific versions and exact requirements
>  Constraints: (Tell AI what NOT to do explicitly) - ex. "Go code, use standard OpenAPI spec, etc."
>  Output: (Expected result - should be REPRODUCIBLE and easy to verify) - same prompt should work next week, next month, and so on.
>  ```

## Idea (detailed)

1. Run `pwd`
    * check current working directory (files that are allowed to edit)
    * Action: run `pwd`
2. Read `git log --oneline -20` output
    * for seeing current progress
    * Action: run `git log --oneline -20` to check current progress via its commit messages
3. Have a progress log `progress.txt`
    * for tracking what the agent had done
    * Action: `git commit` its progress with descriptive messages then write summaries of its progress to `progress.txt` file
4. Features file list `feature-list.json`
    * for listing features spec and its status 
    * choose highest priority that is not yet done 
    * agents are less likely to change structure of JSON files than markdown 
    ```json
    {
        "category": "functional",
        "description": "New chat button creates a fresh conversation",
        "steps": [
            "Navigate to main interface",
            "Click the 'New Chat' button",
            "Verify a new conversation is created",
            "Check that chat area shows welcome state",
            "Verify conversation appears in sidebar"
        ],
        "passes": false
    }
    ```
    * Action: read `feature-list.json` file and choose feature to work on 

5. Have a `init.sh` script to start development server and do verification testing
    * Let agent run this and navigate the application
    * Let it verify if core fundamental features are still working (ex. chat features, theme switching, conversation loading, and error handling) - explicit prompt to use browser automation (Puppeteer MCP)
    * Review `tests.json` comprehensively to understand what needs to be implemented next
    * ONLY edit `passes` field to `true` in `feature-list.json` only if it is passing
    * Action: run `init.sh` script and do verification testing for core fundamental features. Necessary to test before passing a feature (changing to `"passes": true` in `feature-list.json`)

## Workflow for long-running agents

* Initializer - setting up repository for this workflow (for humans, AI can help in planning as well)
    1. **Set up a `feature-list.json` file**: Based on the input spec, set up a structured JSON file with a list of end-to-end feature descriptions - **input spec for features requires human interference**
    2. Set up initial git repo (if not yet done)
    3. Create a progress log `progress.txt` file for agents to write their progress to
    4. Write `init.sh` script for starting development server
    5. Paste the prompt instructions for the agent workflow (see below).
        * NOTE: you need to set the roles of each agent inside the prompt instructions.

* Continuing from a progress state session (for AI agents)
    1. Run `pwd`
    2. Check the `feature-list.json` file and choose highest priority feature with `"passes": false` status
    3. Run `git log --oneline -20` to see recent work 
    4. Run `init.sh` script to start development server and navigate the application and verify if fundamental features are working (verification testing).
    
## Example agent session

```txt
[Assistant] I'll start by getting my bearings and understanding the current state of the project.
[Tool Use] <bash - pwd>
[Tool Use] <read - claude-progress.txt>
[Tool Use] <read - feature_list.json>
[Assistant] Let me check the git log to see recent work.
[Tool Use] <bash - git log --oneline -20>
[Assistant] Now let me check if there's an init.sh script to restart the servers.
<Starts the development server>
[Assistant] Excellent! Now let me navigate to the application and verify that some fundamental features are still working.
<Tests basic functionality>
[Assistant] Based on my verification testing, I can see that the fundamental functionality is working well. The core chat features, theme switching, conversation loading, and error handling are all functioning correctly. Now let me review the tests.json file more comprehensively to understand what needs to be implemented next.
<Starts work on a new feature>
```

## Prompt Instructions

```txt
As an AI agent, when continuing from a progress state session, follow these steps:
1.  **Understand Current Environment**: Begin by running `pwd` to confirm the current working directory and available files.
2.  **Identify Next Task**: Read the `feature-list.json` file. Prioritize and select the highest priority feature that currently has a `"passes": false` status.
3.  **Review Recent Work**: Execute `git log --oneline -20` to review the most recent commits and understand the progress made in previous sessions.
4.  **Verify Fundamental Features**: Run the `init.sh` script to start the development server. Navigate the application to perform verification testing, ensuring that core fundamental features (e.g., chat, theme switching, conversation loading, error handling) are still fully functional.
5.  **Implement and Test**: Based on the selected feature from `feature-list.json`, implement the necessary changes. After implementation, thoroughly test the feature.
6.  **Update Feature Status**: Only if the implemented feature passes all tests, update its `"passes"` field to `true` in `feature-list.json`.
7.  **Log Progress**: `git commit` your changes with descriptive messages and update the `progress.txt` file with a summary of your work.
```
