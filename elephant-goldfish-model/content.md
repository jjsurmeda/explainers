# Elephant-Goldfish Model — Explainer Content

Presenter edition · 14 slides · Sources checked 14 November 2026 (EGM repository at v0.1.1)

Each slide lists the on-screen copy and the source footer. This deck has no speech bubbles; the presenter appears on slides 1, 2, 13 and 14.
Color key used in the deck: **Elephant = slate blue**, **Goldfish = orange**, **You = ink**.

**Source key:** [1] Google Open Source Blog · [2] EGM README · [3] EGM architecture doc · [4] EGM getting-started guide · [5] EGM harnesses doc · [6] EGM skill files · [7] Spec Kit README · [8] OpenSpec docs · [9] All Things AI (Substack)

---

## Slide 1 — Title

**Eyebrow:** Explainer · Spec-Driven Development

**Headline:** The Elephant-Goldfish Model

**Body:**

Google's open-source plugin for designing with full context and reviewing with none. Where it fits in Spec-Driven Development, and how it compares with Spec Kit and OpenSpec.

**Pills:** Open-sourced 22 Oct 2026 | Apache 2.0 | Antigravity · Claude Code · Codex

**Presenter caption:** Julius Jefferson Surmeda, Senior Full Stack / AI Developer

**Footer:** Sources: [1] [2]

---

## Slide 2 — The problem

**Eyebrow:** The problem

**Headline:** AI scales your mistakes as fast as your code

**Body:**

**Slop at speed**

Agents produce code faster than a team can understand it. Bugs travel at the same pace.

**Context collapse**

Long sessions lose the thread. Decisions made in chat never reach the repository.

**Shared blind spots**

A reviewer who sat through the design conversation inherits its assumptions.

> “Design is the new code.”

**Footnote:** Dave Rensin, quoted by the EGM team on the Google Open Source Blog, October 2026

EGM's answer is to make the agent write a design document and defend it before any code exists.

**Footer:** Sources: [1]

---

## Slide 3 — Two roles

**Eyebrow:** The model

**Headline:** Two agents with opposite memories

**Body:**

**Elephant**

Remembers everything

- Sees: Your project, requirements and earlier decisions
- Job: Interview you, explore the problem, draft the specification
- Rule: One long session that writes no code

**Goldfish**

Remembers nothing

- Sees: Only the document and the source files it references
- Job: Find missing assumptions, unclear requirements and implementation risks
- Rule: A fresh subagent for every review

**You**

Make the architectural trade-offs, choose which findings to act on, and approve the specification before anything is implemented.

**Footer:** Sources: [1] [2]

---

## Slide 4 — Why it works

**Eyebrow:** The idea

**Headline:** Why a forgetful reviewer works

**Body:**

*Illustration · Elephant rereads its own draft*

Memory fills every gap in the text, so the gaps stay invisible.

*Illustration · Goldfish reads the same draft*

It reads the page literally. Anything that lived only in the conversation shows up as a question.

1. **The comprehension test** If a Goldfish cannot explain the design back, the document is ambiguous and goes back to drafting.
2. **Fix the document** Answer a finding in the doc, then run a new Goldfish. Explaining it in chat turns the reviewer into a second Elephant.
3. **The doc becomes the handoff** A spec that survives a reader with no context can be handed to a coding agent with no context.

**Footer:** Sources: [1] [9]

---

## Slide 5 — Lifecycle

**Eyebrow:** The plugin

**Headline:** The EGM lifecycle in six steps

**Body:**

*1 · Explore*

**Brainstorm**

The Elephant interviews you. Fresh reviewers attack the idea through seven lenses.

Output: /egm-brainstorm / → concepts_brief.md

*2 · Draft*

**Write the design**

Section by section, pausing for your feedback. Ends with an implementation plan.

Output: /egm-draft / → design_draft.md, / saved to docs/designs/

*3 · Review*

**Goldfish protocol**

Zero-context reviewers check simplicity, clarity, design, fit with the code and readiness.

Output: /egm-review / → findings report

*4 · Resolve*

**Triage findings**

You decide what to address. The doc is patched, then reviewed again if needed.

Output: /egm-resolve / ↺ back to review

*5 · Approve*

**Sign off**

Open a PR for peer review. The approved spec lives in the repository.

Output: pull request

*6 · Implement*

**Build**

