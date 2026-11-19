# Block 1: Prompting well

Pair AB and Pair CD work in parallel. Choose in your group who is A, B, C and D, if your coach hasn't done that already.

## Let's start testing RetroFlow

This block, you plan, design test cases and explore the application by hand, all with an AI chat. **Nobody writes or edits code in this block, on purpose.** Whether you automate tests day to day or work entirely manually, everything below uses Postman (or a browser) and your AI chat tool. From Block 2 on, you and your pair choose between an Automation track and a Manual & Exploratory track. Block 1 is the same for everyone, so there is nothing to choose yet.

The goal of this block is not to find every bug. The goal is to build the testing and prompting habits that make the rest of the day faster and safer.

You get ready-made prompts in this block. In Block 2 and 3 you write your own. So while you use these, look at how they're built. After most prompts you'll find a **CO-STAR check** that points out which parts of the prompting model are in it, and which are missing. That's how you learn to build your own.

Time: **75 minutes**, right after the 15 minutes of theory. The last 15 minutes are for both pairs together.

## The ideas you need for this block

Your coach went through these on the slides. Here they are again, so you can look them up while you work.

**An AI only knows two things: what it learned in training, and what is in front of it right now.** Training gave it the general patterns: what a typical test plan or bug report looks like. It has never heard of StackCraft or RetroFlow. Those only exist in the **context**: your prompt, the chat so far, and the brief and responses you give it. When something is missing from the context, the AI doesn't stop and ask. It fills the gap with what is most common, and it doesn't tell you it made a choice. The brief doesn't say which status code a rejected request returns? The AI will predict one, and write it down as if it were a fact.

**A prompting model is a checklist, so you don't forget a part.** We use **CO-STAR**, plus three lines we always add:

| Part | The question it answers | Example from today |
|---|---|---|
| **C**ontext | What's the situation, and what should it work from? | "I'm taking over QA for RetroFlow before its first release. Here is the brief." |
| **O**bjective | What exactly must it do? One clear job | "Make a risk-based test plan for the business rules." |
| **S**tyle | How should it work? | "Like a test lead who has to explain the priorities to a CTO." |
| **T**one | What attitude? | "Critical and precise. Say when you're not sure." |
| **A**udience | Who reads the result? | "A tester who is new to the project." |
| **R**esponse | What should the output look like? | "A table: rule, risk, how to test it, priority." |
| **Rules** | What must it do, and never do? | "Use only the brief. No code." |
| **Evidence** | How does it show its work? | "Quote the sentence of the brief behind each rule. Mark what you guess." |
| **Stop points** | Where does it hand control back? | "Make a plan first, don't write anything yet." (Block 2) |

You don't need all of them every time. Write the objective first, then ask yourself: "What would the AI have to guess now?" Add the line that removes that guess. The **Rules** line is where most of the quality is decided. And give it an **exit**: "If the brief doesn't say, write *unspecified*." Without an exit, a model prefers a plausible answer over an honest gap.

**Ask for less, so you can check all of it.** An AI gives you as much as it can: three pages of plan, 30 test cases, 25 "gaps". All of it looks useful, and you have to check every line before you can use it. That's where the time goes. So limit it in the prompt: "At most 10 points, the most important first." or "Only the 5 biggest risks." You can always ask for more later. It's a **Response** line, and it's the one people forget most.

**Give it the evidence, not your memory of it.** Paste the brief, not your summary of it. Paste the real response, not "it gave an error". Your summary already holds your assumptions. The source and the response are the truth.

**A predicted response is not a result.** The AI can tell you what it *expects* the API to return. Only Postman can tell you what it *does* return. A test case you didn't run is not a pass.

**You can't check everything, so check in this order.** From cheap to expensive:

1. **Let a machine check it.** Send the request. Look at the real status code and body.
2. **Look up what it quoted.** A quote you can't find in the brief is invented.
3. **Read the lines that hurt.** Business rules, status codes, the body of an error. Skip the boilerplate.
4. **Ask it back.** "What did you assume? What would this test case not catch?" This only works if you know enough to judge the answer.
5. **Get a second opinion.** A new chat or another model. Not the same chat: it tends to agree with itself.

## What you'll learn

By the end of this block you can:

