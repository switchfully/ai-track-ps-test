# Block 2: Going deeper

New pairs! You're now working with someone you haven't paired with yet today.

## Now you choose how you go deeper

In Block 1 everyone did the same thing: plan, design and explore by hand, with an AI chat. This block, you and your pair choose a track:

* **Automation:** an **agent** writes and runs executable tests against the live API. You steer it.
* **Manual & Exploratory:** you go further and harder at the app by hand, with an AI chat as your thinking partner. No code, on purpose.

Both are real QA work, and both are judged on the same thing: **is your evidence convincing?**

**From now on, you write the prompts yourselves.** In Block 1 you got them ready-made. Now we only tell you the goal, what a good prompt for it contains, and what to check afterwards. How you word it is up to you.

Time: **75 minutes**, right after the 15 minutes of theory. Choosing your track and reading the Block 1 files takes the first 10.

## The ideas you need for this block

Your coach went through these on the slides. Here they are again, so you can look them up while you work.

**A failing test, or an odd response: is it the app, or is it the test?** This is the judgement call of the whole block, for both tracks. There are three possible verdicts:

| Verdict | What it means | What you do |
|---|---|---|
| **Bug in the app** | The response breaks the rule in the brief | Log it in `bugs.md`, with the request and the response |
| **Bug in the test** | You (or the agent) read the contract wrong | Fix the test, and write down what you misread in `decisions.md` |
| **The rule is unclear** | Both readings are defensible | Somebody has to decide. Not you, not the AI. Write it in `decisions.md` as an open question |

The AI can help you reason about it, but only from the real response next to the rule in the brief. Not from code it can't see. And you read the response body yourself before you accept a verdict.

**Green doesn't mean good.** A test that only checks `assertThat(result).isNotNull()` is green when the retrospective is closed, and also when it isn't. It checks nothing. Ask of every test: *what would make it fail?* The manual version is a note that says "checked, looks fine", without the response you saw. A green suite, or a clean manual pass, only proves that what you checked works.

**The same five gaps, by code or by hand.** AI-assisted testing has the same blind spots again and again. Both tracks hunt for them:

| Gap | The agent (Automation) | You (Manual) |
|---|---|---|
| **Shallow checks** | Asserts on the body, not just the status | Read the body, not only the status |
| **No boundaries** | Empty, null versus missing, very long, enum edges | The same, in Postman |
| **No adversarial input** | Malformed JSON, wrong content type, extra fields | The same, in Postman |
| **No cross-entity checks** | Another team's id in the path, a feedback item from another retrospective | The same, by hand |
| **Dependent tests, no repeatability** | Run the suite twice, compare | Send the same request twice, compare |

**Chat versus agent (Automation).** In a chat, you are the hands: you copy the answer somewhere. An **agent** works in a loop: *look at the situation, decide on a step, do it, read the result, decide on the next step.* It reads several files at once, writes tests, runs them and reads the output. That's why it can get a suite running on its own, and also why it can do a lot of damage quickly. Your job changes: you **give instructions, review a plan and check what it changed.**

> An agent is not autonomous. It's a powerful tool that needs a skilled operator. You're the senior tester in the room, and the agent is a fast, confident junior who sometimes goes off on its own.

**The five habits of working with an agent.**

1. **Plan first, then act.** Never let an agent write files before you've seen its plan. Say in so many words that nothing may be written yet.
2. **Work in small steps, and let it stop.** "Do part one, run it, then stop and list what you added." A stop point is where you get the wheel back.
3. **Give it a way to check itself.** An agent that can run the tests reads the output and goes again. One that can't, guesses.
4. **Put hard limits on it.** The most important one: **a failing test is information, not a problem to make go away.** A stuck agent likes a green suite, and sometimes it gets there by loosening the assertion. That hides a bug. Say upfront that it may not change a test before it has given you a verdict.
5. **Read the diff, not the summary.** The agent's summary is a claim, like any other AI output. The **Source Control** panel (or `git diff`) shows every file it touched, and exactly what changed.

The Manual track uses the same idea in a chat: ask for the plan first, then act, then check.

