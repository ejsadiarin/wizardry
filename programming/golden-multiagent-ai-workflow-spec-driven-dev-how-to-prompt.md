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

* Initializer (setting up repository)
    1. Run `pwd`
    2. **Set up a `feature-list.json` file**: Based on the input spec, set up a structured JSON file with a list of end-to-end feature descriptions
    3. Initial git repo setup with progress logs `progress.txt` 
    4. Write `init.sh` script for starting development server

* Continuing from a progress state session 
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
