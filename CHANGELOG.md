# Changelog

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