**What a good agent prompt contains.**

| Line | Example |
|---|---|
| **Read first** | "The endpoints are in `retroflow-api-reference.md`. The rules are in the brief." |
| **The goal, and when it's done** | "One test per operation of the closed-retrospective rule." |
| **How it checks itself** | "Run the tests against the live API and report pass or fail per test." |
| **Stop points** | "Plan first, no files yet. After each part, stop and list what you added." |
| **Hard limits** | "Don't change a failing test before you gave me a verdict. No new dependencies." |
| **What to do when stuck** | "If the rule is unclear, say so. Don't decide." |

**Mind the budget.** Every message in a long chat gets read again, and an agent reads your files again on every step. Start a **new chat for each step**, and only paste what that step needs. Keep the plan in your prompt log, not in the chat.

## What you'll learn

By the end of this block you can:

- decide whether a failing test or an odd response means a bug in the app, a bug in the test, or an unclear rule
- make an agent plan before it acts, and check what it changed by reading the diff (Automation)
- go deeper by hand where the risk is highest, including a race (Manual)
- find tests and checks that can't fail
- find the gaps in your own testing, with an agent or by hand

## What you'll make

| Automation | Manual & Exploratory |
|---|---|
| Executable tests in `tests/` | A "Block 2 session" in `session-notes.md` (Role 1) |
| A verdict for every failing test (Role 1) | `scenarios.md` and its results (Role 2) |
| New bugs in `bugs.md` | New bugs in `bugs.md` |
| Your answers in `decisions.md` | Your answers in `decisions.md` |
| Both tracks: `prompt-log.md`, with every prompt and a verdict | |

## Choose your track (5 minutes, with your pair)

You make **one** choice with your partner: the track. Your role follows from what the other pair in your group picks.

#### Step 1: choose your track

| | **Automation** | **Manual & Exploratory** |
|---|---|---|
| What you do | An agent writes and runs executable tests against the live API. You steer it: you review its plan, read what it changed and judge every failing test | You test by hand with Postman or a browser. An AI chat helps you plan, generate scenarios and read responses, but you send every request yourselves |
| What you learn | How to keep an agent on a leash, and how to tell a bug in the app from a bug in the test | How to go deeper than a script would, and how to turn what you see into evidence |
| You need | An editor with an agent (or a Codespace), and at least one of you comfortable reading test code | Postman or a browser, and an AI chat. No code |
| You end with | Executable tests, a bug list, your decisions | Session notes, a bug list, your decisions |

How to decide: can at least one of you read or write automated tests (Playwright, Cypress, Postman scripts)? Then Automation is an option. Neither of you? Take Manual & Exploratory. This day is not about forcing code under time pressure, and Manual is not the lesser track. Both tracks are judged on the same thing: is your evidence convincing?

#### Step 2: your role follows

Each track has two roles. They exist so that two pairs of the same group can share a track without doing the same work.

| | **Role 1: the core rules** | **Role 2: what nobody checked yet** |
|---|---|---|
| Automation | Business rule tests against the live API, diagnosing every failure, and a real race on "one OPEN retrospective" | Expanding the suite with negative, boundary and adversarial tests, and checking that the new tests can fail |
| Manual & Exploratory | A focused exploratory session on the riskiest rules, including the race by hand with two Postman tabs | A checklist of boundary, malformed and cross-entity requests, run by hand, one by one |

* **The other pair chose the other track?** Then you take **Role 1**. The core rules come first.
* **Both pairs chose the same track?** One pair takes Role 1, the other Role 2. Decide together. In Automation, Role 2 starts from the tests Role 1 is building, so keep talking to each other.

Tell your coach your track and your role before you start. Your coach advises, and will say so when a choice clearly won't work. You choose again in Block 3, so this is not a decision for the whole day.

**Then read what your team made.** Before you write a single prompt, take 5 minutes to read the Block 1 files: `test-plan.md`, `test-cases.md`, `session-notes.md` and `bugs.md`. You build on them, so don't start from scratch. They'll probably show you a few things you missed this morning.

