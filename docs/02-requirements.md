# Requirements — *fork*

**Phase:** 2 of 6 · Requirements
**Status:** Draft for review
**Last updated:** 2026-06-29
**Convention:** Each criterion is written to be verifiable — pass/fail, no judgment call. Several are tagged 🤖 where they're natural candidates for automated evals later (ties to the deferred testing-depth decision).

---

## A. Framing — *name the fork* (FR1, FR2)

| ID | Acceptance criterion |
|----|----------------------|
| AC-F1 | On load, the user sees one prompt to name their decision and a single text input. Nothing else competes for attention. |
| AC-F2 | When the user submits a decision, the companion reflects it back in ≤1 sentence and names two paths (A and B). 🤖 |
| AC-F3 | Once named, both paths stay visible for the rest of the session, with neither visually emphasized over the other. |
| AC-F4 | If the decision isn't cleanly binary, the companion names the two dominant poles **and** acknowledges the nuance in text — it never silently forces a false binary. |

## B. Interview — *one question at a time* (FR3)

| ID | Acceptance criterion |
|----|----------------------|
| AC-I1 | Every companion turn during the interview contains **exactly one** question. 🤖 |
| AC-I2 | Each question references something specific the user said earlier — not a generic prompt. |
| AC-I3 | Companion turns are brief (≤ ~3 sentences) and contain no bulleted lists. 🤖 |
| AC-I4 | During the interview the companion never states a recommendation or a preferred path. 🤖 |
| AC-I5 | The companion never stacks two or more questions in one turn. 🤖 |

## C. Transition to mirror (FR4)

| ID | Acceptance criterion |
|----|----------------------|
| AC-T1 | The companion moves to the mirror on its own once the underlying values/fears have surfaced (target window: 5–8 exchanges — tentative, tuned in Design/Testing). |
| AC-T2 | The transition never fires on the first exchange. 🤖 |

## D. Mirror — *stated vs. felt* (FR5) — the core of the product

| ID | Acceptance criterion |
|----|----------------------|
| AC-M1 | The mirror explicitly names all three: (a) the reasons the user gave, (b) what seems to carry weight underneath, (c) the gap between them. |
| AC-M2 | The mirror contains **no** recommendation of which path to choose. 🤖 |
| AC-M3 | The mirror references the user's actual words/specifics, not generic categories. |
| AC-M4 | The UI visibly signals the shift into reflection (the light/register change). |

## E. Close — *hand it back* (FR6)

| ID | Acceptance criterion |
|----|----------------------|
| AC-C1 | The close frames the decision as the user's to make. |
| AC-C2 | The session reaches a clear end — no infinite question loop. 🤖 |
| AC-C3 | After close, the user can keep a copy and start over. |

## F. Keep / reset (FR7, FR8)

| ID | Acceptance criterion |
|----|----------------------|
| AC-X1 | "Keep a copy" outputs the decision, both paths, and the mirror text in copyable form. |
| AC-X2 | Reset returns the app to the initial prompt with no prior content visible. 🤖 |

## G. Safety (NFR2) — non-negotiable

| ID | Acceptance criterion |
|----|----------------------|
| AC-S1 | On crisis-adjacent content, the companion exits the decision frame immediately. 🤖 |
| AC-S2 | It responds with direct care and surfaces appropriate support, and does **not** continue interviewing as if normal. |
| AC-S3 | It never provides harmful detail and never runs a clinical risk-assessment script — it expresses concern plainly and offers resources. |

## H. Privacy (NFR1)

| ID | Acceptance criterion |
|----|----------------------|
| AC-P1 | No session content survives a page reload — nothing written to storage. 🤖 |
| AC-P2 | No accounts, no identifiers collected. |

## I. Conversation engine / contract (R5)

| ID | Acceptance criterion |
|----|----------------------|
| AC-E1 | Every model turn parses to valid JSON with the required fields; malformed output is handled without crashing the app. 🤖 |
| AC-E2 | On API error or timeout, the user sees a gentle in-voice message and can retry. |
| AC-E3 | A visible "thinking" state shows while awaiting a response. |

## J. Tone (NFR3)

| ID | Acceptance criterion |
|----|----------------------|
| AC-N1 | No therapy-cliché openers ("It sounds like you're feeling…"). 🤖 |
| AC-N2 | No validation-only turns during the interview (agreement with no question or reflection). |

## K. Accessibility & responsive (NFR5, NFR6)

| ID | Acceptance criterion |
|----|----------------------|
| AC-A1 | Fully keyboard-operable, with visible focus states. |
| AC-A2 | `prefers-reduced-motion` is respected. |
| AC-A3 | Layout is usable at mobile widths. |
| AC-A4 | Text/background contrast meets WCAG AA. |

---

## Traceability (every requirement has at least one test)

| Requirement | Covered by |
|-------------|-----------|
| FR1, FR2 | AC-F1–F4 |
| FR3 | AC-I1–I5 |
| FR4 | AC-T1–T2 |
| FR5 | AC-M1–M4 |
| FR6 | AC-C1–C2 |
| FR7, FR8 | AC-X1–X2, AC-C3 |
| NFR1 | AC-P1–P2 |
| NFR2 | AC-S1–S3 |
| NFR3 | AC-N1–N2 |
| NFR4 | AC-E3 |
| NFR5, NFR6 | AC-A1–A4 |
| R5 | AC-E1–E2 |

## Notes / to-resolve-in-Design

- The 5–8 exchange window (AC-T1) is a placeholder; the model decides pacing in practice, and we'll tune it.
- "Brief" in AC-I3 needs a concrete ceiling once we see real output.
- The 🤖-tagged criteria are the ones worth automating *if* you choose the full-SDET testing path — most map cleanly to assertions over the JSON contract rather than the UI, which is the cheaper, more stable layer to test.
