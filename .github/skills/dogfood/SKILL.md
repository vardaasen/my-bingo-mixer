---
name: dogfood
description: Playtest the app hands-on in the browser and deliver a brutally honest critique of the user experience. Use when the user asks to dogfood, playtest, "test the app like a user", review the UX, fun factor, or delight, or give critical feedback on the product experience.
user-invocable: true
argument-hint: 'Optional focus area (delight, onboarding, mobile) or just "start"'
---

# Dogfood the app

Act as a critical dogfooder, not a QA engineer and not a cheerleader. Actually use the app in the browser, form opinions about whether it is *delightful to use*, and report with evidence.

## Procedure

1. **Get the app running.** Reuse a running dev task or shared browser page if one exists. Otherwise follow the setup commands in [AGENTS.md](../../../AGENTS.md) to start the dev server, then open it in the integrated browser.

2. **Understand the intent before judging.** Read the content/data and docs (e.g. [questions.ts](../../../src/data/questions.ts), README) to learn what the app *claims* to be. Critique against that promise.

3. **Play the core loop end-to-end like a real user.** Click through: landing → start → primary interactions → completion/win. Do not substitute a code review for play — you must touch the app.

4. **Capture evidence.** Screenshot key moments (landing, mid-loop, win/celebration). If visuals don't match the code on disk, inspect computed styles and note the discrepancy — stale builds and cached CSS are real findings.

5. **Probe the edge cases real users hit.** Mis-taps (toggle off a marked item), mid-session reload (persistence), destructive navigation (back/reset without confirmation), and what happens *after* the win state.

6. **Cross-check behavior against the logic.** Read the state/logic layer (e.g. [useBingoGame.ts](../../../src/hooks/useBingoGame.ts), [bingoLogic.ts](../../../src/utils/bingoLogic.ts)) to confirm what you observed and surface hidden dead-ends the UI never reveals: locked states, unreachable branches, win conditions that stop firing, content pools too small to stay fresh.

7. **Write the report** using the format below. Report only — never start fixing findings in the same pass; the user decides what to act on.

## Report format

- **Delight score: N/10** — one honest number, no flattery. Score the whole experience: fun for a game, frictionless flow for a tool.
- ✅ **What's genuinely good** — keep it short; this earns you the right to be harsh later.
- 🔴 **Delight-killers, ranked by severity** — each with evidence (observed behavior + file/line link). Rank by impact on the experience, not effort to fix.
- 🐛 **Bugs** — broken or stuck behavior, kept distinct from design critique.
- 🎯 **Quick wins** — concrete, small changes that would raise the score.
- **Verdict** — one or two sentences: is this worth a user's time today?

## Quality bar

- Every claim has evidence: something observed in the browser, or a file/line reference. No vibes-only criticism.
- Critique the experience, not code style — unless the code causes an experience problem.
- Be honest to the point of discomfort. A dogfooding report that says "looks great!" is worthless.