## How you work as a pair this block

* **Automation:** the driver writes the prompt and approves what the agent asks permission for. The navigator watches what the agent actually does (which files it writes, which commands it runs, what the test output says) and keeps the **Source Control** panel open. The navigator says "stop" when a test changes that shouldn't.
* **Manual:** the driver sends the requests. The navigator keeps the notes and the brief, and asks "what did you expect?" before every send.
* **Swap after every step.** Both of you write prompts, both of you check.
* **Write the prompt together, out loud.** Before you send it, ask each other: "What is it not allowed to do? Where will it stop? How does it know it's done?"

## How to read this block

Every step starts with three lines:

* **Read:** the files it works with. These exist already.
* **Create:** the file you make or change, and where it lives.
* **Done when:** what has to be true before you move on.

Then you get:

* **Goal:** what the result looks like
* **A good prompt for this:** what your prompt needs to contain. Not the words, the parts. Don't paste these lines into the chat, they describe what your prompt has to *do*
* **Then check it:** the things you check by hand afterwards

**Keep a prompt log.** Every time you write a prompt, paste it in `prompt-log.md` (next to `bugs.md`) and add one line: did it work, or what did you have to fix, and what did you change? For example:

```
## Role 1, step 1, the plan
PROMPT: [paste]
VERDICT: It planned one test for rule 3. Added "add, modify and delete are three tests" and ran it again.
```

This is the start of your team's prompt library, and in Block 3 you turn the best ones into a skill.

**Missing a file from the last block?** That happens: the AI's feedback takes time, and pairs rotate. Don't redo the last block. Your coach has example files (`test-plan.md`, `test-cases.md`, `session-notes.md`, `bugs.md` and `instructions.md`). Take only the one you're missing, or put it next to your own and take the parts you need.

**Stuck after two tries?** Ask your coach for the example prompt for that step. Looking is allowed, just write down in your prompt log what the example does that yours didn't.

**Out of requests?** Don't wait for it to reset. Work on your partner's machine or switch tools, and tell your coach.

**Where things live.** The same group folder as this morning, `retroflow-qa`. Automation tests go in a `tests/` folder inside it, or in your own test project if you bring one. New this block: `prompt-log.md` and `decisions.md`, next to `bugs.md`. Two pairs share one app, so keep putting your initials in every team name you create.

---

# Automation track

In this track, both roles work with an **agent**. This is an AI course, so the agent writes and runs the tests and you steer it. Role 1 builds the business rule tests and diagnoses what fails. Role 2 attacks the API with negative, boundary and adversarial tests.

## Setting up (Automation, about 10 minutes)

* **Read:** the Block 1 files, and `.github/copilot-instructions.md` (or `CLAUDE.md`) if your team made one
* **Create:** a folder with your test project in a git repository, and a `.env` file with your group's URL
* **Done when:** the chat says **Agent**, one example test runs green against your group's URL, and Source Control shows a clean working tree

* **Where do you work?** An agent needs a folder with your test project, in an editor that has an agent: VS Code with GitHub Copilot, with the chat switched from **Ask** to **Agent**. Your tests call your group's URL, so you don't need Java or the code of the app. Put the folder in a git repository (`git init`), because that is what makes the Source Control panel work.
* **No editor with an agent on your laptop?** Ask your coach for the link to the **RetroFlow tests starter**. Open it as a free **GitHub Codespace** (Code, Codespaces, Create codespace on main). It comes with Node.js and Playwright's API testing ready. Copy `.env.example` to `.env`, put your group's URL in it, and run `npm test`: the example test should pass. Enable **GitHub Copilot Free** (no credit card) for agent mode. It's capped at 50 requests a month, which is enough for one block as long as you watch your tokens.
* **Your own editor works too.** Then you choose the tool: Playwright, Cypress or Postman/Newman. Tell the agent which one in your first prompt. No idea? Playwright works for everyone, and the starter has it ready.
* **Your house rules.** Did you put the instructions file from Block 1 where your tool reads it (`.github/copilot-instructions.md`, or `CLAUDE.md` for Claude Code)? Then the agent follows your rules without you pasting them. You'll check it in the first plan.
* Open the **Source Control** panel on the left (or have `git diff` ready in a terminal). Every file the agent touches is listed there, and when you click one, you see exactly what changed.

