# white rabbit

A bounded conversational tool for thinking through one hard decision. It
interviews you, then reflects the gap between the reasons you *give* and what
actually carries *weight*, shows where each gathers on the two paths, and hands
the decision back to you. It never tells you what to choose.

**Status:** prototype, in testing (Phase 5). Single-file web app, no build step.

## Read this first — how it runs

The app expects the model API that the Claude.ai artifact runtime provides; the
in-app request to the model works there with no key. **Opening
`white-rabbit.html` on its own — or hosting it on static GitHub Pages — shows the
interface but the conversation will not work**, because nothing is answering
behind it.

Making it run anywhere is the planned *standalone port*: add your own model
access through a small backend that holds an API key. **Never put an API key in
this HTML file** — it ships straight to the browser. Until that port exists,
treat this repo as the source of truth and workbench, not a live deployment.

## What's here

- `white-rabbit.html` — the whole app: interface + conversation engine in one file
- `docs/01-project-plan.md` — vision, scope, success criteria, decision log
- `docs/02-requirements.md` — testable acceptance criteria
- `docs/03-design.md` — UX, visual direction, conversation contract
  *(note: this doc predates two shifts. The shipped look is a cyberpunk-terminal
  direction, not the "Threshold" palette it describes; and the conversation
  contract has since grown a weight-gauge (`pulls`) and a hand-back line
  (`handback`). Doc reconciliation pending.)*
- `docs/04-test-plan.md` — test scenarios and exit criteria

## How it works (one paragraph, then the part that's new)

Each turn, the app sends the conversation to the model with a system prompt that
defines the companion's character. The model returns a small JSON object —
`{phase, message, pathA, pathB, handback, pulls}` — and the app renders
`message`, tracks the phase (framing → interview → mirror, plus a safety exit),
and shifts the screen from warm green to cool blue when it reaches the mirror.
All state lives in memory; nothing is stored.

The `pulls` are what make the gap visible instead of merely stated. Alongside
each reply, the model tags up to two fragments of the person's reasoning as
either *stated* (a reason they'd defend out loud) or *felt* (where their
language actually lit up), and attaches each to path A or B. Those gather as
quiet weight on the two paths during the interview — no scores, just a fragment
that surfaces and a glow that rises on the heavier side. At the mirror the
interview recedes, the two paths rise into the center as a composed room, and
the accumulated weight becomes legible: the defensible reasons on one side, the
pull on the other, with the reflection and a one-line hand-back (`handback`) as
the centerpiece. The mirror is the destination now — reflect and hand back in
the same beat — so `close` survives only as a fallback.

## What's next

- Run real decisions through it and gather feedback (Phase 5) — especially
  whether the model tags *felt* vs *stated* weight accurately, since the gauge
  leans entirely on that judgment.
- Reconcile the docs with the shipped app: the weight-gauge and hand-back need
  acceptance criteria (`docs/02`) and test coverage (`docs/04`), and the design
  doc (`docs/03`) needs the palette and contract updates noted above.
- If it graduates: port to standalone (backend + API key), then optionally add
  automated checks on the JSON contract.
