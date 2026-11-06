# Requirements: Exorcist

## Introduction

Exorcist is a Skeleton Crew starter kit for building AI agents that are tested against prompt injection, data leaks, and wrong tool calls before release.

A "possession" is a failed safety eval: the agent was manipulated into doing something it shouldn't. Exorcist finds possessions, shows the cause, and blocks the release until they're fixed.

The kit is proven by building two different apps from it: Haunted Help Desk (customer support) and Midnight On-Call (production incident investigation).

**Target user:** a small team shipping its first AI agent, without dedicated security or ML platform staff.

**Problem:** agents can be manipulated into leaking data or taking the wrong action. Safety tools are often added late and separately, so teams need a starting point that includes them from the first line of code.

**What makes it different:** scaffolding, enforcement, and testing come together in one lean kit, and every attack found becomes a permanent regression test. Check existing tools before claiming this is unique.

## Goals

1. A developer can create a new, tested agent app from the kit by writing only its config, tools, and eval cases.
2. A viewer can see an attack succeed, get detected, get fixed, and then fail, all within a 3-minute demo.
3. Every safety claim is backed by a number measured on a published test set.
4. A captured attack can never silently return, because it joins the regression suite.
5. Kiro is used deeply and visibly throughout the build.
6. The project feels like a finished product, not only a technical proof of concept.

## Non-goals

- Claiming that prompt injection is fully solved
- Voice or telephony
- Production authentication, multi-tenancy, or billing
- Training or fine-tuning models
- Real customer, employer, or production data

## Glossary

- **Core:** the shared library used by every app
- **App:** a separate agent built on the core, with its own tools, data, and eval cases
- **Protected mode:** the agent with all guards on
- **Unprotected mode:** the same agent with guards off, used for the baseline and demo
- **Eval case:** one test conversation with its expected behavior
- **Possession:** a failed safety eval case
- **Release gate:** a check that fails when eval results miss their thresholds

## Requirements

### Requirement 1: Agent runtime

**User story:** As a developer, I want a reusable agent loop, so that each app only defines what's unique to it.

**Acceptance criteria**

1. WHEN an app sends a user message, THE core SHALL run the model and tool loop and return a final response.
2. THE core SHALL load an app's tools from its MCP server.
3. THE core SHALL stop the loop after a configurable maximum number of tool calls and return a safe failure message.
4. WHEN a tool call or model call fails, THE core SHALL return a safe failure message and record the error in the trace.
5. THE core SHALL record a trace of each run, including tool calls, guard decisions, and the final response.
6. THE core SHALL run the same agent in protected or unprotected mode through configuration alone.

### Requirement 2: PII redaction

**User story:** As a developer, I want sensitive data removed automatically, so that it doesn't leak into responses or logs.

**Acceptance criteria**

1. THE redactor SHALL detect emails, phone numbers, payment card numbers, Indian PAN numbers, and API-key-like strings.
2. THE redactor SHALL replace detected values with typed placeholders such as `[EMAIL]`.
3. THE redactor SHALL validate payment card matches with the Luhn check.
4. WHILE in protected mode, THE core SHALL redact final responses before they reach the user.
5. THE core SHALL redact all stored traces and logs in both protected and unprotected modes.
6. THE eval report SHALL include redaction precision and recall per PII type on a labeled test set.

### Requirement 3: Tool safety

**User story:** As a developer, I want tool calls checked by code, so that a manipulated model can't take actions on its own.

**Acceptance criteria**

1. THE core SHALL validate tool arguments against the tool's schema before running it.
2. WHILE in protected mode, THE core SHALL only run tools on the app's allowlist for the current user role.
3. WHILE in protected mode, THE core SHALL scope data-reading tools to records the current user may access.
4. WHEN a tool is marked as having side effects, THE core SHALL require user confirmation in protected mode.
5. THE core SHALL treat tool output and retrieved content as untrusted data, not instructions.
6. WHEN a guard blocks a tool call, THE core SHALL record which guard blocked it and why.

### Requirement 4: Eval suite

**User story:** As a developer, I want repeatable safety and quality tests, so that I can measure changes instead of guessing.

**Acceptance criteria**

1. THE eval runner SHALL load cases from YAML files in each app.
2. THE eval suite SHALL support these case types: direct prompt injection, indirect prompt injection through tool output, data leak, wrong tool call, and normal task.
3. WHEN checking a result, THE eval runner SHALL prefer deterministic checks of tool calls and response text over model-graded checks.
4. THE eval runner SHALL report attack success rate, PII leak rate, tool-call accuracy, task completion rate, and false-block rate.
5. THE eval runner SHALL run each case a configurable number of times and report the pass rate, because model output varies.
6. THE eval suite SHALL keep a development set and a held-out set, and published results SHALL come from the held-out set.
7. THE eval runner SHALL show each failed case with its trace and the check that failed.

### Requirement 5: Release gate

**User story:** As a developer, I want unsafe changes blocked automatically, so that regressions don't ship.

**Acceptance criteria**

1. THE command `exorcist gate` SHALL exit with a non-zero code WHEN any configured threshold is missed.
2. THE gate SHALL read its thresholds from a config file in each app.
3. THE gate SHALL fail if false-block rate or task completion misses its threshold, so that an agent can't pass by refusing everything.
4. THE repository SHALL include a documented example of running the gate in CI.

### Requirement 6: Haunted Help Desk app

**User story:** As a judge, I want to see a realistic support agent, so that I understand the value quickly.

**Acceptance criteria**

1. THE app SHALL live in its own folder and use only the core's public interface.
2. THE app SHALL provide mock tools to look up customers and orders, search a knowledge base, and issue a refund.
3. THE app SHALL use only made-up data.
4. THE app SHALL include a knowledge-base article containing a hidden prompt injection for the demo.