> **One rule for this block: you don't edit any test by hand.** The only exceptions are `prompt-log.md`, `bugs.md`, `decisions.md` and the `.env` file with your group's URL. The `.env` file is setup, not a test. If the agent produces a weak test, you fix it by giving it another instruction. It will feel slower at times, but this is exactly how you find out where agents go wrong.

---

## Role 1: executable business rule tests and bug diagnosis

Your job: let the agent turn the core business rules into automated tests that run against the live API, and work out for every failure whether the app is wrong, or the test is.

About 75 minutes: step 1 takes 20, step 2 takes 15, step 3 takes 20 and step 4 takes 10. The rest is setup and the first request.

Before you start, send one request to the app yourselves, in Postman or the browser, so you know what the current state is.

### Step 1: plan, then write the tests

* **Read:** `retroflow-qa-brief.md`, `retroflow-api-reference.md` and your `test-cases.md` from Block 1
* **Create:** the test files in `tests/` (new). Paste the plan in `prompt-log.md`
* **Done when:** you approved a plan before any file was written, and there are executable tests for the four core rules that call the live API over HTTP

**Goal:** executable tests for the four core business rules, running against the live API. Not unit tests on internal methods: real HTTP calls to your group's URL. First a plan, then the tests.

The four rules:
1. A team can only have one OPEN retrospective at a time
2. A retrospective can be closed, but never reopened
3. Feedback items cannot be added to, modified in or deleted from a CLOSED retrospective
4. A completed action item cannot be uncompleted

A good prompt for this:
* Tells the agent what the application is and where it runs (the URL in `.env`)
* Lists the four rules, in the words of the brief
* Points it at `retroflow-api-reference.md`, so it doesn't have to guess the paths
* Says in so many words that the tests have to call the running API over HTTP
* Wants the status code AND the body checked
* Says that nothing may be written yet: you want a plan first
* Names the tool you want to use, if you work in your own editor

Then check it:
* Read the plan. One test per rule, or more? Does it cover the failure case of every rule, or only the happy path? Tell the agent what to change before you let it continue.
* Does the plan follow your house rules without you saying so? If not, is the instructions file being read?
* Then tell it to write the tests and run them against the live app.
* Go rule by rule. Does each test pass? If one fails, do NOT assume your test is wrong. Deciding that is exactly what step 2 is about.
* Look at rule 3. "Added to, modified in or deleted from" is three operations. Do you have three separate tests?

### Step 2: diagnose the failures

* **Read:** the failing tests, their actual responses and the brief
* **Create:** nothing in the tests yet, only verdicts. Genuine bugs go in `bugs.md`, what you learned goes in `decisions.md`
* **Done when:** every failing test has one of the three verdicts, with the response that backs it up

Some of your tests from step 1 will fail. Take them one at a time.

**Goal:** for every failing test, a verdict: a bug in the application, a bug in your test, or a rule that isn't clear enough and needs someone to decide.

A good prompt for this:
* Points the agent to the failing test, and asks it to show the request and the actual response (status code and body) next to the business rule from the brief
* Asks for one of those three verdicts, and the reasoning behind it
* Tells it to reason from the response only, and not to guess at code it can't see
* Says it may not change the test or anything else yet. Only a verdict

Then check it:
* Don't accept the verdict before you've read the response body yourselves.
* An agent likes a green suite. Watch for the moment it "fixes" a test by loosening the assertion. That hides a bug.
* Every genuine bug goes in `bugs.md`, with severity, exact reproduction steps (the request and the response) and business impact. Reproduce it once in Postman first. "Test failed" is not a bug report.
* Every test that was wrong: tell the agent to fix it, and write a line in `decisions.md` about what you learned about reading the API contract.

> **Watch for:** a prompt that says "make the tests pass". A stuck agent will pick the shortest way to green: change the test. If that happens to you, it's not a disaster, it's the lesson of this block. Look at the diff, write it in your prompt log, and add the limit.

