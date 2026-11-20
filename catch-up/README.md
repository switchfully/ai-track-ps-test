# Catch-up files

**For the coach.** Hand out the `after-block-N` folders, not this README.

Example results of a block, for a team that didn't finish something the next block needs. Pairs rotate and AI feedback takes time, so this happens. Without these files a pair gets stuck on a step that asks for a file they don't have.

**Hand them out per block, never earlier.** `after-block-1/` is handed out at the start of Block 2, `after-block-2/` at the start of Block 3. `after-block-2/bugs.md` lists bugs that Block 2 is meant to find, so it spoils Block 2 if it's out before then.

**How a team uses them:** they take only the file they're missing, or put it next to their own and take the parts they need. Their own work stays theirs. A team that finished its own files doesn't need these.

## What's in here

| Folder | File | Needed in |
|---|---|---|
| `after-block-1/` | `test-plan.md` | Block 2, Manual Role 1 step 1 |
| | `test-cases.md` | Block 2, Automation Role 1 step 1 and Role 2 step 1. Block 3, Manual steps 4 and 5 |
| | `session-notes.md` | Block 2, Manual Role 1 step 1 |
| | `bugs.md` | Block 2, Manual Role 1 and Role 2 |
| | `instructions.md` | Block 2, setting up (the agent reads it) |
| `after-block-2/` | `bugs.md` | Block 3, step 1 and the Manual reviews |
| | `session-notes.md` | Block 3, the Manual reviews (it holds the captured responses) |
| | `tests/` | Block 3, the Automation audit. Copy into the Playwright starter's `tests/` folder |
| | `prompt-log.md` | Block 3, the skill |

Which bugs are in which folder, and what the Block 3 audit can still find in `tests/`: see "Catch-up files" in `../facilitator-guide.md`.
