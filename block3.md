# Block 3: Grounding, reviewing and skills

Last rotation of the day! New pairs again.

## From one-off prompts to reusable tools

In Block 2 you went deeper, with an agent or by hand. This block, you take two steps further:

1. You connect the AI to a **real source**: StackCraft's QA standards, served by an MCP server we host for you. From then on, its answers are based on a document you can check, instead of on what it remembers.
2. You turn the prompts that worked into a **skill**, so you and your team don't start from zero next time. Then you swap skills with the other pair and find out if yours works for someone else.

What we want you to notice is the difference between an AI that answers from what it was trained on ("this is what good test practice usually looks like") and an AI that reads StackCraft's actual QA standards and quotes them back to you. Both tracks get to see that difference, each in its own way.

**This block, you write all the prompts yourselves, with fewer hints than before.** For every task we tell you what the result has to look like, and how you check it. You've had a whole day of practice, so now it's up to you.

Time: **75 minutes**, right after the 15 minutes of theory. The last 25 minutes are for the skill.

## The ideas you need for this block

Your coach went through these on the slides. Here they are again, so you can look them up while you work.

**Grounding.** Left alone, an AI doesn't know StackCraft's QA standards. It answers from the patterns it saw elsewhere, and it can cite a "standard" that sounds exactly right and doesn't exist. **Grounding** means you give it the source it should rely on at the moment it answers. You've done a small version of this all day by pasting the brief and the real responses.
Grounding doesn't make the AI infallible. It makes the answer **checkable**: you open the document, look up the section it quoted, and see if it's there. Sometimes it isn't, and that's a finding in itself.

**MCP (Model Context Protocol)** is a standard way for an AI tool to ask an outside source for information, on demand. Think of USB-C: any device works with any port. Three words to know:

* **MCP server:** a small program that offers things. Ours offers documents. Others offer a bug tracker, a test management tool or a database.
* **MCP client:** your AI tool (Claude.ai, Copilot, Claude Code). It connects to the server.
* **Tools:** what the server offers, which the AI can call by itself in the middle of a chat.

Our server is called `ai-track-standards`. It has two tools, `list_documents` and `read_document`, and the document you need has the id `stackcraft-qa`.
Two things to remember: the AI only reads a document **when it decides to, or when you tell it to**, so name the server and the document id in your prompt. And reading is not understanding: it can still misquote a section. Look up what it quoted.

**Only plug in what you trust.** An MCP server is code someone else wrote, and its output goes straight into the AI's context. Our server only reads. Be careful with servers that can write, send or delete. Before you connect a tool at a client, ask: who runs the server, what can it read and write, and what leaves the machine?

**When AI reviews, ask for proof, not opinions.** "The API might leak internal details" sounds serious, but you can't act on it. "This request gives this response, and the body holds a stack trace with class names" you can reproduce in a minute, or throw out. Expect **false positives** and findings **outside the scope** you gave it. Sort every finding into three piles: **real** (proof you reproduced), **speculation** (plausible, no proof) and **invented** (nothing behind it).

**"A test exists" is not "the rule is verified".** AI will happily say a rule is covered because a test with a matching name exists. Follow every claim to the evidence:

| Claim | What it really tells you |
|---|---|
| "There's a test case for this rule" | Somebody wrote it down |
| "The test case was run" | Somebody ran it. What was the actual response? |
| "The test passed" | Does it check something that could fail? |
| "The rule is covered" | Only if you can point at the response that proves it |

**A skill** is a folder with a `SKILL.md` file in it. The file tells the AI how to do one specific task, your way. The folder can also hold a template, like the exact layout of a bug report.
The AI always sees the **name and description** of every skill it has. Only when your request matches a description does it load the full instructions. So the **description** decides whether your skill gets used at all.

| Use | When it's read | For | Example from today |
|---|---|---|---|
| **Instructions file** | Before every request | Rules for everything in this engagement | Your `instructions.md` from Block 1 |
| **Prompt file** | When you start it (`/name`) | A task you start yourself, the same way each time | A bug report prompt you run with `/bug` |
| **Skill** | When the request matches its description | A task the AI should recognise and do your way | `stackcraft-test-audit`, `stackcraft-release-review` |