- let an AI turn a brief into risk priorities, and decide for yourself which risk comes first
- build a prompt from CO-STAR parts, and tell which part carries the quality
- get a test case catalogue, and check it against the real application
- run a timed exploratory session with a charter
- write bug reports that someone else can reproduce
- write your house rules in an instructions file, and see it change an answer

## The application, and where things live

**The application is already running for you.** Your coach gives your group a URL. That is RetroFlow, and everything today happens against it. There is nothing to install or start. Wherever a prompt below says `localhost:8080`, use your group's URL instead. The endpoints are listed in `retroflow-api-reference.md`.

> Your group shares one instance of the app, so you will see each other's data in lists. Give every team you create a name that contains your own initials, and you'll always know which data is yours.

**The course files** (the QA brief, the API reference, the inherited test suite `RetroflowApiTest.java`, these exercises and the cheat sheets) are shared by your coach. You read from them. You don't change them.

**Your working folder.** Everything you create today goes in one folder for your group, called `retroflow-qa`. You rotate partners every block, and Block 2 builds on what you made in Block 1, so the folder has to be shared. Agree in your group how you do that. A git repository is the easiest, because the Automation track needs one later anyway. A shared drive folder works too. "The root of the working folder" in these exercises always means the root of `retroflow-qa`.

## How you work as a pair

Two people, one laptop. That's on purpose: one of you types, the other one thinks and checks.

* **The driver** types the prompt and sends the requests in Postman.
* **The navigator** reads the output with the brief next to it, and does the **Sanity check time** list. The navigator says "stop" when something looks off.
* **Swap after every prompt.** Everyone drives, everyone checks.
* **Talk before you accept.** Don't put anything in a file until you both agree on it. Where you disagree, that's usually where the interesting risk is.

## How to read an exercise

**Every prompt starts with the same three lines**, so you always know where you stand:

* **Read:** the files the prompt works with. These exist already, you don't make them.
* **Create:** the file you make or change, and where it lives. If it says *nothing*, no file changes in this step.
* **Done when:** what has to be true before you move on to the next prompt.

Then you'll find:

* the prompt itself, ready to send. Where it says `[paste ...]`, paste the real thing, in full
* a **CO-STAR check**: which parts of the model are in this prompt? Take a moment to find them
* **Sanity check time**: two to four things you check by hand. Don't skip them. If you skip the check, you're not testing the AI anymore, you're just trusting it
* sometimes a **Watch for** box: a mistake AI makes often at this point

Not sure what to do now? Look at Read, Create and Done when first. Still not sure? Ask your coach. It means the exercise isn't clear enough, not that you're doing it wrong.

**Which tool?** The exercises are written for an AI chat tool and Postman (or a browser). Use GitHub Copilot Chat or Claude.ai, whichever works on your laptop right now. Want to use something else? Go ahead, as long as AI does the work. The cheat sheets in `ai-cheat-sheets/` show you how the main tools work. New to Postman? Ask your partner or your coach for a two-minute tour. That's what pairs are for.

**One chat per prompt.** Start a new chat for each prompt. Long chats drift: an instruction from twenty messages ago has lost most of its weight.

**Answer too long to check?** Don't start reading. Send the prompt again in a new chat, with one extra line at the end: "At most 10 points, the most important first." Or ask in the same chat: "Give me only the 5 that matter most for StackCraft, and why." The prompts below are a starting point, not a script: adding a limit is always allowed, and usually a good idea.

## The files of the day

Over the day your working folder collects a few files that you make yourselves. You'll meet them in this order:

| File | Made by | In | What it is for |
|---|---|---|---|
| `test-plan.md` | Pair AB | Block 1 | Your risk priorities and your test plan |
| `test-cases.md` | Pair AB | Block 1 | The test case catalogue, with the real results next to it |
| `charter.md` | Pair CD | Block 1 | The charter for your exploratory session |
| `session-notes.md` | Pair CD | Block 1 | What you tried, what you expected, what happened |
| `bugs.md` | Both pairs | Block 1 | One shared list of every genuine defect |
| `instructions.md` | Both pairs | Block 1 | Your house rules. The AI reads them before every request |
| `prompt-log.md` | Every pair | Block 2 | Your prompts, with a one-line verdict each |
| `decisions.md` | Every pair | Block 2 | What you decided and what you learned |
| `pre-release-quality-report.md` | Manual track | Block 3 | The report with a release recommendation |
| the skill folder, with `SKILL.md` | Every pair | Block 3 | The skill you build and swap |