### Step 3: edge cases and concurrency

* **Read:** the brief and `retroflow-api-reference.md`
* **Create:** one new test per case in `tests/`. Anything you find goes in `bugs.md`
* **Done when:** all four cases have a test against the live API, and you checked that the race test really races

**Goal:** an executable test, against the live API, for each of these:
1. Two requests to create a retrospective for the same team, fired at the same moment. Is "one OPEN at a time" still enforced afterwards?
2. Creating a team with an empty members list. What comes back?
3. Filtering action items on priority AND completed status together. Does the result satisfy both conditions, or just one?
4. Requesting a retrospective with an id that doesn't exist. What's in the error response? Does it leak anything about the implementation (a stack trace, class names, database error text)?

A good prompt for this:
* Describes each case, and what you expect to see
* For the race: says the requests really have to go out at the same time. A loop that sends one after the other proves nothing
* Asks what result would prove the rule holds, and what result would prove it doesn't
* For the error response: says what you count as a leak
* Asks for a plan first, and for the agent to stop after each case

Then check it:
* Run all four and read the actual responses. This is where several real bugs live.
* The race is the hardest one. Open the test in the Source Control panel and check that both requests are started before either one is awaited. If they aren't, tell the agent to fix the test itself, not just the assertion.
* After the race, how many OPEN retrospectives does that team have? That number is your evidence.
* Log everything you find in `bugs.md`. A real bug is a real bug, whether it was put there on purpose or not.

### Step 4: one operation at a time

* **Read:** your tests from step 1 and the closed-retrospective rule in the brief
* **Create:** three new tests in `tests/`, one per operation. Gaps go in `bugs.md`
* **Done when:** add, modify and delete on a CLOSED retrospective each have their own test, with their own pass or fail

**Goal:** three separate tests for the closed-retrospective rule: add a feedback item, modify one and delete one, each on a CLOSED retrospective. You get a pass or fail for each of them, not one answer for all three.

A good prompt for this:
* Names the three operations, one test each
* Says what a correct response looks like (the request has to be rejected)
* Tells the agent to run them against the live API and report per test: pass or fail

Then check it:
* This is the most likely place to find a real gap. Read the results carefully, and don't assume the three behave the same because they're related.
* Log every gap with full reproduction steps.
* Open the diff once more. Did the agent change something you didn't ask for?

---

## Role 2: let an agent expand the test suite

Your job: use the agent to add negative, boundary and adversarial tests across the whole API, in one coordinated operation. Everything goes through the agent, you don't write test cases by hand.

About 75 minutes: step 1 takes 10, step 2 takes 20, step 3 takes 20 and step 4 takes 10. The rest is setup.

> Start from the tests the Role 1 pair built. Your group has no Role 1 pair? Then your first prompt asks the agent for a small baseline suite for the four core rules (your Block 1 test case catalogue is good input), in the tool you know.

Your target is the five gaps from the table at the top of this file: shallow assertions, no boundary tests, no adversarial input, no cross-entity checks, and tests that depend on each other.

### Step 1: plan before you act

* **Read:** the tests in `tests/` (from the Role 1 pair, or none yet) and `test-cases.md`
* **Create:** nothing in the code. Paste the plan in `prompt-log.md`, you need it in step 2
* **Done when:** you have a plan per gap that you both agree with, and no file changed

Never let an agent start changing files before you've seen its plan. That's the habit of this block.

**Goal:** a plan, and no changed files yet. For each of the five gaps: which endpoints or scenarios need a new test, and why.

A good prompt for this:
* Tells the agent what the project is and which tests to look at
* Says in so many words that nothing may be written or changed yet
* Names the five gaps
* Asks for a plan per gap, not one long list

Then check it:
* Read the plan. Does it match what you can see in the current suite?
* Did it miss things? Did it flag things that aren't really gaps?
* Only continue when you agree with it. If you don't, tell the agent what to change in the plan.

### Step 2: let it execute

