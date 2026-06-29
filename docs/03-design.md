# Design — *fork*

**Phase:** 3 of 6 · Design
**Status:** Draft for review
**Last updated:** 2026-06-29

---

## 1. UX flow

One person, one decision, five states. Linear, with a safety interrupt that can fire from anywhere.

```
  THRESHOLD  →  FRAMING  →  INTERVIEW  →  MIRROR  →  CLOSE
   (entry)      (name        (one Q       (stated    (hand it
                the two      at a time)    vs felt)    back)
                paths)
                     �‖
              SAFETY INTERRUPT  ← can fire from any state
              (leave the decision frame, surface care)
```

**Threshold** — Calm entry. Wordmark, one line of what this is, a single input: *"What are you trying to decide?"* Nothing else.

**Framing** — User describes the decision. Companion reflects it in one line, names the two paths, asks the first real question. The two paths now pin to the top and stay there.

**Interview** — Adaptive. One question per turn, following the user's own words. The two paths remain visible above, held in balance.

**Mirror** — When the real weight has surfaced, the companion stops asking and reflects back: the reasons given, what carries weight underneath, the gap. **The light shifts here** — this is the signature moment.

**Close** — One framing that returns the decision to the user. Then: *keep a copy* / *start over*.

## 2. Visual direction — "The Threshold"

The emotional truth of a fork isn't anxiety — it's the quiet of finally sitting down with the thing. So the space is warm-dark and contemplative, lit by a single lamp. Two futures held in tension at the top, never weighted toward one. When the tool stops gathering and starts reflecting, the light cools — a visible signal that the register has changed.

This deliberately avoids the three default AI looks (cream + serif + terracotta; black + acid accent; broadsheet hairlines).

**Color**
| Token | Hex | Use |
|-------|-----|-----|
| ink | `#1A1A2E` | background — deep twilight indigo, not black |
| dusk | `#232347` | raised surfaces |
| lamp | `#E8B86D` | the one warm accent, used sparingly |
| parchment | `#ECE6D8` | primary text |
| mist | `#8B8BA7` | labels, secondary text |
| sage | `#9DB3A0` | the cooler light of the mirror register |

**Type**
- **Fraunces** — the companion's voice (questions and reflections). Literary, warm; italic for the mirror to feel intimate.
- **Instrument Sans** — UI, labels, and the user's own words. Quiet, modern.
- A dialogue distinguished by *typeface and alignment*, not chat bubbles — closer to a written exchange than a messaging app.

**Layout** — A single narrow centered column (~640px), letter/journal proportions, generous line-height. Two paths pinned at top once named. Input fixed at the bottom.

**Motion** — Companion turns fade-rise gently (the considered pause). A soft breathing dot while thinking. The light-cool at the mirror is the one orchestrated moment. `prefers-reduced-motion` respected.

**Signature** — The two paths held in balance + the light shift at the mirror. Boldness spent here; everything else stays quiet.

```
            fork
       think through one decision

   ┌──────────────────────────────────┐
   │   STAY                LEAVE       │   ← held, neither emphasized
   └──────────────────────────────────┘

     What's making this hard            ← companion · Fraunces italic
     to choose right now?

                  the money's fine but   ← user · Instrument Sans, right
                  i feel invisible there

     Invisible to whom, specifically?    ← companion

   [ take your time…              ] [→]
```

## 3. Conversation-engine contract

The intelligence is one LLM call per turn that returns structured JSON. The app renders `message`, tracks phase, and shifts the UI on `mirror`/`close`/`safety`.

**Per-turn JSON schema**
```json
{
  "phase": "framing | interview | mirror | close | safety",
  "message": "string — what the user reads",
  "pathA": "string or null",
  "pathB": "string or null"
}
```
- `pathA`/`pathB` set at framing, stable thereafter.
- App strips any stray code fences and parses defensively (AC-E1).
- Each call, the app injects a short state note: current phase + exchange count + a pacing nudge toward the mirror.

**System prompt (the companion's character)**
```
You are the quiet voice inside fork — a tool for thinking through ONE hard
life decision. You are not a therapist, coach, or friend who fixes things.
You're closer to a perceptive person across a table who asks the question no
one else thought to, and who notices the difference between the reasons
someone gives and what actually has weight for them.

Your job: help this person hear their own reasoning clearly enough to decide.
You never tell them what to choose — you protect that the decision is theirs.

PRINCIPLES
- One question at a time. Never stack questions. Never give lists.
- Follow what they actually said — their specific words, what they lingered
  on, what they rushed past. Don't run a script.
- Warm and unhurried, but honest. Don't only validate. If they give the
  "responsible" answer while their language lights up around the other path,
  gently point at it.
- Notice the gap between stated reasons (what sounds defensible) and felt
  weight (what they're actually pulled toward or afraid of). That gap is the
  point.
- Brief: one to three sentences. No bullet points, no therapy-speak, no
  "it sounds like you're feeling."
- Don't rush to resolve. Sit in the hard part.

PHASES
- framing: On the first message, reflect the decision back in one line, name
  the two paths as you hear them (set pathA/pathB), ask your first real
  question.
- interview: Keep asking, one question at a time. Surface values, fears,
  regrets, who they're performing for.
- mirror: When the real weight has surfaced (usually 5–8 exchanges), STOP
  asking. Reflect back the reasons they gave, what actually carries weight
  underneath, and the gap between them. Name it plainly and kindly. Do NOT
  pick for them.
- close: One framing that hands the decision back — something they can carry
  out of the room. Warm. Done.
- safety: If they express crisis-adjacent distress, LEAVE the decision frame
  entirely. Respond with plain care, encourage reaching out to someone they
  trust or a crisis line, and do not keep interviewing. Never assess risk
  clinically or mention methods.

OUTPUT
Respond with ONLY a JSON object — no markdown, no code fences:
{"phase":"...","message":"...","pathA":"... or null","pathB":"... or null"}
```

## 4. Decisions this phase resolves

- **D2 — where it lives.** The design is deployment-agnostic: single file, one LLM call per turn, no backend, no storage. It drops cleanly into either a Claude artifact (fastest to a working thing) **or** a standalone static site (more control, your own domain, shareable link). Your call now.
- **D1 — name.** "fork" is still the working name. The wordmark is built to be swapped.

## 5. Open tuning knobs (resolved in build/test, not now)

- Exact pacing before the mirror (the 5–8 window).
- The concrete length ceiling for companion turns.
- Whether the two-path display ever shows a third pole or stays strictly binary.