Anything that is not in this table already exists in the course files.

## Your block at a glance

A rough guide, not a rule.

| Part | Pair AB | Pair CD | About |
|---|---|---|---|
| Setting up | both | both | 5 min |
| The work | Prompts 1 to 5: risks, test plan, catalogue, run it, interrogate it | Prompts 1 to 5: charter, a 25-minute session, the inherited suite, bug reports, interrogate it | 55 min |
| House rules | together | together | The last 15 min. Don't skip this one, Block 2 needs it |

Running behind? Don't skip Prompt 5, it's the most important prompt of the block for both pairs. Pair AB can shorten Prompt 2. Pair CD can shorten Prompt 3.

---

## Setting up (both pairs, about 5 minutes)

* **Read:** `retroflow-qa-brief.md` and `retroflow-api-reference.md`
* **Create:** the shared `retroflow-qa` folder, if nobody in your group made it yet, and a Postman collection
* **Done when:** your pair sent one request to your group's URL and saw a real response, and you both read the brief yourselves

* In Postman, create a collection "RetroFlow" with a variable `baseUrl` set to your group's URL. Then every request can start with `{{baseUrl}}`.
* Send one request to check that it works: `POST {{baseUrl}}/api/teams` with a team name that contains your initials and at least one member (the exact body is in `retroflow-api-reference.md`). Look at the status code and the body.
* **Read the brief yourselves.** Don't use the AI for this part. The first prompt uses AI to understand something you've just read, so you can judge what it says.

---

## Pair AB: risk-based test plan and test case catalogue

You are building the test strategy for RetroFlow: what to test, in what order, and why. And then you find out how the real app compares to the plan.

---

### Prompt 1: Understand before you test

* **Read:** `retroflow-qa-brief.md`
* **Create:** `test-plan.md` in the root of the working folder (new file)
* **Done when:** you and your partner agreed on a risk priority order, and wrote it down

Open a new chat. Paste the full contents of the brief and send:

```
I'm a test engineer taking over QA for a Spring Boot backend called RetroFlow, ahead of its
first release. Here is the handover brief:

[paste brief here]

Explain the domain and the business rules to me as if I'm a new tester joining the team.
Then identify which of these business rules carries the highest risk if it fails silently
in production, and why. Be specific about the business consequence, not just "this could
be a bug."
```

**CO-STAR check:** there's a Context (the project and the brief), an Objective (explain, then rank the risks), an Audience (a new tester) and a bit of Response ("be specific about the business consequence"). What's missing? The **Rules** and **Evidence** lines.
Try it: add *"Quote the sentence of the brief for every rule you mention."* and run it again in a **new** chat. Which version is easier to check?

#### Sanity check time

- Do you agree with the risk ranking? What would you rank differently, and why?
- AI tends to rank on generic API intuition. Check it against what the CTO says in the brief: what does StackCraft actually worry about?
- Write your agreed priorities in `test-plan.md`. You'll refer back to them all day.

> **Watch for:** a ranking that sounds reasonable for any API, but ignores what this brief says matters. The ranking is yours, not the AI's.

---

### Prompt 2: Risk-based test plan

* **Read:** the brief and your `test-plan.md`
* **Create:** the plan itself, added to `test-plan.md`
* **Done when:** the plan matches your own judgement and not just the AI's, and concurrency has a clear place in the priority order

```
Based on this domain brief, propose a risk-based test plan for RetroFlow covering:
- Test levels to use (API-level, integration, exploratory) and why
- Entry and exit criteria for this QA pass
- A risk matrix: for each business rule, rate likelihood of a defect and impact if it ships,
  and use that to set testing priority order
- Environments and test data needed

Explain your reasoning for the priority order, not just the ranking itself.
```

**CO-STAR check:** an Objective with a detailed **Response** (four parts), and a **Style** line at the end ("explain your reasoning"). That last line is the **ask for the reasoning** technique: when you see why it ranked something, you can disagree with the why, not just the order.