* **Read:** your plan and the existing tests
* **Create:** new tests in `tests/`. The existing passing tests stay as they are
* **Done when:** there are new tests for all five gaps, added one gap at a time, and the existing passing tests are unchanged

**Goal:** new tests for all five gaps, written by the agent, with you in control.

A good prompt for this:
* Points to the plan, so you don't have to repeat it
* Gives an order of work, and says the agent has to stop after each gap and list what it added
* Says every new test has to assert something specific. Nothing that passes whatever the API returns
* Sets limits: existing passing tests stay as they are unless they are provably wrong, and no new dependencies without asking you first

Then check it:
* Watch what the agent does between the gaps. This is the closest you'll get to seeing how an agent "thinks".
* Did it add a test with a weak or tautological assertion? Stop it, tell it why, and ask for a rewrite.
* Keep a running note of everything the agent did that surprised you, good or bad. You need it in step 4.

### Step 3: verify what it built

* **Read:** the new tests in `tests/`
* **Create:** the list of issues in `prompt-log.md`, then the fixes in `tests/`
* **Done when:** the suite gives the same result twice in a row, and you can explain five random tests in plain language

**Goal:** a list of everything that's wrong with the new tests, and then those things fixed. Two prompts, in this order.

A good prompt for the first one:
* Asks whether the new tests actually compile and run
* Asks to run the whole suite twice in a row and compare the results (a different result is a sign of flakiness or shared state)
* Asks to explain five random tests in plain language: what does each one really verify?
* Asks to find the tests that would pass whatever the API returns
* Says: list the issues, don't fix anything yet

And then a second prompt that tells the agent to fix what it listed.

Then check it:
* Compare the five explanations with the names of the tests. Do they match what you'd expect?
* Open the Source Control panel after the fixes. Did the agent change something you didn't ask for?

### Step 4: reflect

* **Read:** your `prompt-log.md` and the diff of what the agent changed
* **Create:** your answers in `decisions.md`
* **Done when:** all four questions are answered, in your own words

This one isn't a prompt, it's a conversation with your partner. Talk it through, and write your answers in `decisions.md`:
1. What did the agent generate that surprised you?
2. At what point, if any, did you stop trusting what it produced?
3. If you did this on a production test suite at a client, what guardrails would you want?
4. What would have happened if you hadn't looked at the plan first, and had merged everything because the suite was green?

---

# Manual & Exploratory track

You go further and harder at the app than you did in Block 1, aimed at the business rules that carry the most risk. You only need Postman (or a browser) and your AI chat. No code, on purpose. The AI is your thinking partner: it plans, generates scenarios and helps you read responses. You send every request yourselves.

## Role 1: deeper exploratory testing

About 75 minutes: step 1 takes 10, step 2 takes 15, step 3 takes 30 and step 4 takes 10. The rest is setup.

### Step 1: plan a focused session

* **Read:** `test-plan.md`, `session-notes.md` and `bugs.md`
* **Create:** the session plan, as a new section "Block 2 session" in `session-notes.md`
* **Done when:** you have a plan for 30 minutes, aimed at the rules you tested the least, with at least one deliberate attempt to break each of them

**Goal:** a plan for a 30-minute exploratory session, aimed at the rules you've tested the least so far.

A good prompt for this:
* Gives the AI your `test-plan.md`, `session-notes.md` and `bugs.md`
* Asks for a session of 30 minutes, aimed at the least-tested rules
* Asks for 2 or 3 things to try by hand per rule
* Says at least one of them has to be a deliberate attempt to break the rule, not just the expected path

Then check it:
* Does it really point at gaps, or does it repeat what you did in Block 1? If it's too similar, push back and ask for something sharper.

### Step 2: race the concurrency rule by hand

* **Read:** the "only one OPEN retrospective at a time" rule in the brief
* **Create:** the procedure, and a line for every attempt, in `session-notes.md`
* **Done when:** you ran the race at least 3 times and wrote down every attempt, also the ones where nothing went wrong

This is the hardest rule to test by hand, so it's worth doing properly.

