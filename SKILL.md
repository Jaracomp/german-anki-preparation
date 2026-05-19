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
- Optional hint via `=`: `hund = собака`, `See = озеро`
- Explicit article (`der See`) = unambiguous, no question needed

## Output format

Two fields, separator **tab** (`\t`). Semicolons appear inside fields (multiple meanings), so they cannot be used as the column separator:

```
<Field 1: DE word + grammar><TAB><Field 2: UA translation(s)>
```

Multiple translations (inside Field 2):
- Similar meaning → comma: `гарний, красивий`
- Different meanings → semicolon: `том (книги); стрічка; гурт`

CSV is sorted by German corpus frequency: most frequent first, phrases/idioms `[Rdw]` last,
unknown-frequency words just before idioms.

---

## Grammar templates — Field 1

Read `references/grammar-templates.md` for the full reference with all examples.
Summary of formats by PoS:

| PoS | Field 1 format | Example |
|-----|---------------|---------|
| Nomen | `art Wort, Gen-ending, Pl-ending` | `der Hund, -es, ¨-e` |
| Nomen (pl only) | `art Wort (pl), -, -` | `die Leute (pl), -, -` |
| Verb (regular) | `inf (Prät, hat/ist P.II)` | `kaufen (kaufte, hat gekauft)` |
| Verb (Vokalwechsel) | `inf (3sg, Prät, hat/ist P.II)` | `fahren (fährt, fuhr, ist gefahren)` |
| Verb (trennbar) | `pre\|inf (Prät, hat/ist P.II)` | `an\|rufen (rief an, hat angerufen)` |
| Verb (reflexiv) | `sich inf (...)` | `sich freuen (freute sich, hat sich gefreut)` |
| Verb (Rektion) | `inf (...); Präp +Kasus` | `warten (wartete, hat gewartet); auf +Akk` |
| Adjektiv | `adj (Komp, Superl)` | `schnell (schneller, am schnellsten)` |
| Adjektiv (irreg.) | `adj (Komp, Superl)` | `gut (besser, am besten)` |
| Adjektiv (Rektion) | `adj Präp +Kasus (Komp, Superl)` | `stolz auf +Akk (stolzer, am stolzesten)` |
| Adverb | `adv` or `adv (Komp, Superl)` | `leider` / `gern (lieber, am liebsten)` |
| Präposition | `präp +Kasus` | `mit +Dat` |
| Wechselpräp. | `präp +Dat/+Akk [wo/wohin]` | `in +Dat/+Akk [wo/wohin]` |
| Konjunktion | `konj [type]` | `weil [sub]` |
| Partikel | `wort [partikel]<br>Beispielsatz.` | `doch [partikel]<br>Das ist doch klar!` |
| Pronomen | word only | `jemand` |
| Zahlwort | word only | `zwanzig` |
| Phrase | phrase as-is | `auf jeden Fall` |
| Redewendung | `phrase [Rdw]` | `jdm. auf die Nerven gehen [Rdw]` |

**Verb note:** Write `hat` and `ist` in full, never abbreviated. Add 3sg Präsens **only** when there is a vowel change (fahren→fährt, lesen→liest). Regular 3sg (kauft, macht) — omit.

**Adjektiv note:** Show comparative for ALL adjectives, including regular ones.

---

## Symbol legend

| Symbol | Meaning |
|--------|---------|
| `¨` | Umlaut in that form |
| `¨-e` | Umlaut + ending -e |
| `-` | No change / form absent |
| `(pl)` | Pluralia tantum |
| `\|` | Trennbar boundary (an\|rufen) |
| `sich` | Reflexive verb |
| `+Akk / +Dat / +Gen` | Case after preposition |
| `+Dat/+Akk` | Wechselpräposition |
| `[wo/wohin]` | Wechselpräp. usage hint |
| `[koord]` | Coordinating conjunction (word order unchanged) |
| `[sub]` | Subordinating conjunction (verb to end) |
| `[adv]` | Adverbial conjunction (verb-second inversion) |
| `[partikel]` | Modal particle |
| `[Rdw]` | Redewendung / idiom |
| `[ugs.]` | Colloquial/slang |
| `jdm.` | jemandem (Dat) |
| `etw.` | etwas (Akk) |
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
| Trennbar in conjugated form (`ruft an`) | Normalize silently → `an\|rufen` |
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

---

## Output format

```
✅ ГОТОВО — 27 карток

[CSV block — ready to copy]

────────────────────────
⚠️ ПОТРЕБУЄ УТОЧНЕННЯ — 3 пункти

1. "See" — кілька значень:
   [a] der See, -s, -n — озеро (внутрішня водойма)
   [b] die See, -, -n — море (відкрита вода)
   [c] обидва (дві окремі картки)

2. "übersetzen" — два різних дієслова:
   [a] über|setzen (fuhr über, ist übergesetzt) — перевезти через воду/кордон
   [b] übersetzen (übersetzte, hat übersetzt) — перекладати текст

3. "іти" — яке значення руху?
   [a] gehen (ging, ist gegangen) — іти пішки
   [b] fahren (fuhr, ist gefahren) — їхати транспортом
   [c] laufen (lief, ist gelaufen) — бігти / іти (розм.)
   [d] усі три (три окремі картки)
```

User replies in one message:
```
1. b
2. a
3. a, c
```

Then append resolved cards, re-sort, output final complete CSV.

---

## Built-in documentation block

The Artifact includes a collapsible "Як читати картки" section with:
- Full symbol legend
- One example per PoS
- Conjunction type table with word-order explanation in Ukrainian
