# RetroFlow in 5 minutes

The brief tells you what RetroFlow **should** do. The API reference lists the endpoints. This page puts them together: what the app is about, how a normal retrospective goes through it, and what the data looks like. Read it once with your partner. Then paste the block at the bottom into every new chat, so the AI starts from the same picture as you.

## What RetroFlow is

Twelve teams at StackCraft hold a retrospective at the end of every sprint: what went well, what needs to improve, and what will we do about it. Today that ends up on sticky notes and Miro boards, and nobody knows afterwards whether the action items ever got done. RetroFlow is a small backend to keep it in one place. There is no screen, only a REST API. You talk to it with Postman.

## The four things in it

```
Team  ──has many──▶  Retrospective  ──has many──▶  Feedback item
(name, members)      (title, date,                 (content, type, submittedBy)
                      OPEN or CLOSED)
                                    ──has many──▶  Action item
                                                   (content, priority, submittedBy,
                                                    completed)
```

* A **team** is a name and a list of member names. There are no logins: anyone can act as anyone.
* A **retrospective** belongs to one team. It starts `OPEN` and can be closed. The app fills in the date.
* A **feedback item** is one note in a retrospective, of type `WENT_WELL`, `NEEDS_IMPROVEMENT` or `ACTION_ITEM`.
* An **action item** is a to-do that comes out of a retrospective, with a priority (`LOW`, `MEDIUM`, `HIGH`) and a `completed` flag. It has its own endpoints.

## A normal retrospective, step by step

This is the happy path. Every step is one request from `retroflow-api-reference.md`.

| # | What happens in real life | Request |
|---|---|---|
| 1 | A team starts using RetroFlow | `POST /api/teams` with a name and members |
| 2 | The sprint ends, the team opens a retro | `POST /api/teams/{teamId}/retrospectives` with a title |
| 3 | Everyone adds notes | `POST /api/retrospectives/{id}/feedback-items`, once per note |
| 4 | Someone fixes a typo in a note | `PUT /api/feedback-items/{id}` |
| 5 | The team agrees on actions | `POST /api/retrospectives/{id}/action-items` |
| 6 | The retro is over | `PUT /api/retrospectives/{id}/close` |
| 7 | Next sprint: the team opens a new retro | back to step 2 |
| 8 | During the sprint, actions get done | `PATCH /api/action-items/{id}/complete` |
| 9 | The lead checks what's still open and important | `GET /api/action-items?priority=HIGH&completed=false` |

The business rules in the brief are about what may **not** happen along this path: a second retro while one is open, changing notes after closing, reopening a closed retro, un-doing a completed action, a team without members. That's where you test.

## What the data looks like

These are the fields you'll see in responses. Ids are numbers the app hands out, so yours will be different. What the app returns in status codes and error messages is not here on purpose: that's what you find out today.

```json
Team:
{ "id": 1, "name": "Team CM-Phoenix", "members": ["Alice", "Bob"] }

Retrospective (with its team inside):
{ "id": 1, "team": { "id": 1, "name": "Team CM-Phoenix", "members": ["Alice", "Bob"] },
  "title": "Sprint 14 retro", "date": "2026-11-20", "status": "OPEN" }

Feedback item (with its retrospective inside):
{ "id": 1, "retrospective": { ... }, "content": "The demo went smoothly",
  "type": "WENT_WELL", "submittedBy": "Alice" }

Action item (with its retrospective inside):
{ "id": 1, "retrospective": { ... }, "content": "Fix the flaky deploy script",
  "priority": "HIGH", "submittedBy": "Bob", "completed": false }
```

## Good to know before you prompt

* **Your whole group shares one instance.** Lists show the other pair's data too. Put your initials in every team name.
* **The brief doesn't give status codes.** When the AI says "this returns 409", it's guessing from REST conventions. Mark it as an assumption until Postman shows you.
* **The brief is the source of truth, not the AI.** If the AI states a rule that isn't in the brief, it made it up.

## Paste this at the start of a chat

Copy this block, and add the brief and the API reference below it when the prompt needs them.

```
Context: I'm a test engineer testing RetroFlow, a REST API (no UI, no login) that StackCraft
wants to roll out to 12 teams. Teams hold retrospectives in it.
- A team has a name and members. A retrospective belongs to a team, has a title, a date and
  a status OPEN or CLOSED. Feedback items (WENT_WELL, NEEDS_IMPROVEMENT, ACTION_ITEM) and
  action items (priority LOW/MEDIUM/HIGH, completed true/false) belong to a retrospective.
- Normal flow: create team, open retro, add feedback and action items, close retro, later
  mark action items as completed and filter them.
- The business rules are in the brief I'll paste. The endpoints are in the API reference I'll
  paste. Status codes and error messages are NOT documented: anything you say about them is
  an assumption, so label it that way.
- I test against a running instance with Postman. Don't present a predicted response as a
  result. Use only the brief and the API reference, and say when something isn't in them.
```
