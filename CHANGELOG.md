# Changelog

## 2026-09-22 · The good-faith declaration in the Terms

Approved by Victor, it enters as the second paragraph of clause 3 of the Terms
("Who can use it"), with no change to the numbering: whoever answers declares a
real relationship with the assessed company, a sincere perception, and no intent
to harm or to favor anyone; and Signal does not ask for the reading to be
positive or negative, but for it to be the person's own. One new check in
`t_legal.js`. **198 checks, all green.**

## 2026-09-22 · A fine-comb pass

A full pass over the Portuguese master found five small leftovers of the old
model, fixed there and carried here: the heading after adding still said the
signal was part of "a pulse" (now "the numbers of your industry"); the delete
notice spoke of "added to a pulse"; the home footer link used the old, inverted
page name (now "Terms and privacy"); the ™ was missing in the home paragraph and
the pillars heading used a superscript "TM" (now ™); and the font licence notice
inside the file itself, asked for in point 11, was added before the first
@font-face. Five new checks in `t_legal.js` lock these fixes. **197 checks, all
green**, the same number as the master.

## 2026-09-21 · The legal workshop reaches the English build

The Portuguese master changed with the legal workshop (commit 5238316), and this
build was regenerated from it.

**What changed in the product:** no company appears any more (My Company became
My Industry, World Pulses lists industries, numbers need 5 voices); Add your
signal asks for company, industry and country; the six policy documents (Terms
of Use 2.0, Privacy Policy 1.0, Principles Charter, Moments of crisis,
Accessibility, Credits and licenses); consent for free text; minimum age 18;
14-day refund; Stripe Checkout, with Stripe as the reseller; the California
renewal notices; a crisis protocol that reacts to what the person writes, with
988 and findahelpline.com as the default in this build; the "Signal by Vocalis"
brand; and the WCAG 2.1 AA fixes.

**Translation:** about 155 new strings; the map now has 2,519 entries. The
industry list comes out in English alphabetical order because the master sorts
it by the page language. Leftovers from the first translation were found and
fixed ("Entrar", several toasts and dialogs, accessible names, and "Seguir"
rendered as "Follow").

**Tests:** four new suites ported (`t_remaining`, `t_industries`, `t_legal`,
`t_accessibility`), all failing on the previous build; the six existing suites
adjusted where the change was intentional. **192 checks, all green**, the same
number as the Portuguese master.

**A test that was not testing this build:** `t_journey.js` loaded a fixed path
from the machine that produced the first package, and ignored the file passed
to it, so its 6 checks were about an old Portuguese file. It now tests the file
given, or `Signal_M2_English.html` by default, as every suite does. Once pointed
at this build it failed where adding now needs an industry, and was adjusted.

**Docs:** RULES (ten sections brought up to date, including two that were stale
since before this round), SCREENS_AND_FLOWS, DATA_MODEL, VISUAL_FIDELITY (the
typography named the wrong fonts), DETERMINISTIC_CORE, TRANSLATION, AUDIT,
LICENSE (the authorship split between Victor B. Costa and Vocalis Consulting),
README and tests/README.

## Portuguese that survived the first translation

A second audit found about 150 texts still in Portuguese, most of them where
people see them. The first audit's checks all relied on recognizing Portuguese,
and each had a blind spot: plain sentences without accents were discarded as
"technical", single-letter words ("501 a 5.000") were ignored, mixed lines
("PERFIL 20% complete") never matched line by line, and the questionnaire had
been walked along one branch only.

Fixed: the size bands, "Yes" and the "company as a whole" option in the
questionnaire, and the "Profile" label; 29 reach items in the separation;
journal questions and prompts; "The signal:" before every high pillar reading;
the processing screen, share-image buttons, account menu, Voices and Pulses
labels; plural labels; and the intellectual property paragraph.

**The layer sentence** mixed both languages ("comes more of the business … and
less from da empresa"). The layer names now hold only the noun, and the
sentence supplies "from".

Three identifiers doubled as screen text (`'sinal'`, `'pilares'`, `'assunto'`);
only their display expressions changed.

Verified with checks that do not depend on recognizing Portuguese, now in
`tools/` and `docs/AUDIT.md`: data compared path by path, 121 states across
every branch, every word on 56 screens and in 2,652 generated texts. Keys and
engine identical to the Portuguese build; 471 clicks clean; 90 checks green.

## Answers to Elaine's questions

In line with the Portuguese master at commit `ecd950a`.

**Subscriptions renew automatically**, reversing the previous rule: monthly on
the same day each month, annual on the anniversary. The one-off does not renew.
The confirmation states both monthly prices and requires a separate tick for
automatic renewal. Subscriptions shows the next renewal date and can turn
renewal off and back on. Dates read "October 21".

**The Assistant drawer no longer reaches the Answer screen**, which is the
person's own words, with no AI. **The subject draft survives choosing a
country.** **The sensitive area uses the palette red.** **The annual badge
reads "49% off".**

## Initial English build

The complete English version of Milestone 2, generated from the Portuguese
master and audited.

Bugs the first audit caught: every pillar reading was empty above 78 (a
translated corpus key the code still read in Portuguese); nine navigations broke
because a route name was translated; dates rendered as "17 of set"; and ticking
the terms box erased the typed email, a bug since fixed in the master too.

Deliberate divergences: the crisis line points at the 988 Suicide & Crisis
Lifeline instead of the Brazilian CVV 188 and SAMU 192; fictional company names
with Portuguese names were localized.
