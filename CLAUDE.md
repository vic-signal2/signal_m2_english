# CLAUDE.md · how to work in this repository

*For any agent that opens this project.*

## The context

The product is **Milestone 2**, in `Signal_M2_English.html`, with an identical
copy in `index.html` for the site root. It is the **English build**, generated
from the Portuguese master by `tools/apply_translation.py` and the maps in
`translation/`. Content decisions are made in Portuguese and flow here.

## The golden rules

1. **The deterministic report is the heart and is never rebuilt.** Calculation
   and text generation are executable specification; evolve them, never rewrite.
2. **The database records, it does not replace.** Inputs and outputs, with the
   corpus version; no reimplemented calculation or text.
3. **Portuguese is the master language.** To change what the product *says*,
   change the master and the map. To fix a *bad translation*, change the map
   and regenerate. Never edit the HTML by hand: the next regeneration undoes it.
4. **The questionnaire is universal**: the same 25 questions for everyone.
5. **Scope decides.** What was not decided was not decided: ask first.
6. **When the mockup and a document disagree, the final mockup prevails**, and
   the document is updated. This was fixed in a workshop on 20 Sep and settled
   several open questions on its own.

## Writing

Simple, clear, concrete. No vague analogies. **No em dashes.** The score is
written with the percent symbol: **"59%"**. In the separation, what is within
reach is an action and starts with a verb; what is not is a condition.

## Delivering a change

1. Regenerate: `python3 tools/apply_translation.py <pt> Signal_M2_English.html`.
2. Copy it to `index.html`. The two must be identical.
3. Run all six suites (`tests/README.md`) and the checks in `docs/AUDIT.md`.
4. Record it in `CHANGELOG.md`: what changed **and why**.

## Technical traps

- **Never replace the `innerHTML` of a scrolled container** to reflect state:
  Safari defers the repaint and the screen stays blank until scrolled.
- **Every re-render wipes what is typed and not yet stored in state.** Store
  field values before calling `render()`. Two bugs came from this: the email
  erased by ticking the terms box, and the subject draft erased by choosing a
  country.
- **A re-render destroys element references a test is holding.** Re-query.
- **`innerText` returns text already transformed by CSS.** Compare
  case-insensitively.
- **State leaks between test scenarios through `localStorage`.** Clear it.
- **A test that calls an action by hand proves nothing about the click.**

## Translation traps

Read `docs/TRANSLATION.md` first. In short:

- Route names, action names, pillar keys, state keys, journey letters and step
  types are **identifiers**. Translating one breaks navigation or the engine
  silently.
- Some identifiers are **also** screen text (`'sinal'`, `'pilares'`). Fix those
  with a targeted patch on the display expression, never a map entry.
- **Do not trust a check that relies on recognizing Portuguese.** Every such
  check here had a blind spot. Compare against the Portuguese build instead:
  identical means untranslated.

## What not to do

- Change the universal questionnaire or the calculation without an explicit
  request.
- "Improve" approved texts without a new request.
- Use real third-party data.
- Change a business rule without confirming first.