#### Sanity check time

- Compare the plan with your own order from Prompt 1. Where does it differ, and who's right?
- Does concurrency (two people acting on the same retrospective at once) rank high enough, for a tool that is collaborative by design?
- Don't copy blindly. Adapt the plan to your team's judgement, then save it.

> **Watch for:** a plan of three pages that you don't have time to read. Add a limit and run it again: "Keep it to one page. At most 7 rules in the risk matrix, the riskiest first." A plan you checked beats a plan that's complete.

---

### Prompt 3: Test case catalogue for core flows

* **Read:** the brief, `test-plan.md` and `retroflow-api-reference.md`
* **Create:** `test-cases.md` in the root of the working folder (new file)
* **Done when:** the catalogue covers all four core rules, with three separate cases for the closed-retrospective rule

```
Write a structured test case catalogue for the following RetroFlow flows. For each test case,
include: ID, preconditions, steps, expected result, and which business rule it verifies.

Flows to cover:
- Creating a team (including the "at least one member" rule)
- Creating a retrospective (including the "only one OPEN at a time" rule)
- Closing a retrospective (including the "cannot be reopened" rule)
- Adding a feedback item to an OPEN retrospective
- Adding, modifying, or deleting a feedback item in a CLOSED retrospective

Include both positive and negative cases for each flow.
```

**CO-STAR check:** an **Objective** and a precise **Response** (the fields per test case). What's missing is an **exit**. Where the brief doesn't say what the response should be, the AI will make one up. Try adding *"Where the brief doesn't say what the expected response is, write 'unspecified' instead of guessing."*

#### Sanity check time

- Does the catalogue cover all three operations of the closed-retrospective rule: add, modify AND delete? That's three separate test cases, not one.
- Look at the expected status codes. Where did the AI get them from? If the brief doesn't say, they are predictions, not facts. Mark them as assumptions.
- Add a column "Actual result" to `test-cases.md`. You fill it in during the next prompt.

---

### Prompt 4: Manually exercise the API

* **Read:** `test-cases.md`, `retroflow-api-reference.md` and your group's URL
* **Create:** the real results in `test-cases.md`, and the first entries in `bugs.md` (new file)
* **Done when:** at least 3 test cases ran against the live app, with the real status code and body written down