Your coding agent implements the approved design.

Output: any coding agent

`/egm-bootstrap` creates or updates documentation for an existing codebase, so brownfield work starts with context.

`/egm-drift-report` compares checked-in docs with the current code. You choose whether the doc or the code changes.

`/egm-review-loop` runs reviewers and standards you pick, for documents that are not software designs.

**Footer:** Sources: [2] [4] [6]

---

## Slide 6 — Brainstorm

**Eyebrow:** The plugin · /egm-brainstorm

**Headline:** Brainstorm: attack the idea before writing it down

**Body:**

1. **Drill the problem** The Elephant asks open-ended questions over several turns. No multiple-choice shortcuts.
2. **Readiness gate** It sums up the working hypothesis in two or three sentences. Nothing is spawned until you confirm.
3. **Goldfish swarm** Reviewers run in parallel. Their findings are de-duplicated and the brief is written.
4. **Iterate** You get the file path and the two or three biggest risks, then refine until the concept holds.
5. **Hand off** Publish the brief as an RFC, move on to `/egm-draft`, keep refining, or stop and keep the brief.

*Seven lenses the swarm looks through*

**Contrarian / YAGNI**

Could an existing library or service solve most of this?

**Cost, quota and tokens**

What is the peak load and token budget? Will it hit quota ceilings?

**UX and developer ergonomics**

Is the API or CLI intuitive? Are the errors actionable?

**Migration and compatibility**

How do existing clients and data migrate, and how is rollback handled?

**Observability and debugging**

How will on-call triage an incident at 3 AM?

**Tech debt and maintenance**

Will this be painful to maintain in two years?

**Inventive Catalyst**

Which trade-off is being treated as inevitable, and can it be designed away?

Output: `concepts_brief.md`, the agreed direction that `/egm-draft` turns into a design doc.

**Footnote:** Questions condensed from the lens table in the brainstorm skill.

**Footer:** Sources: [6]

---

## Slide 7 — Goldfish review

**Eyebrow:** The plugin

**Headline:** Inside the Goldfish review

**Body:**

*Stage 1*

**Zoom out**

A skeptical architect asks whether this is the simplest design, and whether existing code could be reused.

*Stage 2*

**Comprehension**

A naive reader gets the document alone and flags undefined terms and missing background.

*Stage 3*

**Critic**

A hostile design critic attacks architecture, scale and unmitigated risk. Structure and style reviewers run beside it.

*Stage 4*

**Zoom out again**

Re-audits whatever was added while answering stages 2 and 3, so fixes do not bloat the design.

*Stage 5*

**Implementation readiness**

A pragmatic tech lead checks file paths, signatures and tests, leaving no design decision to the implementer.

**Synthesizer**

Runs the reviewers in parallel, removes duplicate findings and writes one consolidated report. You then work through it with `/egm-resolve`.

**Yours to tune**

Reviewer personas and document standards are Markdown files in the repo. Teams can edit the rubrics to match their own rules.

**Footnote:** Stages run in order, with a checkpoint after each; reviewers inside a stage run in parallel. You can also run the feature-spec stages or the implementation stage alone. The skill numbers these stages 5 to 9, after four setup steps.

**Footer:** Sources: [3] [6]

---

## Slide 8 — What ships in the plugin

**Eyebrow:** The plugin

**Headline:** What ships in the box: four layers, seven skills

**Body:**

- **Skills** (`skills/`): The slash commands you run. Each is a modular capability.
- **Personas** (`agents/`): Ten prompt files: Zoom Out, Comprehension, Design Critic, Implementation Readiness, Codebase Details, Structural, Style, Inventive Catalyst, the Synthesizer, and a bootstrap orchestrator.
- **Document standards** (`resources/document_standards/`): Five versioned rubrics the reviewers load, including zoom-out, feature spec and implementation readiness.
- **Living docs** (`docs/`): User guides, plus approved design specs under `docs/designs/`.
Artifact-first. Skills do not dump long drafts or review logs into chat. Content goes to files or side-panel artifacts, and the skill points you there and waits.

| Skill | What it does |
|---|---|
| egm-brainstorm | Tests an idea with a seven-lens reviewer swarm and saves the brief |
| egm-draft | Compiles the design doc section by section |
| egm-review | Orchestrates the multi-stage Goldfish review |
| egm-resolve | Works through findings with you and patches the doc |
| egm-review-loop | Runs reviewers and standards you choose, for other documents |
| egm-bootstrap | Documents an existing codebase from the bottom up |
| egm-drift-report | Compares specs and READMEs with the code as it is now |

