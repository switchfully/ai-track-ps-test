# RetroFlow test case catalogue

> **Example file.** This is what a team typically has at the end of Block 1: every rule has test cases, and a part of them ran against the live app. "Not run" means exactly that: nobody knows the result yet. The ids in the responses come from one instance, yours will be different.

Expected results marked **(assumption)** come from REST conventions, not from the brief. The brief doesn't give status codes.

## Teams

| ID | Rule | Preconditions | Steps | Expected | Actual result |
|---|---|---|---|---|---|
| TC-01 | Create a team | none | `POST /api/teams` with `{"name":"Team CM","members":["Alice","Bob"]}` | 201 (assumption), body has an `id`, the name and both members | **201** `{"id":1,"name":"Team CM","members":["Alice","Bob"]}`. Pass |
| TC-02 | "A team must have at least one member" | none | `POST /api/teams` with `"members": []` | 400 (assumption), no team created | **201** `{"id":2,"name":"Empty CM","members":[]}`. **Fail, see BUG-03** |
| TC-03 | Validation | none | `POST /api/teams` with `"name": ""` | 400 (assumption) | **400** `{"status":400,"error":"name: name is required"}`. Pass |
| TC-04 | Get a team | TC-01 | `GET /api/teams/{id}` | 200, same body as TC-01 | **200**, same body. Pass |
| TC-05 | Not found | none | `GET /api/teams/999` | 404 (assumption) | **404** `{"status":404,"error":"Team 999: not found"}`. Pass |

## Retrospectives

| ID | Rule | Preconditions | Steps | Expected | Actual result |
|---|---|---|---|---|---|
| TC-06 | Create a retrospective | a team | `POST /api/teams/{teamId}/retrospectives` with `{"title":"Sprint 14 retro"}` | 201 (assumption), status `OPEN` | **200** with status `OPEN` and a date. **Fail on the status code, see BUG-01** |
| TC-07 | "Only one can be OPEN at a time" | TC-06, retro still open | Create a second retrospective for the same team | 409 or 400 (assumption), no second retro | **409** `{"error":"Team 1: already has an open retrospective"}`. Pass |
| TC-08 | "Only one can be OPEN at a time", at the same moment | a fresh team | Two create requests for the same team, sent at the same time | One succeeds, the other is rejected | **Not run.** Postman sends one request at a time. Carry to Block 2 |
| TC-09 | Get one retrospective | TC-06 | `GET /api/retrospectives/{id}` | 200 with the retrospective | **200** with the retrospective. Pass |
| TC-09b | Get a retrospective that doesn't exist | none | `GET /api/retrospectives/999` | 404 (assumption), like `GET /api/teams/999` | **500** with a long Java stack trace in the body. **Fail, see BUG-02** |
| TC-10 | Get all retros of a team | TC-06 | `GET /api/teams/{teamId}/retrospectives` | 200, a list | **200**, a list with the retro. Pass |
| TC-11 | Close a retrospective | an open retro | `PUT /api/retrospectives/{id}/close` | 200, status `CLOSED` | **200**, status `CLOSED`. Pass |
| TC-12 | "Once closed, it cannot be reopened" | TC-11 | Close the same retro again. There is no reopen endpoint | Unspecified in the brief | **200**, status stays `CLOSED`. Not a reopen. Question: should a second close be an error? |
| TC-13 | After closing, a new retro may be opened | TC-11 | Create a new retro for the same team | 201 (assumption) | **200**, status `OPEN` (same status code issue as TC-06) |

## Feedback items

| ID | Rule | Preconditions | Steps | Expected | Actual result |
|---|---|---|---|---|---|
| TC-14 | Add to an OPEN retro | an open retro | `POST /api/retrospectives/{id}/feedback-items` with content, type `WENT_WELL`, submittedBy | 201 (assumption) | **201** with the item. Pass |
| TC-15 | Validation | an open retro | Same, with `"content": ""` | 400 (assumption) | **400** `{"error":"content: content is required"}`. Pass |
| TC-16 | CLOSED: cannot **add** | a closed retro | Add a feedback item | Rejected, 409 (assumption) | **409** `{"error":"Retrospective 1: cannot add feedback to a closed retrospective"}`. Pass |
| TC-17 | CLOSED: cannot **modify** | a feedback item, then close the retro | `PUT /api/feedback-items/{id}` with new content | Rejected, 409 (assumption) | **409** `{"error":"FeedbackItem 1: cannot modify feedback in a closed retrospective"}`. Pass |
| TC-18 | CLOSED: cannot **delete** | a feedback item, then close the retro | `DELETE /api/feedback-items/{id}` | Rejected, 409 (assumption), the item still exists | **Not run.** Ran out of time |

## Action items

| ID | Rule | Preconditions | Steps | Expected | Actual result |
|---|---|---|---|---|---|
| TC-19 | Add an action item | an open retro | `POST /api/retrospectives/{id}/action-items` with priority `HIGH` | 201 (assumption), `completed: false` | **201**, `"completed":false`. Pass |
| TC-20 | Mark as completed | TC-19 | `PATCH /api/action-items/{id}/complete` with `{"completed":true}` | 200, `completed: true` | **200**, `"completed":true`. Pass |
| TC-21 | "Completed action items cannot be uncompleted" | TC-20 | Same request with `{"completed":false}` | Rejected, stays completed | **Not run** |
| TC-22 | Filter by priority **and** completed | items with different priorities and states | `GET /api/action-items?priority=HIGH&completed=false` | Only items that are HIGH **and** not completed | **Not run** |

## Coverage after Prompt 5

| Rule | Covered? | What it would NOT catch yet |
|---|---|---|
| Only one OPEN at a time | TC-07, one request after the other | Two requests at the same moment (TC-08, not run) |
| A closed retro cannot be reopened | TC-12, partly | There is no reopen endpoint, so only indirect ways. Not every operation on a closed retro is tried |
| Closed: no modify or delete | Add and modify pass (TC-16, TC-17) | **Delete was not run (TC-18).** One operation passing says nothing about the others |
| Completed cannot be uncompleted | Completing works (TC-20) | **The rule itself was never tried (TC-21)** |
