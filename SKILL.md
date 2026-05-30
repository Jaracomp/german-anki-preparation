---
name: german-anki-preparation
description: >
  Use this skill whenever the user wants to convert a list of German or Ukrainian words
  into a formatted CSV file for Anki. Triggers include: "зроби csv для anki", "відформатуй
  список слів", "підготуй слова для anki", "конвертуй слова в anki", providing a raw word
  list and asking to process it, or any mention of Anki + German vocabulary. Also triggers
  when the user pastes a list of words and asks to "add grammar", "format", or "prepare"
  them for study. Always use this skill — do not attempt to process word lists for Anki
  without it.
---

# German Anki Preparation

Converts raw German/Ukrainian word lists into Anki-ready CSV with full grammatical
information. Two-phase output: auto-process what's clear → surface ambiguities as a
numbered clarification queue the user answers with `1. b`, `2. a, c`, etc.

## Input format

- One word or phrase per line
- Language: DE, UA, or mixed
- Optional hint via `=` or `:`: `hund = собака`, `See: озеро`
- Explicit article (`der See`) = unambiguous, no question needed

## Output format

Two fields, separator **tab** (`\t`). Semicolons appear inside fields (multiple meanings), so they cannot be used as the column separator:

```
<Field 1: DE word + grammar><TAB><Field 2: UA translation(s)>
```

Multiple translations (inside Field 2):
- Similar meaning → comma: `гарний, красивий`
- Different meanings → semicolon: `том (книги); стрічка; гурт`

The CSV file is sorted by frequency of occurrence in the German corpus: most frequent first.

---

## Grammar templates — Field 1

Read `references/grammar-templates.md` for the full reference with all examples.
Summary of formats by PoS:

| PoS | Field 1 format | Example |
|-----|---------------|---------|
| Nomen | `art Wort, Gen-ending, Pl-ending` | `der Hund, -es, ¨-e` |
| Nomen (pl only) | `art Wort (pl), -, -` | `die Leute (pl), -, -` |
| Verb (regular) | `inf (3sg, Prät, hat/ist P.II)` | `kaufen (kauft, kaufte, hat gekauft)` |
| Verb (Vokalwechsel) | `inf (3sg, Prät, hat/ist P.II)` | `fahren (fährt, fuhr, ist gefahren)` |
| Verb (trennbar) | `inf (3sg, Prät, hat/ist P.II)` | `anrufen (ruft an, rief an, hat angerufen)` |
| Verb (reflexiv) | `sich inf (3sg, Prät, hat/ist P.II)` | `sich freuen (freut sich, freute sich, hat sich gefreut)` |
| Verb (Rektion) | `inf (3sg, Prät, hat/ist P.II); Präp +Kasus` | `warten (wartet, wartete, hat gewartet); auf +Akk` |
| Adjektiv | `adj (Komp, Superl)` | `schnell (schneller, am schnellsten)` |
| Adjektiv (irreg.) | `adj (Komp, Superl)` | `gut (besser, am besten)` |
| Adjektiv (Rektion) | `adj Präp +Kasus (Komp, Superl)` | `stolz auf +Akk (stolzer, am stolzesten)` |
| Adverb | `adv` or `adv (Komp, Superl)` | `leider` / `gern (lieber, am liebsten)` |
| Präposition | `präp +Kasus` | `mit +Dat` |
| Wechselpräp. | `präp +Dat/+Akk [wo/wohin]` | `in +Dat/+Akk [wo/wohin]` |
| Konjunktion | `konj [type]` | `weil [sub]` |
| Partikel | `wort [partikel]<br>Beispielsatz.` | `doch [partikel]<br>Das ist doch klar!` |
| Abkürzung | `Volle Name (ABKÜR.) [Abk.]` | `Europäische Union (EU) [Abk.]` |
| Pronomen | word only | `jemand` |
| Zahlwort | word only | `zwanzig` |
| Phrase | phrase as-is | `auf jeden Fall` |
| Redewendung | `phrase [Rdw]` | `jemandem auf die Nerven gehen [Rdw]` |

---

## Symbol legend