**Goal:** a step-by-step procedure, for two people with Postman, to create two retrospectives for the same team as close together as you can manage, and a clear idea of what result proves the rule holds and what result proves it doesn't.

A good prompt for this:
* Names the rule you want to test, in the words of the brief
* Says you can't write code, so it all has to work with two Postman tabs (or a partner on a second machine)
* Asks for the exact steps, and for what each outcome means

Then actually do it with your partner:
* Each of you has the request ready in Postman, and you both hit send within a second of each other.
* Run it 3 or 4 times. A race condition doesn't always show up on the first attempt.
* After each attempt, list the team's retrospectives and count the OPEN ones. That count is your evidence.
* Write down every attempt in `session-notes.md`, including the ones where nothing went wrong.

### Step 3: run the session

* **Read:** the plan from step 1 and the procedure from step 2
* **Create:** your notes in `session-notes.md`
* **Done when:** the 30 minutes are up and every note has the four lines below

This one isn't a prompt. Set a timer for 30 minutes and work through the plan from step 1, plus the race from step 2. Keep your notes in `session-notes.md`, in the same format as this morning:

```
What I tried:
What I expected:
What actually happened:
Worth a closer look? Y/N
```

* Follow anything that surprises you. You're not bound to the list if something more interesting shows up.
* Found something that looks like a real bug? Reproduce it a second time before you move on. A bug you can't reproduce twice is weak evidence.

### Step 4: write up what you found

* **Read:** `session-notes.md`
* **Create:** one report per genuine issue, added to the shared `bugs.md`
* **Done when:** every report is factual, has all the fields, and you reproduced it from its own steps

**Goal:** a clear bug report for every genuine issue in your notes, in the format your team uses in `bugs.md`.

A good prompt for this:
* Gives the AI your session notes
* Asks for one report per genuine issue, with title, severity (Critical / High / Medium / Low), steps to reproduce, expected result, actual result and business impact
* Says to stay factual: no guessing at the root cause, and no inventing an issue that isn't in your notes

Then check it:
* Compare every report with your own notes before it goes into the shared `bugs.md`. Did it stay factual?
* Follow the steps of each report once in Postman. A step the AI invented shows up now.
* Merge into the same `bugs.md` as Block 1. One list for the whole team.

---

## Role 2: manual adversarial and boundary testing

Your job: the manual version of what the Automation track's agent does in code. You hunt for the same five kinds of gaps (see the table at the top of this file), but you do it by hand. For you, the last gap is the **repeatability check**: does sending the same request twice give the result it should?

About 75 minutes: step 1 takes 10, step 2 takes 30, step 3 takes 15 and step 4 takes 10. The rest is setup.

### Step 1: generate the scenario list

* **Read:** the brief, `retroflow-api-reference.md` and `bugs.md`
* **Create:** `scenarios.md` in the working folder (new file), with the checklist
* **Done when:** every scenario on the list has the exact request and the expected response, and the list is about RetroFlow, not about APIs in general

**Goal:** a checklist of scenarios you can run by hand in Postman. For each scenario: the exact request, and what a correct response looks like.

