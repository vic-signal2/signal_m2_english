# How the English build is produced

*Read this before changing a single word of English in this repository.*

Portuguese is the master language of Signal. This build is generated from the
Portuguese master by applying a translation map, not written by hand. That
matters: if you edit English text here and the Portuguese source later moves,
your edit disappears the next time the build is regenerated.

**Edit the map, then regenerate.** The map is in `translation/`.

---

## The two maps

| File | What it holds | How it is applied |
|---|---|---|
| `translation_map_pt_en.json` | 2,187 complete string literals | The whole literal, including its quotes, is replaced |
| `translation_words_pt_en.json` | 100 words and short phrases | Replaced only inside screen text, between tags |

The second map exists because some words sit **inside** a larger literal. The
button below is a single literal, so "Back" can only be reached by a word-level
replacement:

```js
'<button class="btn btn--q" data-a="proporAssunto">Voltar</button>'
```

Word-level replacements are applied only in these contexts, which is what keeps
them from touching code: `>word<`, `>word'`, `>word"`, `'word<`, `"word<`,
`'word'`, and the attribute values of `aria-label`, `title` and `placeholder`.

---

## The glossary

These are settled. Changing one means changing it everywhere, including the
lookup tables listed in `DETERMINISTIC_CORE.md`.

| Portuguese | English |
|---|---|
| sinal · pulso · vozes | signal · pulse · voices |
| Dez Pilares da Harmonia | Ten Pillars of Harmony |
| jornada | journey |
| diário | journal |
| somar o seu sinal | add your signal |
| ressoar | resonate |
| Ação · Estrutura · Coesão | Action · Structure · Cohesion |
| Autorresponsabilidade | Self-responsibility |
| Risco de Ruptura | Risk of Rupture |
| Crise Aguda · Crise | Acute Crisis · Crisis |
| Tensão · Harmônico · Íntegro | Tension · Harmonic · Whole |
| Organização · Liderança · Indivíduos | Organization · Leadership · Individuals |
| alcance dentro · fora | within your reach · not yours to carry |
| Assinaturas | Subscriptions |

**Íntegro → Whole** deserves a note. "Integral" reads technical in English and
"Sound" reads mechanical. "Whole" carries the wholeness that the Integrity
pillar rests on, and it survives inside sentences: *what is whole does not fear
being seen from within*.

---

## What must never be translated

A string in this codebase is one of three things, and only the first is text:

1. **Screen text.** Translate it.
2. **An identifier disguised as a string.** Never translate it.
3. **A value another piece of code searches for.** Translate both sides, in the
   same change, or neither.

The identifiers are: route names (`ir('sinal')`), action names (`data-a`),
pillar keys (`integridade`, `autorresponsabilidade`), state keys (`k`), journey
letters, layer keys (`org`, `lider`, `indiv`), and every key of `CORPUS`,
`DIARIO` and `SINAL` except the ones listed in `DETERMINISTIC_CORE.md` §5.

Two of these were learned the hard way. Both are in `AUDIT.md`:

- Translating the literal `'sinal'` to produce an English download filename
  also rewrote nine `ir('sinal')` calls, breaking navigation silently.
- Translating the corpus band key `alto` to `high` without moving the accessor
  `P.alto` made every pillar reading come back empty for anyone scoring 78 or
  above.

---

## Regenerating the build

The pipeline is deliberately simple: start from the Portuguese file, apply both
maps, then apply a short list of targeted patches for strings built by
concatenation.

1. Copy the Portuguese `signal_m2.html` to `signal_m2_en.html`.
2. Apply every entry of `translation_map_pt_en.json`, **longest first**, as
   `quote + source + quote` → `quote + target + quote`, escaping any quote that
   appears inside the English text. Apostrophes in English (`company's`) will
   break a single-quoted literal if this step is skipped.
3. Apply `translation_words_pt_en.json` in the restricted contexts listed
   above.
4. Apply the targeted patches, which cannot be expressed as whole literals:
   - the date formatter, rewritten to `Sep 17, 11:15 PM` and `en-US` locale
   - `P.alto.integro` / `P.alto.harmonico` → `P.high.*`
   - `'%</b>, em <b>'` → `'%</b>, in <b>'`
   - `"Assinar o plano '+p[0]+' plan?"` → `"Subscribe to the '+p[0]+' plan?"`
   - the locked-menu test, which matches on the notice text
   - the preview label chain, which needs `.replace('Estrutura','Structure')`
   - the download filename fallback `(chaveEmp(S.empresa)||'signal')`
   - the email field preserved when the terms box is ticked (see below)
5. Verify: JavaScript syntax, then the four suites, then the checks in
   `AUDIT.md`.

---

## Finding leftovers

Three detection methods were used, in increasing order of reliability. Use the
third.

**By regex over the source.** Fails: a naive quote-pairing regex drifts on the
first apostrophe inside a comment and silently skips whole regions of the file.
That is how the entire home screen went untranslated through 1,400 strings.

**By character-level tokenizer.** Better, still fails: it drifts on quotes
inside regular-expression literals.

**By rendering both builds side by side.** Reliable. Render the same screen in
the Portuguese and English builds and compare line by line: **any line that is
identical in both is untranslated text**. This does not depend on recognizing
Portuguese, which is exactly where the other two methods failed.

A fourth check catches what never reaches a screen in a given path: walk every
data structure and test each string against Portuguese morphology (`ção`,
`ções`, `mente`, `ência`, `nh`, `lh`, accented vowels). That is how the journey
layer labels and one shareable-image phrase were found.

Do not use a word list for detection. English "a", "no", "as", "do" and "time"
are all Portuguese words too, and a word list produces hundreds of false
positives that hide the three real ones.

---

## Divergences from the Portuguese master

This build is not a pure translation in two places, both deliberate:

**The crisis line.** The master points at CVV 188 and SAMU 192, which are
Brazilian. This build points at the 988 Suicide & Crisis Lifeline. Any other
market needs its own number.

**Fictional company names** in the sample pulse data were localized: Grupo
Vertente → Vertente Group, Malharia Serena → Serena Knitwear, Prisma
Consultoria → Prisma Consulting, Norte Digital → North Digital. Names that were
already neutral were left alone.

**One bug fix applies only here so far.** Ticking the terms checkbox re-rendered
the email screen and wiped the email the person had already typed; they then
got "write a valid email address" while looking at a field they had just
filled. The fix stores the typed value before the re-render. **The same bug
exists in the Portuguese master** and should be fixed there too.