None of these make the AI correct. They make it more consistent, and that makes checking a lot easier.

## What you'll learn

By the end of this block you can:

- connect an MCP server to your tool, and tell if it is really being used
- see the difference between an answer from memory and an answer grounded in a document
- let an AI audit or review your testing, and sort its findings into real, speculation and invented
- tell "a test exists" from "the rule is verified"
- write a skill, and test whether it gets picked up and whether a stranger can use it

## What you'll make

| What | Who | Where |
|---|---|---|
| An MCP connection | Both tracks | The connector in Claude.ai, or `.vscode/mcp.json` |
| The audit, and fixes for the Critical violations | Automation | `audit-findings.md`, and your tests |
| Four reviews and a release report | Manual & Exploratory | `review-notes.md` and `pre-release-quality-report.md` |
| A skill | Both tracks | A folder with `SKILL.md` (see the end of this file) |

## Choose your track again (2 minutes, with your pair)

Same two tracks as in Block 2, and you may pick a different one than before.

* **Automation:** you audit your test suite against StackCraft's QA standards, through the MCP server, and fix the worst of it.
* **Manual & Exploratory:** a structured review of RetroFlow before release, ending in a report with a release recommendation, which you check against StackCraft's QA standards through the MCP server. No code.

Not sure? Same rule as in Block 2: if at least one of you is comfortable reading test code, Automation is an option. Neither of you? Take Manual & Exploratory. Tell your coach which one you picked. Two pairs of your group on the same track? Work in parallel and compare notes at the showcase.

## How you work as a pair this block

Same roles as before, with one extra job for the navigator: **the navigator has the standards document open**, `stackcraft-qa-standards.md` from the course files, and looks up every section the AI quotes. The driver doesn't open it. That way you test the AI, not yourselves. In the Manual track, the navigator also has Postman ready to reproduce findings. Swap after every step.

## How to read this block

Every step starts with **Read** (what it works with, these exist already), **Create** (the file you make, and where) and **Done when** (what has to be true before you move on). Then:

* **Your mandate:** the job, in one or two sentences. How you prompt for it is up to you
* **The result is good when:** what the output has to contain
* **Then check it:** what you check by hand afterwards

Keep adding to your **prompt log** (`prompt-log.md`): the prompt, and one line on whether it worked or what you had to fix. Your best prompts are the raw material for the skill at the end of this block.

**Missing a file from the last block?** That happens: the AI's feedback takes time, and pairs rotate. Don't redo the last block. Your coach has example files (`bugs.md`, `session-notes.md` (with the captured responses), working tests for the Playwright starter, and `prompt-log.md`). Take only the one you're missing, or put it next to your own and take the parts you need.

**Stuck after two tries?** Ask your coach for the example prompt for that step. Looking is allowed, just write down what the example does that yours didn't.

**Watch your tokens.** Start a **new chat** for each step, and only paste what that step needs. Out of requests? Don't wait: work on your partner's machine or switch tools, and tell your coach.

**Where things live.** The same group folder as before, `retroflow-qa`, with everything from Blocks 1 and 2 in it. The standards document is `stackcraft-qa-standards.md` in the course files. New this block: `audit-findings.md` (Automation), `review-notes.md` and `pre-release-quality-report.md` (Manual), and your skill.

## Your block at a glance

A rough guide, not a rule.

| Part | Automation | Manual & Exploratory |
|---|---|---|
| Choosing your track and connecting the server | 10 min | 5 min |
| Step 1: feel the difference | 10 min | 10 min |
| The audit, or the four reviews | 15 min (audit) | 20 min (four reviews, about 5 minutes each. Short on time? Take two reviews each and merge) |
| Fixing, or the report and its standards check | 15 min (fix) | 15 min (report and check) |
| Build a skill and swap | The last 25 min. Don't skip the drafting, cut the swap if you must | The same |

---

## Setting up: connect the server (both tracks, about 10 minutes)