**Footer:** Sources: [2] [3]

---

## Slide 9 — Recommended workflow

**Eyebrow:** Using it for Spec-Driven Development

**Headline:** A recommended workflow, by who does what

**Body:**

| Lane | Bootstrap (existing repos) | Brainstorm (no code yet) | Draft (the spec) | Review (fresh eyes) | Resolve (loop until go) | Approve (human gate) | Implement (from the doc) | Drift check (weekly) |
|---|---|---|---|---|---|---|---|---|
| You | Run once so the codebase is documented | Bring the idea, answer the interview | Give feedback on each section |   | Triage every finding | PR, peer review, merge | Review the code | Decide: fix the doc or the code |
| Elephant |   | Probes trade-offs, writes the brief | Writes the design doc |   | Patches the doc |   |   |   |
| Goldfish |   | Seven-lens challenge of the idea |   | Staged review, one report |   |   |   |   |
| Coding agent |   |   |   |   |   |   | Builds from the approved doc, runs tests |   |
| Command | /egm-bootstrap | /egm-brainstorm | /egm-draft | /egm-review | /egm-resolve | docs/designs/ | your agent | /egm-drift-report |

**Build to the doc.** Google's guide: if the output is far off, update the design doc and retry. A fresh session is a useful extra test that the doc stands alone.

**Filter, then loop.** Expect a lot of findings and choose which matter. Repeat until the readiness stage says go, then open a PR for a human reviewer.

**Keep specs in the repo.** Versioned designs give the drift report something to measure the code against.

**Footer:** Sources: [4] [6]

---

## Slide 10 — Existing codebases and drift

**Eyebrow:** Using it for Spec-Driven Development

**Headline:** Existing codebases, and keeping the spec true

**Body:**

*Before the first feature*

**Bootstrap**

Gives the Elephant something to remember on a repo that has no docs. It writes and verifies READMEs from the bottom up.

Deepest directories first → Parents roll up their children → Root README written last

It writes documentation only, makes no commits, and re-reads every file it wrote to check it against the code. A person should still review the result.

`/egm-bootstrap <path/to/project>`

*After the code ships*

**Drift report**

Specs rot when code moves on without them. The report scans the design folder and the code each doc references.

Compare each doc with current source → List every place they diverge → You decide: update the doc or fix the code

It looks for missing features, incomplete data models, logic shortcuts and architectural divergence, and writes a dated `DRIFT_REPORT.md`. Google's guide suggests a weekly run.

`/egm-drift-report docs/designs/`

**Why this pair matters**

A design-first workflow only holds if the designs stay trustworthy.

Bootstrap creates the first layer of written context. Drift reporting stops that context from going stale, so the next Elephant session starts from facts.

```text
docs/designs/
├─ SITEMAP.md   index
└─ FEATURE.md   design spec
```

**Footnote:** Layout recommended by Google's guide for where specs live. Both skills apply to software codebases only.

**Footer:** Sources: [4] [6]

---

## Slide 11 — Coverage

**Eyebrow:** Compare

**Headline:** What each framework covers

**Body:**

|   | Project rules | Explore | Specify | Spec review before build | Plan and tasks | Implement | Spec and code in sync |
|---|---|---|---|---|---|---|---|
| EGM | ◐ review rubrics | ● brainstorm | ● design doc | ● zero-context Goldfish | ◐ plan inside the doc | ○ hands off to your agent | ● drift report, bootstrap |
| Spec Kit | ● constitution | ◐ clarify | ● spec.md | ◐ analyze, checklists | ● plan.md, tasks.md | ● implement | ● converge |
| OpenSpec | ◐ config.yaml | ● explore | ● proposal, spec deltas | ◐ format validation, human review | ● design.md, tasks.md | ● apply | ● sync, archive, verify |

Key: ● core strength · ◐ partial · ○ out of scope

All three review the spec in some way. EGM is the one built around a reviewer that never saw the conversation.

**Footer:** Sources: [2] [7] [8]

---

## Slide 12 — Side by side

**Eyebrow:** Compare

**Headline:** Side by side

**Body:**