| Symbol | Meaning |
|--------|---------|
| `¨` | Umlaut in that form |
| `¨-e` | Umlaut + ending -e |
| `-` | No change / form absent |
| `(pl)` | Pluralia tantum |
| `sich` | Reflexive verb |
| `+Akk / +Dat / +Gen` | Case after preposition |
| `+Dat/+Akk` | Wechselpräposition |
| `[wo/wohin]` | Wechselpräp. usage hint |
| `[koord]` | Coordinating conjunction (word order unchanged) |
| `[sub]` | Subordinating conjunction (verb to end) |
| `[adv]` | Adverbial conjunction (verb-second inversion) |
| `[partikel]` | Modal particle |
| `[Abk.]` | Abkürzung — full form known |
| `[Rdw]` | Redewendung / idiom |
| `[ugs.]` | Colloquial/slang |
| `<br>` | Line break in Anki card |

---

## Processing pipeline

1. Detect language per line (DE / UA / mixed)
2. Normalize to base form (infinitive / Nominativ Sg) — see rules below
3. Remove duplicates silently
4. Determine PoS and generate grammatical forms
5. Check for ambiguities → build clarification queue
6. Generate CSV for unambiguous words
7. Sort by German corpus frequency
8. Output: CSV block + clarification queue (if any)
9. After user answers → append resolved words, re-sort, output final CSV

### Normalization rules

| Input form | Action |
|-----------|--------|
| Verb not in infinitive (`fuhr`) | Normalize silently → `fahren` |
| Inflected adjective (`schnellen`) | Normalize silently → `schnell` |
| Trennbar in conjugated form (`ruft an`) | Normalize silently → `anrufen` |
| Plural with unambiguous singular (`Hunde`) | Normalize silently → `der Hund` |
| Plural ambiguous (maps to 2+ singulars) | → Clarification queue |
| Pluralia tantum (`Leute`) | Keep, add `(pl)` |
| Wrong article (`die Hund`) | Correct silently, note: "Виправлено: die → der" |

---

## Ambiguity categories

Read `references/ambiguity-handling.md` for full decision trees.
Quick reference — when to ask vs. act silently:

| Situation | Action |
|-----------|--------|
| Word not found, similar words exist | Suggest candidates in queue |
| Word not found, no candidates | Mark unknown, add to queue |
| Proper noun | Ask: skip or include as-is? |
| Possible slang/colloquial | Suggest base form, ask confirmation |
| Same spelling, different articles (different words) | Always ask — list all meanings with UA context |
| Ambiguous PoS with different grammar (`übersetzen`) | Always ask — show both with UA translation |
| UA word with multiple DE translations | Ask which to use; allow selecting multiple |
| Abstract UA word | Ask for usage context, propose candidates |
| Multi-word input, unknown if fixed phrase | Ask: fixed phrase or separate words? |
| Abbreviation with unknown full form | Ask for clarification — show known candidates if any |
| Kompositum (compound word) | Recommend components, allow selecting full word + parts |

---

## Output format

```
✅ ГОТОВО — 27 карток
────────────────────────

⚠️ ПОТРЕБУЄ УТОЧНЕННЯ — 4 пункти

1. "See" — кілька значень:
   [a] der See, -s, -n — озеро
   [b] die See, -, -n — море
   [c] Усі
   [d] Пропустити

2. "übersetzen" — два різних дієслова:
   [a] übersetzen (setzt über, setzte über, ist übergesetzt) — перевозити через воду/кордон
   [b] übersetzen (übersetzt, übersetzte, hat übersetzt) — перекладати текст
   [c] Усі
   [d] Пропустити

3. "іти" — яке значення руху?
   [a] gehen (ging, ist gegangen) — іти пішки
   [b] fahren (fährt, fuhr, ist gefahren) — їхати транспортом
   [c] laufen (läuft, lief, ist gelaufen) — бігти / іти (розм.)
   [d] Усі
   [e] Пропустити

4. "Umrechnungsfaktor" — що додати?
   AI рекомендує: a + b
   [a] der Umrechnungsfaktor, -s, -en — коефіцієнт перерахунку
   [b] umrechnen (rechnet um, rechnete um, hat umgerechnet) — перераховувати
   [c] die Umrechnung, -, -en — перерахунок
   [d] der Faktor, -s, -en — фактор, коефіцієнт
   [e] Усі
   [f] Пропустити
```

User replies in one message:
```
1. b
2. a
3. a, c
4. a, b
```

Accepted answer values: letter(s) (`a`, `b`, `a, c`), `усі` (all options), `пропустити` (skip entry).