No test code today, just Postman (or a browser, or curl if you're comfortable with it) and your own hands.

```
Here is my test case catalogue: [paste it]. For each test case, tell me exactly what request
to send (method, URL, headers if any, and body if any) so I can enter it into Postman by hand.
Present it as a simple table: test case ID, request, and what response you'd expect.
```

**CO-STAR check:** a **Context** (your own catalogue, pasted), an **Objective** and a **Response** ("a simple table"). Notice that the AI only gives you a *prediction* of the response here. The result comes from Postman.

#### Sanity check time

- Build each request in Postman one at a time (or reuse one your partner already made) and actually send it to the running app. Check the request against `retroflow-api-reference.md` before you send it: did the AI invent a path or a field?
- Write the real status code and body next to the prediction in `test-cases.md`. Don't assume they match. Look.
- A case you didn't run is not a pass. Mark it "not run".
- An endpoint returns a raw stack trace, or a status code you didn't expect? Don't fix it or explain it away yet. Log it in `bugs.md`.
- Are the status codes correct by REST conventions (201 for creating, 404 for not found)? Check a few yourselves, don't just trust the AI's claim.

---

### Prompt 5: Interrogate the output

* **Read:** `test-cases.md` with your results, and the brief
* **Create:** the missing test cases, added to `test-cases.md`
* **Done when:** for every one of the four rules you know if it has coverage, and what that test would not catch

This is the most important prompt of the block. Don't skip it.

```
Review the test case catalogue and the actual results I recorded against these specific
business rules from the brief. For each rule, tell me:
1. Whether it currently has explicit test coverage in the catalogue
2. Where exactly (which test case)
3. What kind of defect this test case would and would NOT catch

Business rules to check:
- A team can only have one OPEN retrospective at a time
- A closed retrospective cannot be reopened
- Feedback items in a closed retrospective cannot be modified or deleted
- Completed action items cannot be uncompleted
```

**CO-STAR check:** the Objective is a *review*, and the Response is a fixed structure of three answers per rule. "Where exactly (which test case)" is **Evidence**: you can look it up.
Notice what you did: you asked the AI to check the work. That's step 4 of the checking order. It only works if you know enough to judge the answer, so the navigator keeps the brief and the recorded results open next to it.

#### Sanity check time

- This is your quality gate. Anything with "no coverage" gets a test case before the block ends.
- Look hard at point 3, "what this test would NOT catch". A test case that checks the happy path of a rule, but not a concurrent or deliberate attempt to break it, is incomplete, even if it exists.
- Did the AI call a rule "covered" because a case with a matching name exists? Check the recorded result. A case that exists is not a case that ran and passed.

---

## Pair CD: exploratory testing and bug reporting

You are setting up the exploratory side of QA: a testing charter, a hands-on session against the running application, an honest read of the tests you inherited, and bug reports that would survive scrutiny.

---

### Prompt 1: Build an exploratory testing charter

* **Read:** `retroflow-qa-brief.md`
* **Create:** `charter.md` in the root of the working folder (new file)
* **Done when:** the charter points at the highest-risk areas, including the parts of the brief that are underspecified

Open a new chat. Paste the full brief and send:

```
I'm a test engineer preparing an exploratory testing session for RetroFlow, a Spring Boot
backend for running team retrospectives. Here is the handover brief:

[paste brief here]

Generate an exploratory testing charter covering:
- The main happy-path flows to explore first
- Likely edge cases given the business rules (timing boundaries, permission-adjacent
  behaviour, concurrent actions, invalid or missing input)
- Anything in this brief that looks underspecified or worth probing deliberately

Keep it as a focused, time-boxed session plan, not an exhaustive checklist.
```

**CO-STAR check:** a **Context** (you, the session, the brief), an **Objective** (a charter) and a **Response** with three parts and a size limit ("focused, not exhaustive"). What would make it sharper? A **Rule** like *"Aim it at the highest risk, and give at least two deliberate attempts to break a rule."*

#### Sanity check time

- Does the charter really point you at the highest-risk areas, or just at the obvious ones? Adjust it with your partner if it doesn't.
- The "underspecified" section matters most. That's usually where a real bug is hiding.
- A good charter has a risk in it. "Test the API" isn't a charter.

---

### Prompt 2: Run the session

* **Read:** `charter.md`, `retroflow-api-reference.md` and your group's URL
* **Create:** `session-notes.md` in the root of the working folder (new file)
* **Done when:** you tested for at least 25 minutes, with real notes in the format below, including the race attempt and the three separate operations on a closed retrospective

This one isn't a prompt. Set a timer for 25 minutes. Using Postman (point-and-click, no scripting) or a browser, work through your charter against the running app. Keep running notes in `session-notes.md`, in this format, for anything notable:

```
What I tried:
What I expected:
What actually happened:
Worth a closer look? Y/N
```

What to do during the session:

- **Race the "only one OPEN retrospective" rule.** Create a retrospective. Then you and your partner both try to create a second one for the same team, within a second or two of each other: each has the request ready in Postman, and you send on "go". This is a manual approximation of a race condition, and it's a real testing technique, not a compromise for not writing code. Try it more than once: a race doesn't always show up the first time.
- **Try the three operations on a closed retrospective one by one:** add, modify and delete a feedback item. Don't assume they behave the same because they're related.
- **Follow anything that surprises you.** That's the point of exploratory testing.

#### Sanity check time

- Found something odd? Reproduce it a second time before you move on. A bug you can't reproduce twice is weak evidence.
- Did you write down the attempts where nothing went wrong too? That's evidence as well.

---

### Prompt 3: Get a plain-language read on the inherited test suite

* **Read:** `RetroflowApiTest.java` (to read, not to run) and your `session-notes.md`
* **Create:** a short section "Inherited test suite" at the bottom of `session-notes.md`
* **Done when:** you can say in plain language which tests would catch a real regression and which would pass whatever the API returned

You don't need to read Java to do this. Let the AI translate it, then you make the call.

```
Here is an existing automated test suite for a Spring Boot project:

[paste the contents of RetroflowApiTest.java, the test suite the developers left behind]

For each test, explain in plain English, no code, what it actually checks, and rate whether
that check would catch a real regression or would pass no matter what the API returned. Flag
any test that only checks "not null" or a status code when the endpoint's whole purpose is to
return specific data.
```

**CO-STAR check:** an **Audience** that doesn't read code ("plain English, no code"), a **Response** per test (explain, then rate) and a **Rule** that tells it what to flag. The flag line is what turns a summary into a review.

#### Sanity check time

- You're judging the plain-language explanation, not the code. Does each described check sound like it would catch something going wrong?
- Cross-check a couple of the "weak" flags against what you saw yourselves in the session. Do they line up?
- A test that checks only "not null" is green whatever the code does. Keep that in mind for Retro 1: the inherited suite passes, but does it prove anything?

---

### Prompt 4: Write up what you found

* **Read:** `session-notes.md`
* **Create:** the reports, in `bugs.md`. Pair AB started that file, so add to theirs
* **Done when:** every genuine issue has a report with severity, steps to reproduce, expected result, actual result and business impact, in one shared list

```
Here are my raw session notes from exploratory testing: [paste session-notes.md]. Write these
up as clear bug reports, one per genuine issue, with a title, severity (Critical / High /
Medium / Low), steps to reproduce, expected result, actual result, and business impact. Keep it
factual, don't guess at the root cause, and don't invent an issue that isn't in my notes.
```

**CO-STAR check:** a **Context** (your own notes), a precise **Response** (the fields of a report) and strong **Rules** ("factual", "don't guess", "don't invent"). Those rules are there because a model writing a bug report likes to add a plausible root cause.

#### Sanity check time

- Reproduce **every** report once more in Postman, from the steps as written. A step the AI invented shows up now.
- Is the severity argued from the business impact? "High" for a missing field is probably inflated.
- Did it stay factual, or did it speculate about the cause? Compare each report with your own notes.
- Merge it into the same `bugs.md` as Pair AB. One list for the whole team.

---

### Prompt 5: Interrogate the output

* **Read:** the brief, `session-notes.md` and `bugs.md`
* **Create:** nothing new. Extra findings go into `session-notes.md` and `bugs.md`
* **Done when:** you know which of the four rules you haven't tried to break yet, and you tried at least one of them

This is the most important prompt of the block. Don't skip it.

```
Based on the RetroFlow brief's business rules, what have I not tried yet in my exploratory
session that a thorough tester would? Be specific about which business rule each gap relates
to, not just "test more edge cases."

Business rules to check against:
- A team can only have one OPEN retrospective at a time
- A closed retrospective cannot be reopened
- Feedback items in a closed retrospective cannot be modified or deleted
- Completed action items cannot be uncompleted
```

**CO-STAR check:** the Objective is a *gap analysis* of your own work, and the **Rule** "be specific about which business rule" stops a generic answer. Without it, you get "test more edge cases", which you can't act on.

#### Sanity check time

- This is your quality gate. Anything flagged as untried: try it now if time allows, or carry it forward into Block 2 and write that down.
- Paste your session notes too, if you didn't. An AI can only say what you haven't tried if it knows what you did try.

---

## The last 15 minutes, with your whole team: write your house rules

* **Read:** the prompts of this block that worked best, from both pairs, and your `bugs.md`
* **Create:** `instructions.md` in the root of the working folder (new file), and a copy in the place where your AI tool reads it
* **Done when:** the file is saved in the shared folder, it has the three required rules, and you saw it change an answer

By now you've probably typed "paste the real response", "label what you assumed", "check the status code and the body" or "at most 10 points" more than once. That's annoying, and it's easy to forget one. So you write them down once, in a place where the AI reads them before **every** request. That's an **instructions file**.

**What goes in:** your role and the source of truth, how a test case and a bug report look, what makes a check good, and what the AI must never do.
**What stays out:** things any model knows ("be thorough"), long explanations, and rules for one single task.
Write rules you can **check**. "Write good tests" is a wish. "Every check names the exact status code and at least one field in the body" is a rule.

Let the AI draft it from your own prompts:

```
Based on the prompts we used this morning [paste 2 or 3 of your best prompts], write a
short set of standing instructions for an AI assistant helping us test RetroFlow. Cover:
our role, what the source of truth is, how it must treat a predicted response versus a real
one, the format of a test case and of a bug report, what makes an assertion good, how long
its answers may be, and what it must never do. Keep it under 20 lines.
```

Then **edit it yourselves**, as a team of four. Keep it to 10 to 20 lines. It must contain at least these three rules, in your own words:

1. The AI never presents a predicted response as a fact. It writes what it expects, and waits for the real status code and body.
2. Every claim points at a source: a sentence from the brief, or a response you pasted. Anything else it assumed gets labelled as an assumption.
3. Answers stay short: at most 10 points, the most important first. More only when you ask for it. A list of 40 findings costs you the block, because you have to check every one.

Put a copy where your tool reads it:

| Tool | Where |
|---|---|
| GitHub Copilot | `.github/copilot-instructions.md` in the working folder |
| Claude Code | `CLAUDE.md` in the root of the working folder |
| Claude.ai | The instructions of a Project |
| Microsoft 365 Copilot | Settings, Personalization, Custom instructions |

In Block 2 the agent reads this file automatically, if it's in the right place.

### Sanity check time

- Start a **new** chat and run one of your earlier prompts again, but without the extra rules you typed in. Does the answer still follow them?
- Ask: "Write a test case for creating a retrospective." Does it name an exact status code and a field in the body, and does it mark the status code as an assumption if the brief doesn't give it?
- Ask the chat: "Which instructions do you have?" Can it list them?
- Ask for something that invites a long list, like "What could go wrong with RetroFlow?" Do you get at most 10 points, the most important first?

> **Watch for:** an instructions file makes the right behaviour *more likely*, not certain. You still check. Want to know what each tool supports exactly? `ai-cheat-sheets/working-efficiently.md`.

---

## Finished early? Get a second opinion

Take your catalogue prompt (Prompt 3) or your charter prompt (Prompt 1) and run it in a **different model**, in a fresh chat: another model in the Copilot model picker, Claude.ai, or another tool you have.

- Which model had more cases that matter, and fewer that don't?
- Which one marked what the brief doesn't say, and which one guessed?

Don't pick a winner for the whole world. Pick one for this kind of task, and write it down in two lines. You can share it in Retro 1.

---

## End of Block 1 checklist

This is also what we look at in Retro 1. Your coach asks one pair to show their result on the shared screen. Before Retro 1, your team should be able to answer yes to:

**Pair AB:**
- [ ] `test-plan.md` exists, with a risk priority order you agreed on
- [ ] `test-cases.md` covers all four core rules, including all three operations of the closed-retrospective rule
- [ ] At least 3 test cases ran against the live app, with real results recorded
- [ ] You ran Prompt 5 and know which rules have no coverage yet

**Pair CD:**
- [ ] `charter.md` exists and was actually followed, not just written
- [ ] `session-notes.md` has real findings from at least 25 minutes of hands-on testing
- [ ] The weak assertions and gaps of the inherited suite are written down in plain language
- [ ] You ran Prompt 5 and know which rules you haven't tried to break yet

**Both pairs together:**
- [ ] `bugs.md` is one shared list, with both pairs' findings, and every report was reproduced
- [ ] `instructions.md` exists, has the three required rules, and you tried it in a new chat
- [ ] Everyone knows the actual behaviour of the app on a handful of endpoints, not just what the brief says it should do

## Before Retro 1

Think about these with your partner, so you have an answer ready:

1. What did AI get right that surprised you?
2. What did it get wrong, guess or invent? How did you find out?
3. Did you change how you prompt during this block? What changed?
4. Did you find a tool, a setting or a trick the others should know about?

## Conclusion

You planned, designed and explored RetroFlow with AI, and more importantly, you checked what it told you against the real app.

You've learned:

- how to let an AI turn a brief into risks and a plan, and why the ranking stays yours
- how to build a prompt from CO-STAR parts, and that the Rules line carries most of the quality
- that a predicted response is not a result, and a case you didn't run is not a pass
- how to run a timed exploratory session with a charter, including a race by hand
- how to write a bug report someone else can reproduce, and to reproduce it yourself first
- how to write your rules down once, instead of typing them forever

Next block, you choose your track: an agent that writes and runs tests, or a deeper dive by hand.