|   | EGM | Spec Kit | OpenSpec |
|---|---|---|---|
| From | Google engineers, Apache 2.0 | GitHub, MIT | Fission AI, MIT |
| Core idea | A context-rich author and reviewers with no context | A fixed pipeline from principles to code | Every change is a proposal with spec deltas |
| Flow | brainstorm → draft → review → resolve → implement | constitution → specify → plan → tasks → implement → converge | explore → propose → apply → sync → archive |
| Artifacts | concepts_brief.md, one design doc per feature | constitution.md, spec.md, plan.md, tasks.md | proposal.md, delta specs, design.md, tasks.md |
| Review | Adversarial, isolated subagents working from rubrics | Optional clarify, checklists and a cross-artifact consistency check | Human review of the change folder, plus format validation |

**Footer:** Sources: [2] [7] [8]

---

## Slide 13 — Getting started

**Eyebrow:** Try it

**Headline:** Getting started, and where it stops

**Body:**

*Install, then start a new session*

**Antigravity**

```text
agy plugin install https://github.com/google/elephant-goldfish-model
```

**Claude Code 2.1.271 or later**

```text
claude plugin marketplace add google/elephant-goldfish-model
claude plugin install egm@egm
claude plugin enable egm@egm
```

**Codex CLI**

```text
codex plugin marketplace add google/elephant-goldfish-model
codex plugin add egm@egm
```

*A first run*

```text
/egm-brainstorm Add webhook delivery to this project.
/egm-draft
/egm-review design_draft.md
```

**Footnote:** In Antigravity CLI and Claude Code, add the /egm: prefix, for example /egm:egm-brainstorm. In Codex, choose the skill from the skill picker.

**Limits**

- Brainstorm and review need a harness that can start fresh subagents. Without one, EGM stops the review.
- Antigravity CLI, Claude Code and Codex pass Google's end-to-end tests. Standalone Antigravity IDE is not supported.
- Isolation means no conversation history. On Codex and Antigravity, project files such as AGENTS.md are reported to still reach reviewers.
- Install the whole plugin. Copying the skills folder alone breaks it.
- A passed review does not replace tests or human judgment. For a simple bug fix, skip design-first.
- Not an officially supported Google product. “Thousands of Googlers” is Google's own figure.

**Footer:** Sources: [2] [5]

---

## Slide 14 — Takeaways

**Eyebrow:** Wrap-up

**Headline:** Three things to keep

**Body:**

1. **Write with context. Review without it.** The same memory that makes an agent a good author makes it a poor reviewer of its own work.
2. **The spec is the handoff.** If a Goldfish cannot build a mental model from the doc, a coding agent will not build the right code from it.
3. **Keep the spec alive.** Approved designs live in the repo, and a weekly drift report keeps them honest about the code.

**Footnote:** EGM details checked against the repository at v0.1.1, 14 Nov 2026. Sources: Google Open Source Blog · google/elephant-goldfish-model · EGM architecture doc · EGM getting started · EGM harnesses · github/spec-kit · Fission-AI/OpenSpec · openspec.dev · All Things AI (slide 4, point 2)

**Presenter caption:** Julius Jefferson Surmeda, Senior Full Stack / AI Developer

**Sources checked 14 November 2026:**

- [1] [Introducing the Elephant-Goldfish Model plugin · Google Open Source Blog](https://opensource.googleblog.com/2026/10/introducing-elephant-goldfish-model-plugin.html)
- [2] [google/elephant-goldfish-model · README](https://github.com/google/elephant-goldfish-model)
- [3] [EGM architecture doc](https://github.com/google/elephant-goldfish-model/blob/main/docs/designs/architecture.md)
- [4] [EGM getting-started guide](https://github.com/google/elephant-goldfish-model/blob/main/docs/getting_started.md)
- [5] [EGM harnesses doc](https://github.com/google/elephant-goldfish-model/blob/main/docs/harnesses.md)
- [6] [EGM skill files](https://github.com/google/elephant-goldfish-model/tree/main/skills)
- [7] [github/spec-kit · README](https://github.com/github/spec-kit)
- [8] [OpenSpec docs](https://openspec.dev/docs/getting-started) · [Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec)
- [9] [Spec-Driven Development · All Things AI](https://theaiparadigm.substack.com/p/spec-driven-development) (slide 4, point 2 only)

