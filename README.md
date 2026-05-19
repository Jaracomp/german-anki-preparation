<div align="center">

# 🇩🇪 german-anki-preparation

**AI skill for converting raw German/Ukrainian word lists into fully-formatted Anki CSV**

[![Version](https://img.shields.io/badge/version-1.0.0-blue?style=flat-square)](CHANGELOG.md)
[![Skill Format](https://img.shields.io/badge/skill--format-.skill-orange?style=flat-square)](#skill-format)
[![Language Pair](https://img.shields.io/badge/DE%20↔%20UA-German%20%2F%20Ukrainian-yellow?style=flat-square)](#)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)

[Про що цей скіл](#про-що-цей-скіл) · [Можливості](#можливості) · [Skill format](#skill-format) · [Структура файлів](#структура-файлів) · [Використання](#використання) · [Формат карток](#формат-карток) · [Символи](#легенда-символів)

</div>

---

## Про що цей скіл

**german-anki-preparation** — це `.skill`-файл для AI-агентів (Claude Code, OpenCode), що перетворює сирий список слів у готовий CSV для імпорту в [Anki](https://apps.ankiweb.net/).

Вхід — будь-який список: німецькою, українською або змішаний. Вихід — відформатований CSV з повною граматикою: артикль + відмінювання, форми дієслів, відміна прикметників, керування, ідіоми. Два-фазовий пайплайн: спочатку автоматична обробка, потім черга уточнень для неоднозначних випадків.

---

## Можливості

- **Детекція мови** — приймає DE, UA та мішаний список в одному запиті
- **Автонормалізація** — зводить `fuhr` → `fahren`, `schnellen` → `schnell`, `ruft an` → `an|rufen` автоматично, без питань
- **Повна граматика** — Nomen (артикль + Gen + Pl), Verb (Präteritum + Partizip II + Vokalwechsel), Adjektiv (Komparativ + Superlativ), Präposition, Konjunktion (3 типи), Modalpartikeln
- **Черга уточнень** — неоднозначні слова (омоніми з різним артиклем, trennbar vs. nicht-trennbar, UA слова з кількома перекладами) виносяться в окремий блок; одна відповідь `1. b`, `2. a, c` — і CSV доповнюється
- **Сортування за частотою** — картки впорядковані за корпусною частотністю; ідіоми і фрази завжди в кінці
- **Підказки `=`** — `See = озеро` знімає неоднозначність без питань
- **Видалення дублікатів** — мовчки, з нормалізацією форм
- **Вбудована документація** — Artifact містить розкривний блок "Як читати картки" з легендою символів і прикладами для кожної ЧМ

---

## Skill format

Цей репозиторій зберігає скіл у файлі **`SKILL.md`** (Markdown + YAML front-matter). Деякі рантайми/агенти очікують розширення **`.skill`** — у такому випадку:

- просто **перейменуйте** `SKILL.md` → `german-anki-preparation.skill`, або
- **зробіть копію** (щоб залишити `SKILL.md` як “читабельну” версію в репозиторії).

Вміст однаковий — різниться лише розширення файлу.

---

## Структура файлів

```
german-anki-preparation/
├── CHANGELOG.md                      # Історія змін (версії)
├── SKILL.md                          # Головний файл скіла — завантажується агентом
├── SPEC.md                           # Специфікація: формат вводу/виводу, пайплайн
└── references/
    ├── grammar-templates.md          # Граматичні шаблони з прикладами для кожної ЧМ
    └── ambiguity-handling.md         # Дерева рішень для неоднозначних випадків
```

> `SKILL.md` — єдиний файл, який потрібен агенту для роботи. Файли у `references/` завантажуються агентом автоматично при потребі.

## Використання

### Базовий запит

Після реєстрації скіла — просто вставте список слів:

```
зроби csv для anki:

Hund
fahren
schnell
weil
auf jeden Fall
See
іти
```

### Підказки для зняття неоднозначності

```
der See           ← явний артикль = без питань
See = озеро       ← переклад-підказка = без питань
```

### Відповідь на черзі уточнень

Агент поверне дві секції: готовий CSV і блок уточнень. Відповідайте одним повідомленням:

```
1. b
2. a
3. a, c
```

---

## Формат карток

Роздільник: **табуляція** (`\t`). Поле 1 — DE + граматика, поле 2 — переклад(и) українською.

| Частина мови | Поле 1 (DE) | Поле 2 (UA) |
|---|---|---|
| Nomen | `der Hund, -es, ¨-e` | `собака` |
| Verb (регулярний) | `kaufen (kaufte, hat gekauft)` | `купувати` |
| Verb (Vokalwechsel) | `fahren (fährt, fuhr, ist gefahren)` | `їхати` |
| Verb (trennbar) | `an\|rufen (rief an, hat angerufen)` | `дзвонити` |
| Verb (reflexiv) | `sich freuen (freute sich, hat sich gefreut)` | `радіти` |
| Verb (Rektion) | `warten (wartete, hat gewartet); auf +Akk` | `чекати (на когось/щось)` |
| Adjektiv | `schnell (schneller, am schnellsten)` | `швидкий` |
| Adjektiv (Rektion) | `stolz auf +Akk (stolzer, am stolzesten)` | `гордий (кимось/чимось)` |
| Adverb | `leider` | `на жаль` |
| Präposition | `mit +Dat` | `з (кимось/чимось)` |
| Wechselpräp. | `in +Dat/+Akk [wo/wohin]` | `в, у` |
| Konjunktion | `weil [sub]` | `тому що [дієслово — в кінець]` |
| Modalpartikel | `doch [partikel]<br>Das ist doch klar!` | `1) підкреслення очевидного: "Та це ж ясно!" 2) Doch! = "Та ні!"` |
| Phrase | `auf jeden Fall` | `у будь-якому разі` |
| Redewendung | `jemandem auf die Nerven gehen [Rdw]` | `діяти комусь на нерви` |

---

## Легенда символів

| Символ | Значення |
|--------|---------|
| `¨` | Умлаут у формі |
| `¨-e` | Умлаут + закінчення `-e` |
| `-` | Без змін / форма відсутня |
| `(pl)` | Pluralia tantum (тільки множина) |
| `\|` | Межа розділюваного дієслова |
| `sich` | Зворотне дієслово |
| `+Akk` / `+Dat` / `+Gen` | Відмінок після прийменника |
| `+Dat/+Akk` | Wechselpräposition |
| `[wo/wohin]` | Підказка для Wechselpräposition |
| `[koord]` | Координуючий сполучник |
| `[sub]` | Підрядний сполучник (дієслово в кінець) |
| `[adv]` | Прислівниковий сполучник (інверсія підмета) |
| `[partikel]` | Модальна частка |
| `[Rdw]` | Redewendung (ідіома) |
| `[ugs.]` | Розмовне слово |
| `etw.` | etwas — щось (Akk) |
| `<br>` | Перенос рядка в Anki-картці |

---

## Ліцензія

[MIT](LICENSE) — використовуйте, модифікуйте, поширюйте вільно.
