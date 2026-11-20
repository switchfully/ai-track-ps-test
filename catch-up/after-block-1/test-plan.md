# RetroFlow test plan

> **Example file.** This is what a team typically has at the end of Block 1. Use it if your own `test-plan.md` didn't get finished, or put the parts you're missing next to yours. It is a solid plan, not a perfect one.

## What StackCraft worries about

The CTO's question: can RetroFlow go in front of 12 teams on Monday without embarrassing anyone? From the brief: *"Tell me what's actually broken, tell me what you're not sure about, and tell me what you didn't have time to check."*

So we rank on what goes wrong **silently**: a rule that is broken without anyone noticing does more damage than an error message.

## Risk priority order (agreed by the team)

| # | Business rule (from the brief) | Likelihood | Impact | Why this rank |
|---|---|---|---|---|
| 1 | "Feedback items in a CLOSED retrospective cannot be modified or deleted" | Medium | High | A closed retro is the team's record. If items can still change, nobody can trust what was agreed. Three operations to check: add, modify, delete. |
| 2 | "Only one can be OPEN at a time" | High | High | RetroFlow is collaborative: two team members opening a retro at the same moment is normal use. A check-then-create without a lock breaks under that. |
| 3 | "Completed action items cannot be uncompleted" | Medium | High | Action items are the reason RetroFlow exists ("whether action items were ever resolved"). A silent un-complete makes the follow-up data worthless. |
| 4 | "Once a retrospective is closed, it cannot be reopened" | Low | High | There is no reopen endpoint, but closing twice or creating around it may still change state. |
| 5 | "A team must have at least one member" | Medium | Medium | Validation "exists on most endpoints, do not assume it is complete". |
| 6 | Filtering action items "by priority and by completed status" | Medium | Medium | The combination of both filters is easy to get wrong. |
| 7 | Standard REST status codes and error bodies | High | Low to Medium | Wrong codes break clients. Error bodies can leak internals. |

The AI ranked "at least one member" second at first. We moved it down: it is visible (an empty team is easy to spot) and the brief's CTO worries about silent problems more.

## Test levels

* **API level, by hand (Block 1):** Postman requests against our group's instance, every result written down with the real status code and body.
* **Exploratory (Block 1 and 2):** a charter per session, aimed at the top 3 rules.
* **Automated API tests (Block 2, Automation):** executable tests for the four core rules, running against the live API.
* **Out of scope:** UI (there is none), load testing, a full security test.

## Entry and exit criteria

* **Entry:** the instance answers, `POST /api/teams` returns a team.
* **Exit:** every one of the four core rules has a result we recorded ourselves (pass, fail or "not checked, and why"). Every genuine defect is in `bugs.md` with a reproduction.

## Test data

* Every team we create has our initials in the name, so we can tell our data from the other pair's.
* A fresh team per test case where possible, because "only one OPEN retrospective" makes reused teams unreliable.

## Assumptions (not in the brief)

* The brief doesn't give status codes. We expect REST conventions: 201 for create, 404 for not found, 400 for invalid input, 409 for a business rule conflict. These are **assumptions**, and we mark them as such in `test-cases.md`.