* **Read:** nothing yet, the standards come in step 1
* **Create:** the connector in Claude.ai (Option 1), `.vscode/mcp.json` (Option 2) or the Claude Code configuration (Option 3)
* **Done when:** your tool can use `ai-track-standards`, and `list_documents` returns `stackcraft-qa`

The server is hosted for you. There's no login and nothing to install.

* URL: `https://ai-track-ps-production.up.railway.app/mcp`
* Health check: open `https://ai-track-ps-production.up.railway.app/health` in your browser. You should see a short piece of JSON with `ok` in it.
* Name it `ai-track-standards`

Pick the option that fits what you have. The Manual track needs the server for step 1 and to test the skill at the end, and Option 1 is the easiest for you.

**Option 1: Claude.ai in your browser (a free account is fine).** Nothing to install.
* Sign in at [claude.ai](https://claude.ai).
* Go to **Customize**, **Connectors**, **Add custom connector**. Call it `ai-track-standards` and paste the URL. There's no login.
* Start a new chat, click **+**, **Connectors** and switch `ai-track-standards` on for that chat.
* For the skill at the end of the block, go to **Settings**, **Capabilities** and switch on **Code execution and file creation**. Skills don't work without it.

**Option 2: GitHub Copilot in VS Code, IntelliJ or a Codespace.** Create a file `.vscode/mcp.json` with this content:

```json
{
  "servers": {
    "ai-track-standards": {
      "type": "http",
      "url": "https://ai-track-ps-production.up.railway.app/mcp"
    }
  }
}
```

A small **Start** link appears above the server name in that file. Click it. Using IntelliJ? Go to Settings, Tools, GitHub Copilot, Model Context Protocol (MCP), **Configure**, and add the same block there. Then open the chat in **Agent** mode and look at the tools list: you should see `list_documents` and `read_document`.

**Option 3: Claude Code.** Run this in your terminal, then start Claude Code and type `/mcp`. The server should be listed as connected.

```
claude mcp add --transport http ai-track-standards https://ai-track-ps-production.up.railway.app/mcp
```

**Check it with one prompt:**

```
List the documents that are available through the ai-track-standards MCP server.
```

You should get three ids: `stackcraft-coding`, `stackcraft-qa` and `meridian-regulatory`. You only need `stackcraft-qa`.

Nothing shows up? Check, in this order: does the health URL load in your browser? Did you switch the connector on in *this* chat (Claude.ai), or click **Start** (VS Code)? Did you open a new chat after adding the server? Still nothing after 5 minutes? Tell your coach, and switch to Option 1: Claude.ai connects from its own cloud, so your network doesn't have to reach the server. Curious about what you're connecting to? `ai-cheat-sheets/what-is-mcp.md` explains it.

---

## Both tracks, step 1: feel the difference

* **Read:** the standards document (through the server), and your own `bugs.md`
* **Create:** nothing. Write both prompts and what you saw in `prompt-log.md`
* **Done when:** you asked the same question twice, once without and once with the connector, and you can say what was different

You're going to ask the same question twice, each time in a **new** chat. For this one we give you the question, so both runs are comparable:

```
What does StackCraft's QA standard say about tautological assertions like
assertThat(result).isNotNull()? How severe is a violation, and what does a bug report
have to contain?
```

1. **First without the connector.** A chat where `ai-track-standards` is switched off. Ask the question as it is.
2. **Then with the connector.** Switch it on, open a new chat, and add one sentence at the end of the question that makes the AI use the server and quote the document. You write that sentence.

Then check it:
* The first answer: the AI has never seen StackCraft's standards. Did it tell you that, or did it come up with something that sounds plausible? Is anything in it invented?
* The second answer: did the tool call show up in the chat? You should see something like "read_document". If it didn't call it, the answer isn't grounded, however good it looks.
* The navigator looks up what it quoted in `stackcraft-qa-standards.md` (section 2.1 and section 4.1). Does it exist? Is it quoted word for word?
* Now look at your own `bugs.md` from this morning. Does it match the format the standards ask for?

The second answer isn't smarter. It's **checkable**. That's the point of this block.

---

# Automation track

## Auditing your test suite against the standards

**Your mandate:** find out where your test suite breaks StackCraft's QA standards, and fix the worst of it.

### Step 2: the audit

* **Read:** the inherited `RetroflowApiTest.java`, the tests you and the other pair made today, and the standards document through the MCP server
* **Create:** `audit-findings.md` (new file). No changes to the tests yet
* **Done when:** the audit meets the list below, and you checked three quotes and two findings yourselves

Point the agent at the test folder, or paste the full suite into the chat. The suite you audit is the inherited one, plus everything your team added today.

The result is good when:
* Every violation names the file and the test method
* Every violation quotes the section of the standards it breaks
* Every violation explains why it's a violation, and what the correct approach is
* The violations are sorted by severity, Critical first, using the severity in the standards document
* No test changed. This step only reports

Then check it:
* The navigator takes three violations and looks up the quoted section in the document. Can't find it? Then the AI made it up. Write it down, that's great material for Retro 3.
* Take two violations and open the test it names. Is the problem really in there?
* Look for **false positives** (it flags something that is fine) and for **missed findings** (a weak test you know about that isn't in the list).
* Look at what the audit found, and then at what it didn't. Can a test suite also break the standards by what's *missing*, not just by what's wrong?

### Step 3: fix the Critical violations

* **Read:** `audit-findings.md`
* **Create:** the fixes in your own tests, one per Critical violation. Mark each finding as fixed in `audit-findings.md`. A violation that is a real bug goes in `bugs.md` as well
* **Done when:** every Critical violation is fixed or rejected with a reason, and the suite still runs

**Your mandate:** fix every Critical violation, one at a time.

The result is good when:
* Every fix is a separate step, and you run the tests after each one
* Every fix has a short comment: what was wrong, and which section of the standards it's based on
* A violation that turns out to be a real bug in the application doesn't just get fixed in the test: it's also in `bugs.md`
* A finding you disagree with is rejected with a reason, not silently skipped

You know the habits from Block 2 by now: a plan first, a stop after each fix, and a failing test is information.

Then check it:
* Open the Source Control panel (or run `git diff`). Did the AI change more than you asked?
* A test that turned red after its fix: is that the app being wrong? Then it's a bug, not a broken fix.
* Run the whole suite yourselves. Is it still running, and did it get *better*, or only greener?

Done? Go to the skill at the end of this file.

---

# Manual & Exploratory track

## The release review

**Your mandate:** produce the review a senior test engineer would present before RetroFlow goes out to 12 teams. This time you use AI as a **reviewer**, not just as a generator. You don't need any code, only what your team made today.

The interesting part: the AI will find real issues AND invent fake ones. It's up to you to tell them apart.

You do four reviews, and then you bring them together in one report:
1. **Security.** What could go wrong if this API had a wider audience than expected?
2. **API contract.** Is the API well designed, and does it behave the way it's documented?
3. **Business rule coverage.** Is every rule in the brief verified, by an executable test or by something your team confirmed by hand?
4. **Test quality.** Do the checks that exist, automated or manual, actually test the right things?

### Step 2: the security review

* **Read:** your `bugs.md`, `session-notes.md`, the responses you captured today and the brief
* **Create:** a section "Security" in `review-notes.md` (new file)
* **Done when:** every finding has a severity, the exact request, the actual response and a fix, and every Critical or High finding is reproduced by you

Remember: you can only check what you can see by sending requests. This is not a review of the source code.

The result is good when:
* It covers input validation, information disclosure, endpoints that return more data than a caller needs, and anything else you can test from the outside
* Every finding has a severity (Critical / High / Medium / Low), the exact request that shows it, the actual response and a recommended fix

Then check it:
* The brief says no authentication is required for this internal tool. If the AI flags missing authentication anyway, note it: AI reviewers don't always respect the scope you gave them.
* The navigator reproduces every Critical or High finding in Postman before it goes into your report.

### Step 3: the API contract review

* **Read:** the responses you captured today, `retroflow-api-reference.md` and the brief
* **Create:** a section "API contract" in `review-notes.md`
* **Done when:** every check has PASS, FAIL or CONCERN, and every FAIL is backed by a response you captured

The result is good when:
* It looks at the HTTP methods (does a GET ever change something?), the status codes, whether the error responses have the same structure everywhere, and anything that would surprise someone who uses the API
* It gives PASS, FAIL or CONCERN for each of those
* Every FAIL or CONCERN comes with a finding and a fix, based on an actual response you collected today. Not on something hypothetical

Then check it:
* Cross-check every FAIL against a response you captured. A finding you haven't reproduced doesn't go in.
* Look at what the create-retrospective call returns. What does your evidence say about the status code?

### Step 4: the business rule coverage review

* **Read:** the brief, `test-cases.md`, `session-notes.md` and `bugs.md`
* **Create:** a section "Business rule coverage" in `review-notes.md`
* **Done when:** every rule has a status, and every "covered" points at a recorded result that confirms it

This is the most important review of the day. Use the table "A test exists is not the rule is verified" at the top of this file.

The result is good when:
* Every rule from the brief is in there, and the closed-retrospective rule is checked as three separate operations
* For every rule it says: covered, missing, or partly covered
* For a covered rule it points to the exact artifact (test case, session note or bug) and says whether the recorded result confirms that the rule holds
* For a missing rule it says what happens if this ships without coverage

Then check it:
* Check every finding against your recorded results. A test case that exists is not the same as a test case that was run and passed.
* The AI says a rule is covered, and you know it was never really confirmed? Put it in your report as a known gap, not as a covered rule.

### Step 5: the test quality review

* **Read:** `test-cases.md`, `session-notes.md`, and `RetroflowApiTest.java` (the inherited suite)
* **Create:** a section "Test quality" in `review-notes.md`, and stronger evidence for what you find weak
* **Done when:** every weak check is named, and you strengthened at least one of them with a recorded response

The result is good when:
* It finds checks that pass but don't test the right thing, rules without real coverage, checks that are too broad or too narrow, and anything that gives a false sense of security
* Every finding names the specific test case or note, and explains the problem
* Where evidence is weak, it shows what stronger evidence would look like

Then check it:
* This one is adversarial on purpose. The AI is reviewing work that may have been made with AI too. Does it catch its own mistakes, or your team's?
* A manual test case that says "checked, looks fine" with no recorded response is the manual version of `assertThat(result).isNotNull()`. Look for it in your own notes.
* Strengthen the weak evidence it found. Those are real gaps.

### Step 6: the report

* **Read:** `review-notes.md`, `bugs.md`, and the standards document through the MCP server
* **Create:** `pre-release-quality-report.md` in the working folder (new file)
* **Done when:** the report has all six parts below, it passed the standards check, and you both agree with the recommendation

**Your mandate:** one document, built from the four reviews and your bug list from today, that you could realistically hand to the StackCraft CTO.

The result is good when it has:
* An executive summary of 3 or 4 sentences
* The bugs, with severity, reproduction steps and business impact (from `bugs.md`)
* The critical issues (must fix before release), the major ones (should fix) and the minor ones (fix when time allows)
* What's tested and trustworthy (at least 3 things)
* What you did NOT have time to verify
* A release recommendation: BLOCKED / CONDITIONAL / APPROVED

Then check it:
* Does the report say honestly what you didn't get to? Or does it make it sound like everything was covered? The CTO said in the brief that "we don't know" beats a clean report that turns out to be wrong.
* Can you name one finding that turned out to be a false positive, and one that was real?
* Do you agree with the recommendation? You'll have to defend it at the showcase.

**Last: the standards check.** StackCraft has rules for what a report to stakeholders has to contain, and they're in the same document you read in step 1. Swap roles, open a **new** chat with the `ai-track-standards` connector switched on, and paste in your report.

**Your mandate:** let the AI check your report against StackCraft's QA standards, read through the server. Ask for every violation with the section number and a word-for-word quote, and for the place in your report where it goes wrong.

The result is good when:
* The tool call shows up in the chat ("read_document"). No tool call means no grounded check: start again.
* Every violation has a section number, a quote and a place in your report
* It covers at least the bug report format, what a report must say about what wasn't tested, and how a release recommendation has to be explained

Then check it:
* The navigator looks up every quoted section in `stackcraft-qa-standards.md`. Does the section exist? Is the quote word for word?
* Fix the real violations in the report. A violation you disagree with stays, with one line in `review-notes.md` on why.
* Did the check change your recommendation? If a bug without a recorded response was holding it up, that bug is now a guess, not evidence.

Done? Go to the skill at the end of this file.

---

## Both tracks, the last 25 minutes: build a skill

By now you've written a lot of instructions for the same kind of check. A skill saves you that work.

Which skill do you build? That depends on your track:

* **Automation** builds **`stackcraft-test-audit`**: a skill that audits any test suite against StackCraft's QA standards.
* **Manual & Exploratory** builds **`stackcraft-release-review`**: a skill that reviews your tests and your evidence before a release, and writes the report the StackCraft way.
* **Both pairs of your group on the same track?** Then the second pair builds **`stackcraft-bug-report`** instead: a skill that turns raw notes or a failing test into a bug report that follows the standards.

### Step 7: draft the skill

* **Read:** your `prompt-log.md` and the prompts that worked best in this block
* **Create:** a folder with the name of your skill, and in it `SKILL.md` (new). In Copilot: `.github/skills/<skill-name>/SKILL.md`. In Claude Code: `.claude/skills/<skill-name>/SKILL.md`. For Claude.ai: any folder, you zip it in the next step
* **Done when:** the skill exists, it's under 40 lines, and its description says what it does and when to use it

A `SKILL.md` always looks like this. That's the format, and the AI doesn't know it by heart:

```
---
name: the-name-in-lowercase-with-hyphens
description: One or two sentences. What the skill does and when to use it.
---
The instructions: the steps to follow, what to check, what sources to use, the exact
output format, and what never to do.
```

**Your mandate:** have the AI write the `SKILL.md` for the task you just did, based on the prompts that worked best for you today. Your prompt log is the source material.

The result is good when:
* It follows the format above
* It's short: under 40 lines. Longer? Then it's two skills, or the detail belongs in a template file next to `SKILL.md`
* It tells the AI where the **source of truth** is, so it reads the QA standards through the MCP server (`ai-track-standards`, document `stackcraft-qa`) instead of using what it remembers
* It has the checks you learned to do by hand as explicit steps (for example: "quote the section for every finding", or "a rule is only covered if a recorded response confirms it")
* It says what the AI must never do
* The instructions are specific enough that someone else could follow them without asking you anything

Then **edit it yourselves**. Spend most of your time on the description: it decides whether the skill gets loaded.

| Weak description | Better |
|---|---|
| `Helps with testing.` | `Audits an API test suite against StackCraft's QA standards. Use when asked to review, audit or check tests or test quality against company standards.` |

A good description says **what it does** and **when to use it**, in the words a colleague would actually use when asking.

> **Level up:** a skill can be more than one file. Put a template next to `SKILL.md` (the exact layout of a bug report, for example) and refer to it from the instructions.

### Install it and swap

* **Read:** the skill of the other pair in your group
* **Create:** a short comparison at the bottom of `decisions.md`: what did their skill do with your material that you hadn't done yourselves?
* **Done when:** you ran the other pair's skill, and gave them one piece of feedback

Install it:
* **Claude.ai:** put `SKILL.md` in a folder with the same name as the skill, zip that folder and upload it under **Customize**, **Skills** (the **+** button). Switch it on, and switch the `ai-track-standards` connector on in the chat where you use it.
* **GitHub Copilot in VS Code or a Codespace:** `.github/skills/<skill-name>/SKILL.md`. Copilot picks it up by itself, and you can also start it with `/<skill-name>` in the chat. Not showing up? Check that agent skills are switched on in the VS Code settings, and tell your coach.
* **Claude Code:** `.claude/skills/<skill-name>/SKILL.md`.

Then swap with the other pair, and test their skill in a **new chat**, on your own material from today, without explaining what it does. A skill you haven't tested is a guess, so you run three tests:

1. **The trigger test.** Ask for the task in plain words, **without** naming the skill. For example: "Check these tests against our QA standards" on `RetroflowApiTest.java`, "Is this ready to release?" on your `bugs.md` and `test-cases.md`, or "Write this up as a bug report" on your `session-notes.md`. Did the skill get picked up? If not, the description is the problem.
2. **The output test.** Does the result follow the format the skill asks for? Did it read the standards through the server (look for the tool call), or answer from memory?
3. **The stranger test.** Read the other pair's skill as if you'd never seen RetroFlow. Mark every sentence you'd have to ask them about.

Give the other pair **one** concrete piece of feedback. Most often it's about the description.

> **Watch for:** a skill that only works when its author is the one using it. That's a note to yourself, not a skill.

---

## Finished early? Automation: what else could we connect?

* **Read:** `ai-cheat-sheets/what-is-mcp.md`
* **Create:** a heading "MCP exploration" in `decisions.md`
* **Done when:** it has 5 real MCP servers on it, each one checked

You've now seen MCP work with one hosted server. Think about what else you'd want to connect to in a real QA workflow at a client: a test management tool, a bug tracker, a monitoring platform.

**Your mandate:** find out which MCP servers really exist for that.

The result is good when:
* You have a list of 5 real MCP servers, not made-up ones
* For each: what it connects to, what you could do with it in a testing workflow, and what you'd have to think about before using it at a client (security!)

Then check it:
* Look up every server on the list. Does it exist? MCP is new enough that the AI's training data may be incomplete or outdated, so this is a good test of how much you can trust it.

## Finished early? Manual & Exploratory: strengthen your weakest evidence

* **Read:** your `review-notes.md`, section "Test quality"
* **Create:** the extra evidence in `session-notes.md`, and the changes in `pre-release-quality-report.md`
* **Done when:** at least one weak piece of evidence is replaced by a recorded response

**Your mandate:** take the weakest piece of evidence your test quality review found, a case that says "checked, looks fine" without a response. Redo it by hand in Postman and write down the real status code and body. Then update the report: did the result change what you said about that rule?

---

## Done?

This is what we look at in Retro 3 and in the showcase. Your coach asks you to show proof, not to tell a story: the quotes you checked, the finding you reproduced, the skill doing its work in a fresh chat.

Automation:
- [ ] You saw the difference between an answer with and without the connector
- [ ] You have an audit with quoted sections, and you checked at least three of those quotes
- [ ] The Critical violations are fixed one by one, or rejected with a reason
- [ ] Your skill exists, and the other pair tested it

Manual & Exploratory:
- [ ] You saw the difference between an answer with and without the connector
- [ ] All four reviews are done, and `pre-release-quality-report.md` is saved
- [ ] The report passed the standards check, and you checked the quoted sections
- [ ] You can name one finding that turned out to be a false positive, and one that was real
- [ ] You have a release recommendation, and you can defend it
- [ ] Your skill exists, and the other pair tested it

Both tracks:
- [ ] `bugs.md` has every genuine defect of the whole day
- [ ] Your prompt log is complete, and you know which of your prompts you'd reuse on a real project

## Before Retro 3 and the showcase

Get your proof ready: the quotes you checked, the finding you reproduced, and your skill's name and description. Then think about these with your partner:

1. What changed in the answer when the AI could read the document?
2. What did the audit or the reviews find that you wouldn't have found yourselves, and what did they get wrong?
3. Did the other pair's skill get picked up on its own? What would you change in its description?
4. Which of your prompts would you reuse on a real project on Monday?

## Conclusion

You started the day with one-off prompts. You end it with house rules, tests or evidence you can defend, a grounded source and a reusable skill.

You've learned:

- that grounding the AI in a real document makes its answers checkable, not just better
- what MCP is, how to connect a server, and how to see if it's really being used
- how to check whether a quoted standard really exists
- why AI reviewers find real issues and invent fake ones, and why you reproduce the important ones yourselves
- why "a test exists" and "a test was run and passed" are two different things
- what a skill is, how it differs from an instructions file, and why the description matters so much
- how to brief an AI when all you have is the result you want