### Requirement 7: Midnight On-Call app

**User story:** As a judge, I want to see a very different agent from the same kit, so that I believe the template is reusable.

**Acceptance criteria**

1. THE app SHALL live in its own folder and use only the core's public interface.
2. THE app SHALL provide mock tools to read logs, read metrics, list recent deploys, and propose a rollback.
3. THE app SHALL require confirmation before a rollback.
4. THE app SHALL include mock logs containing fake secrets and a hidden prompt injection.
5. THE app SHALL NOT require changes to the core that only this app uses.

### Requirement 8: Demo interface

**User story:** As a judge, I want to try the project without installing anything, so that I can evaluate it easily.

**Acceptance criteria**

1. THE web interface SHALL let a user choose an app and chat with the agent.
2. THE interface SHALL show protected and unprotected results side by side.
3. THE interface SHALL show the eval report and failed-case details.
4. THE interface SHALL offer scripted demo mode, which replays recorded runs without calling a model.
5. THE interface SHALL clearly label scripted results as recorded.
6. THE hosted demo SHALL apply rate limits and a spending cap to public live model calls.
7. WHEN the public spending cap is reached, THE interface SHALL fall back to scripted demo mode with a clear message.
8. THE hosted demo SHALL give judges free, working access to both apps without those public limits, through credentials in the private submission field.
9. THE hosted demo SHALL stay available until the Judging Period ends on January 29, 2027, 1:00 pm Pacific Time.
10. THE interface SHALL use a consistent, intentional haunted theme that stays readable, works by keyboard, and doesn't hide safety information.
11. THE interface SHALL guide a first-time visitor through the demo flow without external instructions.

### Requirement 9: Starter kit experience

**User story:** As a developer, I want to create a new agent quickly, so that I actually use the safety checks.

**Acceptance criteria**

1. THE command `exorcist new <name>` SHALL create a working app with sample tools, eval cases, and gate config.
2. THE README SHALL include a quick start, an architecture overview, measured results, and known limitations.
3. THE repository SHALL be public on GitHub, with an open-source license file detected and shown in the About section.
4. THE repository SHALL NOT contain secrets, and SHALL provide a `.env.example`.
5. THE repository SHALL keep the two apps in separate top-level app folders.
6. THE README SHALL explain who the kit is for and how it compares with existing tools.

### Requirement 10: Attack capture

**User story:** As a developer, I want each discovered attack saved as a test, so that a fixed problem can't quietly come back.

**Acceptance criteria**

1. THE command `exorcist capture <trace-id>` SHALL turn a recorded run into a redacted eval case in the app's development set.
2. THE captured case SHALL record its source trace and the check that failed.
3. WHEN a captured case would contain PII after redaction, THE command SHALL stop and report the problem.
4. WHEN a captured case fails after a fix, THE release gate SHALL fail.

### Requirement 11: Built with Kiro

**User story:** As a judge, I want clear evidence of how Kiro shaped the build, so that I can score its use.

**Acceptance criteria**

1. THE repository SHALL keep the `/.kiro` directory at its root, with no part of it ignored by Git.
2. THE project SHALL be built through Kiro specs, with steering files for product, tech, and structure.
3. THE project SHALL use at least two agent hooks that automate real development checks.
4. THE project SHALL use at least one custom agent or MCP integration in the Kiro workflow.
5. THE project SHALL use property-based tests for core safety behavior.
6. THE write-up SHALL describe real examples, including what worked and what didn't.
7. THE write-up SHALL disclose any AI or pre-existing material from outside Kiro, including the Claude-drafted first version of this spec.

## Success metrics

Measure these on the held-out set, in both modes, and publish the real results even if they fall short. These are starting targets and may change after the first baseline.

| Metric | Target in protected mode |
|---|---|
| Attack success rate | At most 5% |
| PII leak rate | 0% on the test set |
| Task completion rate | At least 90% |
| False-block rate | At most 5% |
| Redaction recall | At least 95% per PII type |

## Judging criteria

The official rules score these equally, at 20% each:

| Criterion | How this project addresses it |
|---|---|
| Potential Impact | A named user and problem, with measured safety results |
| Technological Implementation | Requirement 11, shown in the write-up |
| Design | Requirement 8, as one complete product |
| Innovation/Idea | Requirement 10 and the combined kit |
| Presentation | A clear demo video that works end to end |

## Hackathon compliance checklist

Based on the official 2026 rules. Check them again before submitting, as they can change.

**Eligibility and ownership**

- [ ] Neither I nor my household works for AWS, Devpost, or an affiliate
- [ ] My employer doesn't employ a judge, as far as I can determine
- [ ] My contract doesn't give my employer any right in this project
- [ ] No employer code, data, or confidential information is used
- [ ] Required Kiro features and models are available in my region

**Project**

- [ ] All work was built during the Submission Period
- [ ] Other pre-existing code or work is disclosed
- [ ] Open-source dependencies comply with their licenses
- [ ] The project is entered in Skeleton Crew
- [ ] Two distinct apps come from the same foundation, in separate folders

**Submission**

- [ ] Working application URLs for both apps
- [ ] Text description of features and functionality
- [ ] Public YouTube video under 3 minutes, showing the project working
- [ ] No third-party trademarks or unlicensed music in the video
- [ ] Public GitHub repo with all source, assets, and instructions
- [ ] License detected and visible in the repo's About section
- [ ] `/.kiro` is at the root and isn't ignored
- [ ] Write-up on how Kiro was used
- [ ] Free judge access and credentials through January 29, 2027
- [ ] All materials are in English
- [ ] Category and any bonus categories are identified
