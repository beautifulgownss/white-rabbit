# Project Plan — *fork* (working name)

**Phase:** 1 of 6 · Planning
**Type:** Spare-time skill-expansion build (solo)
**Status:** Draft for review
**Last updated:** 2026-06-29

---

## 1. Vision

A bounded conversational tool that helps one person think through **one** hard life decision by reflecting their own reasoning back to them — separating the reasons they *give* from what actually *carries weight* — and then handing the decision back. It never tells them what to choose.

The point isn't an answer. It's clarity: the moment of *"huh, I hadn't seen it that way."*

## 2. Problem

Big personal forks — leave the job, end the relationship, move cities, take the offer — get stuck for predictable reasons. People loop on the same three arguments. They conflate the *defensible* reason with the *felt* reason. Friends advise instead of asking. Journaling doesn't ask the next question. There's no neutral, present space to externalize the reasoning and hear it back clean.

## 3. Goals & Non-Goals

**Goals**
- Learn the real skill stretch: multi-turn conversational state + reflection-prompt design (single-shot reflection is easy; a conversation that *goes somewhere* is the craft).
- Ship something usable in a weekend, used by you + a few testers.
- Make the reflection land — the mirror surfaces something the user hadn't articulated.

**Non-Goals** (these are the ethical guardrails, stated as scope — they are not "later," they are *never* for this product)
- ❌ Not a forever-companion or always-on chat.
- ❌ Does not advise, recommend, or pick a side.
- ❌ Not a therapy or crisis tool.
- ❌ No engagement-maximizing or retention mechanics. A good session *ends*.

## 4. Success Criteria

| # | Criterion | How we check |
|---|-----------|--------------|
| S1 | A full session completes: frame → interview → mirror → close | Manual walkthrough |
| S2 | You + ≥3 testers complete a real decision end to end | Test sessions |
| S3 | ≥half of testers say the mirror surfaced something new | 1-question post-session |
| S4 | The session feels bounded — it has a clear end | Observation / feedback |

## 5. Target User

An adult standing at a real, roughly binary life fork, mid-deliberation and mentally looping. They want clarity, not an answer handed to them. They'll give the tool ~10 minutes of honest attention.

## 6. Scope

**In (MVP)**
- One decision per session
- Frame the two paths and hold them neutrally on screen
- Adaptive interview — one question at a time, following what the user says
- Mirror: stated reasons vs. felt weight + the gap between them
- Bounded close that hands the decision back
- "Keep a copy" of what surfaced
- Single-file web app — no backend, no accounts, no stored data

**Out (later, or never)**
- Saved history across sessions, accounts/auth
- Multi-decision comparison
- Payments, sharing, native mobile
- Anything needing a server or database

## 7. Requirements (high level — firmed up in Phase 2)

**Functional**
- FR1 — Capture the decision in the user's own words.
- FR2 — Derive and display two paths, held without bias.
- FR3 — Conduct an adaptive interview, one question at a time, picking up the user's specific language.
- FR4 — Detect when enough has surfaced and transition to the mirror.
- FR5 — Reflect stated reasons vs. felt weight + the gap. No recommendation.
- FR6 — Close: a final framing that returns the decision to the user.
- FR7 — Export / copy the session takeaways.
- FR8 — Reset to start a new decision.

**Non-functional**
- NFR1 — Privacy: ephemeral session, no persistence, nothing stored beyond the live model call.
- NFR2 — Safety: detect crisis-adjacent content, step out of the "decision" frame, and gently surface support rather than role-playing through it. *(First-class requirement — a life-decision tool will eventually meet someone in a dark place.)*
- NFR3 — Tone: warm, brief, no therapy-speak, never validation-only.
- NFR4 — Latency: each turn responds within a few seconds, with a visible thinking state.
- NFR5 — Accessible: keyboard operable, visible focus, reduced-motion respected, readable contrast.
- NFR6 — Responsive down to mobile.

## 8. Technical Approach (MVP)

- **Single-file web app** driven by an LLM that returns structured JSON each turn (`{phase, message, pathA, pathB}`).
- All state held in memory; no database, no auth.
- Rationale: fastest path to a working, *testable* thing. Productization is a separate decision made later, not pre-built for now.

## 9. Constraints & Assumptions

- Spare-time effort, solo, no budget.
- Skill project **first**; "is there a company here" is a separate question deferred until after testers use it.
- Build environment specifics (fonts, storage limits, API access) get pinned in the Design phase.

## 10. Risks

| # | Risk | Mitigation |
|---|------|------------|
| R1 | Validation trap — easiest build just flatters the user | Make "stated vs. felt" the core mechanic, enforce in prompt design |
| R2 | "Did it help?" is hard to measure | Single post-session question (S3) |
| R3 | Scope creep toward forever-companion | Enforce the bounded-session non-goal |
| R4 | Crisis content in a vulnerable domain | NFR2 — detect and redirect, don't play through |
| R5 | Malformed JSON / latency from the model | Defensive parsing + retry, thinking state |

## 11. SDLC Roadmap

1. ✅ **Planning** — this document
2. ⬜ **Requirements** — turn FRs/NFRs into testable acceptance criteria (your home turf as an SDET)
3. ⬜ **Design** — UX flow, visual direction, and the conversation-engine contract (system prompt + JSON schema)
4. ⬜ **Implementation** — build the single-file app
5. ⬜ **Testing** — manual + optional automation; eval the conversation engine against scripted personas
6. ⬜ **Release** — share with testers, collect the one-question feedback

## 12. Decision Log

| # | Decision | Status |
|---|----------|--------|
| D1 | Name | **"white rabbit"** (decided 2026-06-29) |
| D2 | Where it lives (artifact vs. standalone web app) | **Build as Claude artifact now, port to standalone later** (decided 2026-06-29) |
| D3 | Testers — how many, and who (aim for ≥3 with a real decision) | Open |
| D4 | Testing depth | **Sessions now; build automated checks (Layer 2) only if it graduates past a fun build** (decided 2026-06-29) |
