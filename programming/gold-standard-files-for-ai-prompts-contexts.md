---
date: 2025-06-12T15:04
title: Gold Standard Files Approach for AI Prompts for Contexts
tags: 
    - Programming
---
<!-- 2025-06-12-1504 (June 12, 2025 03:04:13 PM) -->

# Gold Standard Files Approach for AI Prompts for Contexts

`devs: stop letting AI learn from random code. use "gold standard files" instead`

so i was talking to this engineer from a series B startup in SF (Pallet) and he told me about this cursor technique that actually fixed their ai code quality issues. thought you guys might find it useful. 

basically instead of letting cursor learn from random internet code, you show it examples of your actual good code. they call it "gold standard files." 

*how it works:* 

1. pick your best controller file, service file, test file (whatever patterns you use) 
2. reference them directly in your `.cursorrules` file 
3. tell cursor to follow those patterns exactly 

here's what their cursor rules looks like:

```txt
You are an expert software engineer. 
Reference these gold standard files for patterns:
- Controllers: /src/controllers/orders.controller.ts
- Services: /src/services/orders.service.ts  
- Tests: /src/tests/orders.test.ts

Follow these patterns exactly. Don't change existing implementations unless asked.
Use our existing utilities instead of writing new ones.
```
    

*what changes:* 

the ai stops pulling random patterns from github and starts following your patterns, which means: 

* new ai code looks like their senior engineers wrote it 
* dev velocity increased without sacrificing quality 
* code consistency improved 

*practical tips:* 

* start with one pattern (like api endpoints), add more later
* don't overprovide context - too many instructions confuse the ai 
* share your cursor rules file with the whole team via git 
* pick files that were manually written by your best engineers

*the key insight:* "don't let ai guess what good code looks like. show it explicitly." 

# Refs
- [https://www.reddit.com/r/LLMDevs/s/CGECxr3lfy](https://www.reddit.com/r/LLMDevs/s/CGECxr3lfy)
