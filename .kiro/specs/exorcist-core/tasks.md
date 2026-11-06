# Implementation Plan: Exorcist

Submission deadline: December 4, 2026, 2:00 pm Pacific Time (December 5, 3:30 am IST). No extensions or late entries.

Build through Kiro, keep notes for the write-up throughout, and don't start new work until task 0.1 is clear.

## Week 0: Setup (by November 12)

- [x] 0.1 Check employment contract and employer connections against the compliance checklist
- [x] 0.2 Register on Devpost
- [x] 0.3 Request Kiro credits now (form closes December 1, 12:00 pm PT, and review can take 48 business hours)
- [x] 0.4 Redeem credits by December 4, and confirm needed Kiro features and models work in your region
- [-] 0.5 Create a public GitHub repo, confirm the license appears in About, and add `.gitignore` and `.env.example`
  - _Requirements: 9.3, 9.4, 11.1_
- [~] 0.6 Set up `uv`, `pytest`, Hypothesis, and the package layout
- [~] 0.7 Add a CI workflow (pushing it needs the GitHub `workflow` permission or the website editor)
- [~] 0.8 Add Kiro steering files for product, tech, and structure
  - _Requirements: 11.2_
- [~] 0.9 Add hooks to run related tests and validate eval YAML
  - _Requirements: 11.3_

## Week 1: Core (November 13–19)

- [~] 1.1 Choose the model provider and hosting
- [~] 1.2 Define the model provider interface and a scripted fake model
  - _Requirements: 1.1_
- [~] 1.3 Build the agent loop with a tool-call limit and safe failures
  - _Requirements: 1.1, 1.3, 1.4_
- [~] 1.4 Load tools from an MCP server
  - _Requirements: 1.2_
- [~] 1.5 Build redaction, with unit and property-based tests
  - _Requirements: 2.1, 2.2, 2.3, 11.5_
- [~] 1.6 Save redacted traces in both modes
  - _Requirements: 1.5, 2.5_
- [~] 1.7 Add schema, allowlist, scope, and confirmation guards, with property-based tests
  - _Requirements: 3.1, 3.2, 3.3, 3.4, 3.6, 11.5_
- [~] 1.8 Mark tool output as untrusted, and add the mode switch
  - _Requirements: 1.6, 2.4, 3.5_
- [~] 1.9 **Checkpoint:** one agent runs a tool call in both modes using the fake model

## Week 2: Evals and first app (November 20–26)

- [~] 2.1 Build Haunted Help Desk tools, mock data, and config
  - _Requirements: 6.1, 6.2, 6.3_
- [~] 2.2 Create the poisoned knowledge-base article, and confirm the attack works on the unprotected agent
  - _Requirements: 6.4_
- [~] 2.3 Build eval case loading, checks, and repeat runs
  - _Requirements: 4.1, 4.2, 4.3, 4.5_
- [~] 2.4 Build scoring, the failed-case report, and redaction metrics
  - _Requirements: 2.6, 4.4, 4.7_
- [~] 2.5 Build the red-team custom agent or report MCP server, and review what it produces
  - _Requirements: 11.4_
- [~] 2.6 Write development and held-out cases
  - _Requirements: 4.6_
- [~] 2.7 Run the first baseline in both modes, then set thresholds
- [~] 2.8 Build `exorcist gate`, including the false-block check
  - _Requirements: 5.1, 5.2, 5.3_
- [~] 2.9 Build `exorcist capture`
  - _Requirements: 10.1, 10.2, 10.3, 10.4_
- [~] 2.10 Compare at least three existing tools, then update the positioning
  - _Requirements: 9.6_
- [~] 2.11 **Checkpoint:** an attack is found, captured, and fixed, and the gate goes from fail to pass

## Week 3: Second app and product (November 27–December 2)

- [~] 3.1 Build `exorcist new`, and test it from a clean folder
  - _Requirements: 9.1_
- [~] 3.2 Build Midnight On-Call from the kit in its own folder, and record any core changes needed
  - _Requirements: 7.1, 7.2, 7.3, 7.4, 7.5, 9.5_
- [~] 3.3 Build the guided page, web chat, and side-by-side view with the haunted theme
  - _Requirements: 8.1, 8.2, 8.10, 8.11_
- [~] 3.4 Add the eval report page
  - _Requirements: 8.3_
- [~] 3.5 Record real runs, and add labeled scripted mode
  - _Requirements: 8.4, 8.5_
- [~] 3.6 Add public limits, the spending cap, fallback, and separate judge access
  - _Requirements: 8.6, 8.7, 8.8_
- [~] 3.7 Deploy both apps, set billing alerts, and test from a private browser window
  - _Requirements: 8.8, 8.9_
- [~] 3.8 Have one person try it without help, then fix what confuses them
- [~] 3.9 **Checkpoint:** the full demo works on deployed links, including the judge flow

## Final days: Submission (December 3–4)

- [~] 4.1 Run held-out evals, and put the real results in the README
  - _Requirements: 9.2_
- [~] 4.2 Document CI gate usage, known limitations, and tool comparison
  - _Requirements: 5.4, 9.2, 9.6_
- [~] 4.3 Finish the Kiro write-up with real examples and disclosures
  - _Requirements: 11.6, 11.7_
- [~] 4.4 Record the video, and publish it on YouTube
- [~] 4.5 Scan the repo and its history for secrets
  - _Requirements: 9.4_
- [~] 4.6 Complete the compliance checklist in `requirements.md`
- [~] 4.7 Fill in the Devpost form as a draft, including judge credentials
- [~] 4.8 **Submit by December 3**, leaving time to fix problems

## After submitting

- [~] 5.1 Keep the app free and working until January 29, 2027, 1:00 pm PT, and test it in early January
- [~] 5.2 Watch for winner messages around February 3, 2027, as required forms are due within 10 business days

## If time runs short

Cut in this order:

1. CLI or Web comparison, and any Kiro feature that isn't helping
2. `exorcist new` generator, replacing it with a documented copyable template
3. Some Midnight On-Call tools, keeping logs, deploys, and the rollback confirmation
4. The theme's extra details, keeping a clean and consistent interface

Don't cut the held-out results, the working attack, attack capture, the second app, judge access, the Kiro write-up, or the video.
