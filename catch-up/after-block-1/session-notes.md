# Session notes

> **Example file.** This is what a team typically has at the end of Block 1: one exploratory session, with what was tried and the real responses. The ids come from one instance, yours will be different.

## Session 1 (Block 1, Pair CD, 30 minutes)

**Charter:** explore team creation and the retrospective lifecycle, with the rules in the brief as the target, to find out where validation and status handling don't match the brief.

| Time | What we tried | What came back | Note |
|---|---|---|---|
| 09:52 | Created team "CD Phoenix" with two members | 201, team with id | as expected |
| 09:54 | Created a team with `"members": []` | **201** `{"id":4,"name":"CD Empty","members":[]}` | brief: "A team must have at least one member". Bug |
| 09:56 | Team without a `members` field at all | 201, `"members":[]` | same problem |
| 09:58 | Team with `"name": ""` | 400 `name: name is required` | validation on the name works, not on members |
| 10:01 | Created a retro for "CD Phoenix" | **200** and status `OPEN` | creating returns 200, not 201 |
| 10:03 | Fetched that retro with `GET /api/retrospectives/{id}` | 200 with the retro | as expected |
| 10:05 | `GET /api/retrospectives/999` | **500**, body has `"trace"` with about 50 lines of Java, starting with "Could not write JSON: Unable to find com.stackcraft.retroflow.entity.Retrospective with id 999" | should be 404, like a missing team. Leaks package and class names |
| 10:08 | Second retro while the first is open | 409 "already has an open retrospective" | rule holds, one request after the other |
| 10:11 | Action item with `"priority": "URGENT"` | 400, but the body also has a stack trace (`HttpMessageNotReadableException ... not one of the values accepted`) | right status, but it leaks internals too |
| 10:14 | Feedback item with `submittedBy: "Mallory"`, not a member | 201 | the brief says "the name of the team member who submitted it". Question for StackCraft, not sure it's a bug |
| 10:17 | Closed the retro, closed it again | 200 both times, stays CLOSED | no reopen possible that we found |
| 10:20 | Added feedback to the closed retro | 409 "cannot add feedback to a closed retrospective" | rule holds for adding |

**Not tried yet:** deleting or modifying feedback in a closed retro, un-completing an action item, the combined filter, two creates at the same moment.

## Inherited test suite

What we learned from reading `RetroflowApiTest.java` (not run):

* `createRetrospective_succeeds` checks `is2xxSuccessful()`. Any 2xx passes, so it can't notice that the API returns 200 instead of 201.
* `closeRetrospective_updatesStatus` only checks that the body is not null. It would pass even if the status stayed `OPEN`.
* Nothing tests a closed retrospective's feedback being deleted, an action item being un-completed, both filters together, an empty member list, two creates at the same time, or a retrospective that doesn't exist.
* All tests are green, and that says very little: they test that the happy path answers, not that the rules hold.
