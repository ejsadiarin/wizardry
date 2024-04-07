---
tags:
  - Interview
date: 2024-03-25T11:11
title: SRE/DevOps Interview Questions
---
<!-- 2024-03-25-1111 (March 25, 2024 11:11 AM) -->

# SRE/DevOps Interview Questions
### 1. Is a five nines SLO good or bad.
  - Bad, that gives you a very small error budget. Plus the user doesn't give a shit about nines. They care about using your product. 
### 2. Why is Configuration as Code important?
  - Among other things, prevents configuration drift and allows you to build infrastructure very quickly and consistently. 
### 3. Should I automate everything or just some things?
  - No right answer, but i'd want the canddiate to give me a logical answer to that. Personally I'm an automate as much as possible as long as it makes sense type of guy. 
### 4. Can you explain the CAP theorem?
  - [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem) - network partitions and consistancy vs availability.
### 5. Give me a non technical explanation for immutability.
  - Pressing an elevator button changes state from wait to summon. Pressing it again doesn't change the state any further. *WRONG. That's idempotent, not immutability. I meant what is Idempotent.
### 6. What does SRE mean to you?
### 7. Tell me about the current or most recent stack is implemented?
  - Helps me understand where in the stack they have experience and lots of opportunities for follow-up questions.
### 8. See [What happens when you type google.com into your browser and press enter?](https://github.com/alex/what-happens-when)
  - An attempt to answer the age old interview question "**What happens when you type google.com into your browser and press enter?**"
> Like most of my interview questions, it's an open  -ended question, and everyone can answer it to some extent. The extent proves seniority. I think that right-or-wrong trivia questions can be unfair, and stress out the interviewee too much. It's our job as interviewers not to let the interviewee tilt!
  >
  > The question is a well-worn cliché at this point, so most people that have prepared for a DevOps/SRE interview have probably seen this one at some point.
  >
  > Even just memorizing the answer makes you a better engineer, because many of these details are important, especially when it comes to debugging networking problems.
  >

# Resources
- [This Reddit Post](https://www.reddit.com/r/sre/comments/1bmu5un/interview_questions_for_sredevops_candidates/)
- [Substack: Daily Interview Questions](https://gotyanged.substack.com/p/daily-devops-interview-questions)
