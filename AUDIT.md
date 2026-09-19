# Audit of the English build

*What was verified before this build was considered finished, what it found,
and how to run it again.*

A translated product is not verified by reading it. Most of what broke here was
invisible on screen: an empty string where a paragraph belonged, a route name
that no longer resolved, a date built from a Portuguese template. The checks
below exist because each of them caught something.

---

## What the audit found

### 1 · Every pillar reading was empty above 78

The corpus band key `alto` had been translated to `high`, while the code still
read `P.alto`. Anyone in a **Harmonic** or **Whole** state received an empty
string where the pillar reading belonged.

This is the most dangerous class of bug in a translation: nothing throws,
nothing looks broken, the product simply says less than it should to the people
who are doing well.

**Caught by:** generating all 1,692 texts across 6 journeys × 6 states and
asserting that none is empty, undefined or `[object Object]`.

### 2 · Nine navigations were broken by one word

Translating the literal `'sinal'` (to give the downloaded report an English
filename) also rewrote nine `ir('sinal')` calls into `ir('signal')`, a route
that does not exist.

**Caught by:** comparing the internal key sets of both builds. Routes, actions,
pillars, states, journeys, layers, icons, menus and question ids must be
**identical** in the two languages.

### 3 · Ticking the terms box erased the typed email

On the email screen, typing an address and then ticking "I have read and
accept" re-rendered the screen and cleared the field. The person then pressed
continue and got "write a valid email address" while looking at the address
they had just typed, on the screen that decides conversion.

**This bug also exists in the Portuguese master** and should be fixed there.

**Caught by:** driving the journey with real clicks rather than by calling
actions directly.

### 4 · Dates rendered as "17 of set"

The Portuguese formatter builds `17 de set, 23h15`. A global replacement of
`" de "` reached it. The formatter was rewritten for English: `Sep 17,
11:15 PM`, with the `en-US` locale.

### 5 · Portuguese surviving in rare paths

Journey-specific layer labels, the downloadable report's section titles, eight
system notices, download filenames, the assistant's opening line, the "0 of 3
marked" counter, "Create signal", "Pillar 1 of 3", two corpus lines and one
shareable-image phrase.

**Caught by:** rendering both builds side by side and flagging identical lines,
plus a morphology sweep over every data structure.

---

## The checks, and how to run them

All of them assume Playwright with Chromium installed and run against
`signal_m2_en.html`.

### Structure
Every `data-a` in the source has a handler in `A`; every `ir('...')` target
exists in `R`; every `I.<icon>` exists. Expect **89 actions, 25 routes, 31
icons, nothing orphaned**.

### Internal keys
Compare against the Portuguese build: routes, actions, pillar keys, state keys,
journey letters, group key lists, menus, layers, icons, profile keys, question
ids and each question's pillar. **All must be identical.** This is the check
that would have caught the broken routes immediately.

### Corpus parity
Walk both builds and compare the *shape* of `CORPUS`, `DIARIO`, `SINAL`,
`QUESTIONS`, `PERFIL`, `POL`, `PULSOS`, `VOZES`, `PAINEIS`, `ARTE`: same paths,
same types, same array lengths, no string that became empty. Expect **3,907
keys, identical shape**.

The only intended difference is the band key `alto` → `high`, in ten pillars.

### Click crawler
For each of the 25 routes, build a complete state, enumerate every `[data-a]`
element and click each one from a fresh state, checking that `#app` never
empties and that no screen prints `undefined`, `NaN` or `[object Object]`.
Expect **471 clicks, zero problems**. Destructive actions are excluded here and
tested separately.

### All journeys × all states
For each of the 6 journeys and 6 score bands, generate the question, the
past-tense question, the layer text, the synthesis sentence, all ten pillar
readings, the reach lists, both journal questions per step and the image
phrase. Expect **1,692 texts, zero empty, zero broken, zero Portuguese**.

### Deterministic engine
Run seven scenarios through both builds and compare score, state key, journey,
all ten pillar scores, the three layer scores and the choice of pillars to
separate. Expect **zero divergences**. This is the proof that translation did
not move a single number.

### Journey by clicks
Home → questionnaire (answered by clicking options) → email screen, including
its rejection of empty and invalid addresses → payment → pillars → separation,
marking items one at a time → actions → journal, ten answers typed and saved →
answer. Expect **7 of 7**.

Note: answering every statement with "strongly agree" produces a perfect score,
and a perfect score correctly leaves **nothing to separate**. Use a middling
option if you want the separation flow to be exercised.

### Keyboard
The whole questionnaire can be answered with number keys: expect **20 answers
and arrival at the email screen**.

### Data, persistence and destructive actions
State survives a reload; a second signal archives the first and clears the
answers; returning to the first signal restores its report and journal;
withdrawing from the pulse works; deleting a signal leaves no broken screen.

### Artifacts
The shareable image generates a real PNG in memory (about 2.2 MB, 12 share
channels) and the downloadable report builds as HTML and as a Blob, titled
`Signal · full report`, containing the score, with no Portuguese.

### Assistant
Opens, declares what it knows, answers a suggestion, and respects the quota of
15 on the one-off.

### Portuguese detection
Two sweeps, both expected to return zero: every rendered screen, and every
string inside every data structure. See `TRANSLATION.md` for why the detector
uses morphology rather than a word list.

### Test suites

```bash
node tests/t_adjustments.js   signal_m2_en.html   # 32 checks
node tests/t_two_signals.js   signal_m2_en.html   #  6 checks
node tests/t_journey.js       signal_m2_en.html   #  6 checks
node tests/t_review_round.js  signal_m2_en.html   # 25 checks
```

**69 checks, all green.**

---

## A note on test failures

Several failures during this audit were the *test* being wrong, not the
product: a stale element reference after a re-render, a selector that changed
name (`irAlcance` becomes `sepSegue` once a pillar is complete), an assertion
still searching for Portuguese text, and `innerText` returning text already
transformed to uppercase by CSS.

The discipline that paid off: **before fixing anything, prove the product is
actually wrong**. Twice during this work a "bug" turned out to be correct
behaviour, and changing it would have broken a real gate.
