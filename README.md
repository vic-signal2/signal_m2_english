# Signal · Milestone 2 · English build

**Signal** turns what people perceive at work into a structured reading of
organizational harmony, through the **Ten Pillars of Harmony™**, the
methodology of Victor B. Costa and Vocalis Consulting.

This repository holds the **English build** of Milestone 2. It is a single
self-contained HTML file: no server, no build step, no dependencies. Open it in
a browser and it runs.

---

## What is in here

| Path | What it is |
|---|---|
| `signal_m2_en.html` | The product. One file, ~705 KB, corpus embedded. |
| `index.html` | Redirect to the product, so the site root works. |
| `_headers` | No-store caching, so people always get the current build. |
| `docs/` | Business rules, deterministic core, screens, data model, design. |
| `tests/` | Four Playwright suites, 69 checks. |
| `translation/` | The Portuguese to English maps that produced this build. |

---

## Running it

Open `signal_m2_en.html` in any modern browser. On a phone it is the real
experience: **95% of users are on mobile**, and the build is designed for an
iPhone 13 viewport first.

State lives in `localStorage`. To start from zero, open a private window or
clear site data.

---

## Running the tests

```bash
npm install playwright
npx playwright install chromium
node tests/t_adjustments.js   signal_m2_en.html
node tests/t_two_signals.js   signal_m2_en.html
node tests/t_journey.js       signal_m2_en.html
node tests/t_review_round.js  signal_m2_en.html
```

Each suite prints a one-line summary and exits non-zero on failure. All four
pass against the build in this repository. See `docs/AUDIT.md` for what else
was verified and how to reproduce it.

---

## The relationship with the Portuguese master

**Brazilian Portuguese is the master language of Signal.** Every word of the
product is written in pt-BR first, and translations derive from it. This build
is one such derivation.

That has a practical consequence: **content changes do not start here**. A new
pillar text, a new journal question, a reworded screen: all of that is written
in the Portuguese repository and then flows into this one through the
translation map in `translation/`. Editing English text directly is allowed for
fixing a bad translation, and is the wrong move for changing what the product
says.

`docs/TRANSLATION.md` explains how the map works and how to regenerate this
build when the Portuguese source moves.

---

## The rules that never bend

1. **The deterministic report is the heart and is never rebuilt.** Indicator
   calculation and text generation are executable specification. Evolving them
   is incremental; rewriting from scratch is forbidden.
2. **The database records, it does not replace.** It persists inputs and
   outputs along with the corpus version. It never reimplements calculation or
   text.
3. **Portuguese is the master language.** See above.
4. **The questionnaire is universal.** The same 25 questions for everyone; what
   changes per journey is the framing of the texts.
5. **Scope decides.** What was not decided in the scope document was not
   decided. Ask before building it.

---

## Intellectual property

The **Ten Pillars of Harmony™**, the methodology, the assessment instrument,
the texts and the Signal material are the property of **Victor B. Costa and
Vocalis Consulting**, protected by copyright. Copying, reproduction,
translation, adaptation, distribution, model training and incorporation into
another product or service are not permitted without written authorization.

© 2026, all rights reserved. Contact: **contact@sendsignal.live**
