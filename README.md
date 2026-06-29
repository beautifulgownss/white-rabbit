# white rabbit

A bounded conversational tool for thinking through one hard decision. It
interviews you, then reflects back the gap between the reasons you *give* and
what actually carries *weight* — and hands the decision back to you. It never
tells you what to choose.

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
  *(note: describes the earlier "Threshold" palette; the shipped look is a
  cyberpunk-terminal direction — doc reconciliation pending)*
- `docs/04-test-plan.md` — test scenarios and exit criteria

## How it works (one paragraph)

Each turn, the app sends the conversation to the model with a system prompt that
defines the companion's character. The model returns a small JSON object
`{phase, message, pathA, pathB}`. The app renders `message`, tracks the phase
(framing → interview → mirror → close, plus a safety exit), and shifts the screen
from warm green to cool blue when it reaches the mirror. All state lives in
memory; nothing is stored.

## What's next

- Run real decisions through it and gather feedback (Phase 5).
- If it graduates: port to standalone (backend + API key), then optionally add
  automated checks on the JSON contract.
