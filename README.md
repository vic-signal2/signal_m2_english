# Signal · Milestone 2 · English build

**Signal** turns what people perceive at work into a structured reading of
organizational harmony, through the **Ten Pillars of Harmony™**, the
methodology of Victor B. Costa and Vocalis Consulting.

This repository holds the **English build** of Milestone 2: a single
self-contained HTML file, with no server, no build step and no dependencies.

---

## What is in here

| Path | What it is |
|---|---|
| `Signal_M2_English.html` | The product. One file, corpus embedded. |
| `index.html` | **An identical copy of the product**, so the site root opens Signal directly. |
| `_headers` | No-store caching, so people always get the current build. |
| `docs/` | Rules, deterministic core, screens, data model, design, translation, audit. |
| `tests/` | Six Playwright suites, 90 checks. |
| `translation/` | The two Portuguese to English maps that produce this build. |
| `tools/` | The script that generates the build, and the data comparison check. |

**`index.html` and `Signal_M2_English.html` must always be the same file.**
Whenever the build changes, update both. The site root serves `index.html`.

---

## Running it

Open `Signal_M2_English.html` in any modern browser. **95% of users are on
mobile**, and the design targets an iPhone 13 viewport first. State lives in
`localStorage`; open a private window to start from zero.

## Running the tests

```bash
npm install playwright
npx playwright install chromium
node tests/t_adjustments.js     Signal_M2_English.html
node tests/t_two_signals.js     Signal_M2_English.html
node tests/t_journey.js         Signal_M2_English.html
node tests/t_review_round.js    Signal_M2_English.html
node tests/t_email_terms.js     Signal_M2_English.html
node tests/t_elaine_answers.js  Signal_M2_English.html
```

---

## The relationship with the Portuguese master

**Brazilian Portuguese is the master language of Signal.** Every word is
written in pt-BR first; this build is generated from it.

**Content changes do not start here.** A new text or a reworded screen is
written in the Portuguese repository, and flows here through `translation/`.
To fix a bad translation, change the map and regenerate
(`docs/TRANSLATION.md`); editing the HTML directly is lost at the next
regeneration.

---

## The rules that never bend

1. **The deterministic report is the heart and is never rebuilt.** Evolving it
   is incremental; rewriting from scratch is forbidden.
2. **The database records, it does not replace.** It persists inputs and
   outputs with the corpus version, and never reimplements calculation or text.
3. **Portuguese is the master language.**
4. **The questionnaire is universal**: the same 25 questions for everyone.
5. **Scope decides.** What was not decided was not decided: ask first.
6. **When the mockup and a document disagree, the final mockup prevails**, and
   the document is what gets updated.

---

## Intellectual property

The Ten Pillars of Harmony™, the methodology, the assessment instrument, the
texts and the Signal material are the property of **Victor B. Costa and Vocalis
Consulting**. See `LICENSE.md`. © 2026, all rights reserved.
Contact: **contact@sendsignal.live**
