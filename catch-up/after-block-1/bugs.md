# RetroFlow bug list

> **Example file.** This is what a team typically has at the end of Block 1: the bugs found by hand this morning, each one reproduced. It is not the complete list. Block 2 is where you go looking for the rest. The ids in the responses come from one instance, yours will be different.

Severity: Critical (a business rule silently broken, data loss, or a security exposure), High (a core flow broken), Medium (works, but wrong, or with a workaround), Low (cosmetic).

---

## BUG-01: Creating a retrospective returns 200 instead of 201

* **Severity:** Low
* **Found by:** Pair AB, TC-06
* **Steps to reproduce:**
  1. `POST /api/teams` with `{"name":"AB Bug01","members":["Alice"]}`, note the `id`
  2. `POST /api/teams/{id}/retrospectives` with `{"title":"Sprint 14 retro"}`
* **Expected:** `201 Created`, the REST convention for creating something (assumption: the brief says "standard REST status codes", not which one)
* **Actual:** `200 OK` with `{"id":1,"team":{...},"title":"Sprint 14 retro","date":"2026-11-19","status":"OPEN"}`. All other create endpoints return 201.
* **Business impact:** a client that checks for 201 treats a successful create as a failure. Inconsistent with the rest of the API.

## BUG-02: Fetching a retrospective that doesn't exist returns 500 with a Java stack trace

* **Severity:** High
* **Found by:** Pair AB (TC-09b) and Pair CD (session 1, 10:05)
* **Steps to reproduce:**
  1. `GET /api/retrospectives/999` (an id that doesn't exist)
* **Expected:** `404` with a short error, like `GET /api/teams/999` gives: `{"status":404,"error":"Team 999: not found"}`.
* **Actual:** `500` with `"error":"Internal Server Error"` and a `"trace"` field with the full Java stack trace: package names (`com.stackcraft.retroflow.entity.Retrospective`), Spring, Hibernate and Jackson class names.
* **Business impact:** a client can't tell "doesn't exist" from "the server is broken". The stack trace tells anyone which framework runs behind it and how the code is organised. The same trace shows up on a 400 for an invalid priority (session 1, 10:11).

## BUG-03: A team can be created without members

* **Severity:** Medium
* **Found by:** Pair CD (session 1, 09:54), confirmed by Pair AB (TC-02)
* **Steps to reproduce:**
  1. `POST /api/teams` with `{"name":"CD Empty","members":[]}`
* **Expected:** `400`, no team created. Brief: "A team must have at least one member".
* **Actual:** `201` with `{"id":4,"name":"CD Empty","members":[]}`. Leaving out `members` entirely gives the same result.
* **Business impact:** teams without members can't be used (nobody can submit feedback in their name), but they show up in data and reports.

---

## Questions for StackCraft (not bugs, until someone says so)

* **Feedback from a non-member is accepted** (`submittedBy: "Mallory"`, 201). The brief doesn't say it has to be a member. Session 1, 10:14.
* **Closing a closed retrospective returns 200.** It doesn't reopen anything, so the rule holds. Is an error expected? TC-12.
