---
date: 2026-01-28T18:02
tags:
    - system-design
---

<!-- 2026-01-28-1802 (January 28, 2026 06:02:05 PM) -->

# Developing a recruitment app (like VRICrew):

1. Usually this is justified by managers, but first I would assess the "why" (Why are we developing this in the first place? What is the end goal of the app? Is this tied to our business goals? Does it bring value to our stakeholders/customers?)

- After that, then I would start with the technicalities of the app (assess it on my own first):

2. First, I would create a system design spec/doc, which holds the details about:
    - **(1.1) functional reqs. (core features) of the app** - companies can create job posts, users can apply to job posts made by companies, users and companies can chat eachother, users can browse/filter/sort/search job posts, companies receive user details and resume when a user applies to their job posts, etc..
    - **(1.2) non-functional reqs. (attributes/operational contraints, SLAs)**: ensure 99% of user application requests ("Apply Now" request) should always be sent to the company who created the job post, app should comply to employment laws, etc.
    - **(2) core database models (that we can extend later on)** - [User] - _(id, name, resume_link, contact_number, linkedin_url, etc.)_, [Company] - _(id, name, description, location, contact_number, mission, vision, founded_year, link, etc.)_, [JobPost] - _(id, company_id, title, description, requirements, benefits, status, created_date, etc.)_, [ApplicationRequest] - _(id, user_id, job_post_id, status, created_date, etc.)_
    - **(3) core API endpoints** - this will be technical translations of our functional reqs. in 1.1 .
    - **(4) high level architecture (start simple, design for extension)** - depends on scale, but this would usually be a three tier application (ex. nextjs frontend, backend API/websocket server for chat sessions, postgresql database), best to start simple then scale from there when traffic demands it.

3. Ask for feedback and clarifications from managers/tech lead/etc. - if there are missing critical business details.
4. Reiterate and improve the plan with AI before development:
    - [PLANNING - in PLAN mode] Ask to verify the initial system design spec/doc, do research to create a comprehensive plan - in PLAN mode
        - "Based on the given system design document, validate ideas and assumptions through concrete, realistic research. Make sure to add sources for manual review and auditing. Don't assume, ask me for clarifications to backfill missing context about the project."
    - [PLANNING - in PLAN mode] Ask to create project phases in a PLAN.md file - our goal is to MVP.
        - "Create a PLAN.md based on the improved system design. Create Phases that serves as milestones to be completed so the context window wouldn't be overloaded."

5. Once a concrete PLAN.md is created, that's when I will start the actual development.
    - For this, I have my own global AGENTS.md file that I personally use for my programming style and workflow (as well as guardrails that makes sure the AI agent wouldn't lose context/state of the project). This is important for context management.
    - Then create a local AGENTS.md file that holds the details of the project/app to be created - usually a /init suffice.
    - [BUILD] Create tests that will be run every minor phase completion
    - [BUILD] Then i would go through each phases step-by-step.
    - [BUILD] Ask for logs creation (with timestamps) in logs/ directory for every change, git commits for easy rollbacks, while reviewing every added/edited files.