A good prompt for this:
* Tells the AI what the application is, and that you test by hand in Postman
* Names the kinds of scenarios: boundary and missing input per entity (team, retrospective, feedback item, action item), malformed requests, cross-entity consistency, and requests where sending the same thing twice should (or shouldn't) give the same result
* Asks for the exact request and the expected response for every scenario
* Gives it an exit: where the brief doesn't say what the response should be, it writes "unspecified"

Then check it:
* Read the list before you run anything. Does it cover the real entities and rules of this app, or does it look generic? If it's generic, push back.
* Check a few requests against `retroflow-api-reference.md`. Did it invent a path or a field?

### Step 2: run the checklist

* **Read:** `scenarios.md`
* **Create:** every result in `session-notes.md`, also the ones that behave correctly
* **Done when:** every scenario on the list has a result next to it

This one isn't a prompt. Work through the checklist in Postman, one scenario at a time, and write down every result in `session-notes.md`.

* Don't skip a scenario because you expect it to pass. Several real bugs in this app only show up on the boring cases.
* Something looks wrong? First try to explain in plain language why, together. Then ask the AI to help you read the response, and compare its explanation with yours.

### Step 3: interrogate the results

* **Read:** `session-notes.md`
* **Create:** a verdict per result in `session-notes.md`. Genuine defects go in `bugs.md`
* **Done when:** every "worth a closer look" result has a verdict, and you know which of the five gaps you under-tested

**Goal:** for every result you marked "worth a closer look": is it a defect, expected behaviour that just looked odd, or something you can't judge without more information? Plus an honest answer to the question: which of the five kinds of gaps did we not try enough?

A good prompt for this:
* Gives the AI your `session-notes.md`
* Asks for one of those three verdicts per result, based on the response and the brief
* Names the five kinds of gaps and asks which ones are under-tested

Then check it:
* Read the response body yourselves before you accept a verdict.
* The AI says a category is under-tested? Go back and try 2 or 3 more scenarios there before you move on.
* Log every genuine defect in `bugs.md`, reproduced and with full steps.

### Step 4: reflect

* **Read:** your `session-notes.md` and `prompt-log.md`
* **Create:** your answers in `decisions.md`
* **Done when:** all three questions are answered, in your own words

This one isn't a prompt either. Talk it through with your partner, and write your answers in `decisions.md`:
1. What did you find by hand that a quickly generated automated test would have missed, and why?
2. At what point, if any, did you stop trusting your own first read of a response and dig deeper?
3. If you handed this to someone who writes automated tests from your findings, what would you tell them so they don't lose anything?

---

## Done?

Before lunch at 12:45, you should be able to say yes to these. This is also what we look at in Retro 2. Your coach asks one pair to show their result on the shared screen, so be ready to open the diff, the notes or the bug list.

Automation, Role 1:
- [ ] The agent wrote the tests from a plan you approved, and you wrote none by hand
- [ ] All four core rules have executable tests running against the live API
- [ ] Every failing test has a verdict, and `bugs.md` has every genuine defect, with severity and reproduction steps
- [ ] Your concurrency test really races two requests. It doesn't run them one after the other
- [ ] Add, modify and delete on a closed retrospective are tested separately
- [ ] No test was made green by loosening an assertion

Automation, Role 2:
- [ ] You approved a plan before the agent changed any file
- [ ] The agent expanded the suite for all five gaps
- [ ] No tautological or shallow assertions are left in the new tests
- [ ] `decisions.md` has your answers about the agent

Manual & Exploratory, Role 1:
- [ ] You raced the concurrency rule by hand, more than once, and wrote down every attempt
- [ ] `session-notes.md` shows a focused session, not a repeat of Block 1
- [ ] `bugs.md` has every genuine defect, with severity and reproduction steps

Manual & Exploratory, Role 2:
- [ ] You ran a full adversarial and boundary checklist by hand, covering all five kinds of gaps
- [ ] Every genuine defect you found is in `bugs.md`
- [ ] `decisions.md` has your answers

Everyone together:
- [ ] `bugs.md` is one shared list, not a separate file per pair
- [ ] Your prompt log has every prompt you wrote, each with a one-line verdict
- [ ] You picked your **best** and your **worst** prompt from the log

## Before Retro 2

Think about these with your partner, so you have an answer ready:

1. How did you decide whether a failure was a bug in the app, in the test, or in your own reading?
2. What surprised you, from an agent or from your own hands? What was the difference between your best and your worst prompt?
3. What does "green", or a clean manual pass, really prove?

## Conclusion

A green suite or a clean pass only proves what you checked. This block was about checking the right things, and knowing what you didn't.

You've learned:

- how to decide whether a failing test means a bug in the app, a bug in the test, or an unclear rule
- why you let an agent make a plan before it starts, and check what it changed by reading the diff
- that a green test suite, or a clean manual pass, only proves that what you checked works
- what a good prompt needs to contain, so you don't get tests that can't fail
- how to find gaps in your own testing, by hand or with an agent

After lunch, you connect the AI to StackCraft's real QA standards, and turn what worked into a skill.
