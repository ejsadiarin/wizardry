---
date: 2024-09-25T20:44
tags: 
- Wizardry
- How-To
---
<!-- 2024-09-25-2044 (September 25, 2024 08:44:05 PM) -->

# How to Improve in Tech (Go-focused)

What does it mean, to you, to level up? What are you feeling is lacking? Who do you look up to and why?

For me, there are multiple ways to improve that count as up leveling. And they don't always involve your language.

One is pure technical understanding in Go: do you know the language and what makes code idiomatic, how to write good tests, how to structure a project. Do you have some familiarity with common standard lib packages and common public packages? Do you really grasp the sync package? Can you write lower level components, like a wire protocol? Can you profile and understand how the stack and heap work?  Can you use delve to debug?

Another way to up level is Go agnostic: can you build operable software? Do you leverage structured logging, metrics, and tracing? Do you have alerting? Are errors actionable? Do you have runbooks? Documentation? Can your thing be safely restarted? Do you have more advanced measurements to govern SLOs and SLIs? Error budgets?

Yet another is architecture: this can count structure of the application, but more towards how do you scale the application? What failure modes are there? How do you protect downstream services from unneeded load? How do you scale the datastore(s)? Be familiar with distributed system problems and why at-least-one and at-most-once delivery are important concepts.

Yet another way to improve: increase your impact on the organization. Do things that uplift more people than just you. Share Go knowledge and learnings, set up standards that are org specific, speed things up for everyone (speed up testing, reduce friction, improve response times during incidents by making things more operable).

I would expect a senior Go software developer to just nod at everything I wrote and likely point out obvious things I missed. 