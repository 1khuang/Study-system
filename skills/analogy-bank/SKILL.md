---
name: analogy-bank
description: >
  Generate an analogy that connects a new concept to something the
  learner already understands, then save the ones that work into a
  per-topic analogy bank for re-use. Use when the learner says
  "give me an analogy", "I don't get this — try another way", or when
  an explanation just failed a comprehension check.
phase: 3
triggers:
  - "give me an analogy"
  - "explain it like I'm a"
  - "try another way"
  - "I don't get this"
  - the learner failed the same comprehension check twice
inputs:
  - "topic-config.md: §7 (prior knowledge & preferences)"
  - "progress/<tracker>: §\"Analogy Bank\" (created on first use), §\"Per-Domain Detail\""
outputs:
  - "tracker: creates / updates §\"Analogy Bank\""
  - "session-notes: appends an \"Analogy\" block noting which analogy was tried and whether it worked"
verification: inherits
---

## Purpose

The default loop allows "try a different angle" on a miss, but doesn't
*remember* which angles worked. This skill builds a persistent,
per-topic analogy bank inside the tracker so the coach can reach for a
known-good analogy on later sessions and across related sub-topics.

## When to use

- Comprehension check failed and re-explaining didn't help.
- Learner explicitly asks for an analogy.
- Concept is abstract / unfamiliar (probability, recursion, options
  pricing, immune-system signaling, …).

## When NOT to use

- Topic is already concrete and procedural (analogies often
  *over-complicate* — use `worked-example`).
- Learner is in `strict` exam-prep mode and the analogy might
  introduce a non-canonical model. Caveat heavily if you proceed.

## Procedure

1. Identify the concept the learner is stuck on and the specific
   property that's not landing (mechanism, scale, direction, trade-off).
2. Read `topic-config.md` §7 *Prior Knowledge* and any prior bank
   entries — prefer analogies grounded in domains the learner already
   knows.
3. Offer **2 candidate analogies**, briefly. Ask the learner to pick
   the one that resonates more.
4. Stretch the chosen analogy: where does it map cleanly? Where does
   it **break**? Be explicit about the breakdown — analogies that pretend
   not to break create later misconceptions.
5. Re-pose the original comprehension check in the analogy's terms,
   then in the original terms.
6. If the analogy worked (learner now passes the check), promote it to
   the bank. If it didn't, log it as "tried, didn't help" so the coach
   doesn't reach for it again.
7. **Tracker write-back** — see below.

## Verification policy hook

Analogies are pedagogical, not factual. Under `strict`, the coach must
add a one-line caveat: *"Analogy only — for the exam, use the
canonical phrasing from {{source}}."* Under `standard`/`relaxed`, no
extra citation is needed.

## Tracker write-back

- **tracker** —
  - Maintain a `## Analogy Bank` section with rows
    `Concept | Analogy (1 line) | Where it works | Where it breaks |
     Status (worked / didn't help)`.
  - On success, also bump the related sub-topic from `[ ]` → `[~]` or
    update its *Key points* with a 1-line analogy reference.
- **session-notes** — append an `### Analogy` block listing the two
  candidates, which one was picked, and whether it resolved the
  blocker.

## Example

> **Learner:** "I keep losing on Roth conversion timing. Give me an
> analogy."
>
> **Coach (skill: `analogy-bank`):** "Two options:
> (a) Pre-paying tax now is like buying a non-refundable train ticket
> at today's price even though tomorrow's price might be higher.
> (b) Like converting cash to a foreign currency before a vacation —
> you lock in today's exchange rate and skip future volatility.
> Which one clicks more?"
>
> *(learner picks (b); coach maps the cleanly-mapping bits, flags
> where 'currency' breaks down because the IRS isn't a market; logs to
> the bank with status: worked.)*
