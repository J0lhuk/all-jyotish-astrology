# All Jyotish Astrology — calculations & API reference

**A math-first Vedic astrology (Jyotiṣa) engine.** This repository is
**documentation only** — it describes *everything the engine calculates* and the
*complete HTTP API* for calling it. There is **no source code** here; the engine
itself is a separate private project.

**Design rule: compute what is algorithmic; never fabricate what isn't.** Where a
classical rule cannot be sourced cleanly, the engine says so (see *Boundaries*)
rather than inventing numbers or tables.

> **Document format.** The **first half of this document is entirely in English**;
> the **second half is the same content entirely in Russian** (jump to
> [🇷🇺 Русская версия](#-русская-версия)). The repository is maintained this way
> going forward — English first, then a full Russian mirror.

---
---

# 🇬🇧 English version

## Table of contents

- [Astronomical foundation](#astronomical-foundation)
- [What is calculated — by school](#what-is-calculated--by-school)
  1. [Core chart](#1-core-chart)
  2. [Parāśara (BPHS)](#2-parāśara-bphs)
  3. [Jaimini](#3-jaimini)
  4. [Krishnamurti Paddhati (KP)](#4-krishnamurti-paddhati-kp)
  5. [Aṣṭakavarga](#5-aṣṭakavarga)
  6. [Tājika / annual (Varṣaphala)](#6-tājika--annual-varṣaphala)
  7. [Daśā systems](#7-daśā-systems)
  8. [Transits & timing](#8-transits--timing)
  9. [Longevity & mundane](#9-longevity--mundane)
  10. [Praśna & muhūrta](#10-praśna--muhūrta)
  11. [Doṣas, remedies, folk & compatibility](#11-doṣas-remedies-folk--compatibility)
  12. [Lāl Kitāb, BNN, Nāḍī, rarities](#12-lāl-kitāb-bnn-nāḍī-rarities)
  13. [Cross-school verdict](#13-cross-school-verdict)
- [HTTP API reference](#http-api-reference)
- [Boundaries](#boundaries)
- [Disclaimer](#disclaimer)

---

## Astronomical foundation

| Aspect | Choice |
|---|---|
| Ephemeris | Swiss Ephemeris, **Moshier** analytical mode — no data files; ~0.1″ planets, ~0.3″ Moon |
| Zodiac | **Sidereal**; ayanāṁśa: **Lahiri** (default), **Krishnamurti** (KP), **Raman** |
| Houses | **Whole-sign** (default) · **Placidus** (KP) · **Porphyry/Śrīpati** (bhāva-chalit) |
| Time | IANA timezone auto-detected from lat/lon with the **historical** offset (DST-correct for the birth date); `LMT` for pre-standard-time births |
| Sunrise | Visible sunrise (upper limb + refraction, drik-pañcāṅga standard); matches published pañcāṅgas to seconds |
| Moon | Geocentric by default; **topocentric** Moon available (observer-parallax corrected, up to ~1° shift) |
| Nodes | Rāhu–Ketu axis, exactly 180° apart (mean nodes) |

---

## What is calculated — by school

### 1. Core chart
- Sidereal longitude, sign, **nakṣatra + pāda**, and whole-sign house of the 7 grahas + Rāhu/Ketu; retrogression and speed.
- **Ascendant (lagna)**, its sign and lord; house cusps in the chosen house system.
- Full **pañcāṅga**: tithi, nakṣatra, yoga, karaṇa, vāra (weekday from the *governing sunrise* of the local civil day).
- **Topocentric Moon** longitude (parallax-corrected for the birthplace) as an alternative to the geocentric position.

### 2. Parāśara (BPHS)
- **Divisional charts (vargas):** D1, D2 (horā), D3 (drekkāṇa), D4 (chaturthāṁśa), D6 (ṣaṣṭhāṁśa), D7 (saptāṁśa), D8 (aṣṭāṁśa), D9 (navāṁśa), D10 (daśāṁśa), D12 (dvādaśāṁśa), D16, D20, D24, D27, D30 (triṁśāṁśa), D40, D45, D60 (ṣaṣṭyāṁśa), plus D150 and D300 for fine work.
- **Ṣaḍbala** — all six strengths per graha: sthāna, dig, kāla (incl. **naisargika, ayana, hora, vāra, māsa, cheṣṭā, tribhāga, yuddha** components), and their totals in rūpas, with the classical minima and rank.
- **Bhāva-bala** — house strength (occupant/aspect, lord, and digbala contributions).
- **Iṣṭa / kaṣṭa phala** — benefic vs malefic capacity of each graha.
- **Sphuṭas** — fertility: **Bīja** (Sun+Venus+Jupiter) and **Kṣetra** (Moon+Mars+Jupiter) with odd/even rāśi+navāṁśa judgement; longevity/praśna: **Trisphuṭa, Chatuḥsphuṭa, Pañcasphuṭa, Prāṇa, Deha, Mṛtyu** sphuṭas (Praśna Marga VI, verified vs the source's worked example).
- **Planetary horas** — the 24 hour-lords (weekday lord first, Chaldean order), for electional use.
- **True node** — optional true (osculating) Rāhu/Ketu alongside the default mean nodes.
- **Ṣaṣṭyāṁśa (D60) named aṁśas** — the BPHS 60-part deity/nature per graha.
- **Vimśopaka bala** — weighted dignity across a varga group.
- **Avasthās** — bālādi, jāgradādi, dīptādi, and combustion (astaṅgata) states.
- **Yogas** — a large catalogue (rāja, dhana, daridra, nābhasa, lunar, solar, and named yogas from Phaladīpikā / Sārāvalī / Sarvārtha Cintāmaṇi), each with a score and the exact planetary reason.
- **Graha-yuddha** (planetary war), **upagrahas** (Gulika/Māndi & shadow-points), **curses (śāpa)** indications.
- **Bhāva-chalit** (Śrīpati/Porphyry cusp chart) and **Sudarśana chakra** (lagna + Moon + Sun tri-wheel).

### 3. Jaimini
- **Chara kārakas** (7- and 8-kāraka schemes) → **Ātmakāraka**.
- **Kārakāṁśa** and the **Kārakāṁśa lagna** analysis.
- **Arūḍha padas** for all houses; **Upapada (UL)** for marriage.
- **Argala** (intervention) and **rāśi-dṛṣṭi** (sign aspects).
- **Special lagnas** (Ārūḍha, Hora, Ghati, etc.).
- **Jaimini rāja-yogas** — AK–AmK sambandha (by rāśi-dṛṣṭi or conjunction), Kārakāṁśa rāja-yogas.
- **Death significators** (Brahma / Maheśvara / Rudra) for longevity work.
- **Jaimini rāśi daśās** — **Sthira** (fixed 7/8/9-yr signs from Brahma's rāśi), **Thrikona** (from the strongest trine, trine-triad order), **Brahma** (lagna-parity start, span to the 6th-lord's placement); **Yogardha** (½·[Chara+Sthira]), **Varnada** (Varṇada-lagna per bhāva), **Drig** (Jagannath/Rao — 9th/10th/11th rāśi-dṛṣṭi sets), **Lagna-Kendrādi** & **Sudasa** (wealth, from **Sree Lagna**; Rao), **Mandooka** (+3 frog-jump, BPHS) and **Karaka daśā** (kāraka graha daśā, BPHS); plus Chara / Nārāyaṇa / Śūla.

### 4. Krishnamurti Paddhati (KP)
- KP chart on **Placidus cusps** with the **Krishnamurti ayanāṁśa**.
- **Star-lord / sub-lord / sub-sub-lord** for every cusp and planet (the 249-fold division).
- **Significators** of each house, ranked; **Ruling Planets** for a moment.
- Event verdicts (marriage, career, wealth, children, health, foreign travel) via cusp sub-lord significations and dāśā-bhukti confirmation.
- **KP horary (1–249)** — a querent number seeds the ascendant sub-lord.

### 5. Aṣṭakavarga
- **Bhinnāṣṭakavarga (BAV)** for each graha and **Sarvāṣṭakavarga (SAV)** (the 337-bindu map).
- **Śodhya piṇḍa** (rāśi + graha reductions) — the refined strength residue.
- **Kakṣyā** transit gating for timing.

### 6. Tājika / annual (Varṣaphala)
- **Varṣa (annual) chart** for any year; **Muntha**, **Varṣeśa** (year lord).
- **Muddā daśā** (annual sub-periods) and **Tithi-Praveśa** (lunar-return annual chart).
- **Sahams** (50+ Arabic-part-style sensitive points).
- **Tājika yogas** (Iṭṭhaśāla, Īśarāpha, etc.) and **Pañca-vargīya bala** (Vishwa bala for the year).

### 7. Daśā systems
- **Vimśottari** (up to 5 levels: mahā → antar → pratyantar → sūkṣma → prāṇa), with the current running chain at *any* instant.
- **Yoginī**, **Aṣṭottarī**, **Kālachakra**, and minor **conditional** daśās.
- **Jaimini**: **Chara daśā** (Rao and Iranganti variants, with co-lords and unequal antardaśās), **Nārāyaṇa daśā**, **Niryāṇa Śūla daśā** (longevity), and **Sudarsana Chakra daśā** (1 house/year from the strongest of Lagna/Moon/Sun; Parasara/Rao).

### 8. Transits & timing
- **Gochara** (transits) over the natal chart, with **Vedha** (obstruction) and the **Aṣṭakavarga-gated** transit map.
- **Sāḍe-sātī** (Saturn's 7½ over the Moon) full passport; **Kaṇṭaka / Kaṇṭaka-śani**.
- **Kāla-sarpa** windows, **eclipse** proximity, **Bhṛgu-bindu**.
- **Sarvatobhadra Chakra (SBC)** with rāśi-ring and akṣara-vedha; **Kota chakra**; **Kūrma** directional map.
- A **personal calendar** — favourable/unfavourable day windows for a date range.

### 9. Longevity & mundane
- **Āyurdāya** — longevity by **four** classical methods, cross-checked: **Piṇḍāyu** (Raman, *How to Judge a Horoscope* vol. 2 — piṇḍa term scaled by arc from debilitation), **Aṁśāyu** (Satyacharya — navāṁśa-based, with Bharaṇa increases), **Naisargāyu** (Sāravalī 40.20 — fixed natural years Sun..Saturn 20/1/2/9/18/20/50, sum 120), and **Jaimini āyus** (Studies in Jaimini art.103-108 — Chara/Sthira/Dual natures of three factor-pairs); each with the three **Haraṇas** (Chakrapātha, Śatrukṣetra, Astaṅgata). **Ethics:** the engine returns **only a coarse band** — *alpa / madhya / pūrṇa* — plus the **maraka (killer) lords**, and never a date or age of death.
- **Mūrti-nirṇaya** (transit form) — each transiting graha's **Suvarṇa / Rajata / Tāmra / Loha** mūrti from its house from the natal Moon (1/6/11 gold, 2/5/9 silver, 3/7/10 copper, 4/8/12 iron), rating the transit's quality (Muhūrta-Chintāmaṇi tradition).
- **Medini (mundane) weather** — the **Tri-Nāḍī chakra** (27 nakṣatras in Heaven / Earth / Pātāla by a verified boustrophedon-of-three), reading rainfall from where benefics vs malefics fall; and the **Sun–Moon nakṣatra owner + gender** rainfall rule. The **Saptanāḍī** chakra (7 nadis, Krittika boustrophedon — verified vs the source figure) groups the grahas and flags rainfall vedha.

### 10. Praśna & muhūrta
- **Classical praśna (1–108)** — the seed fixes the ascendant to a navāṁśa-pāda; houses judged for the question type.
- **KP horary (1–249)** — sub-lord method (see §4).
- **Aṣṭamaṅgala-praśna** (Kerala deva-praśna, numeric core, *Prasna Marga* ch. VII): the querent's three-digit **Aṣṭamaṅgala number** divided by 30, 27, 7, 12, 9, 5 yields the tithi, nakṣatra, weekday, rāśi, planet and element; its digits map to future / present / past; results are reckoned from the Janma rāśi & nakṣatra. (The cowrie/gold-coin *rite* is not computable — the number is the input.)
- **Muhūrta** — pañcāṅga-śuddhi plus the **21 mahā-doṣas** (Viṣṭi, Pañcaka, gaṇḍānta, Gaṇḍa-mūla, dagdha-tithi, and the rest) for choosing an auspicious time.
- **Rectification** — birth-time refinement within a stated bracket.

### 11. Doṣas, remedies, folk & compatibility
- **Doṣa panel** — Maṅgala (Kuja), Kāla-sarpa, Sāḍe-sātī, gaṇḍānta, and the common afflictions.
- **Upāya engine** — gemstones, mantras, and remedial measures keyed to the weak/afflicted graha.
- **Points** — MKS (mṛtyu-bhāga type sensitive degrees), Puṣkara navāṁśa / bhāga.
- **Folk panels** — Diśā-śūla (direction of the day), Choghaḍiyā, name (nāmakaraṇa) letter, **Pañca-pakṣī** (five-bird timing: birth bird, the full weekday × pañca-yama activity grid, apahāra sub-birds, friend/enemy, power factor).
- **Compatibility** — the 8-fold **Aṣṭa-kūṭa (Guṇa Milan)** and South-Indian **Dasa-Porutham** (10-fold), including the zig-zag **Rajju**, the **Vedha** star-pairs, and **Stree-Dīrgha**.

### 12. Lāl Kitāb, BNN, Nāḍī, rarities
- **Lāl Kitāb** — ancestral debts (ṛṇa), sleeping/awake planets, blind/one-eyed houses, and its own remedies.
- **Bhṛgu-Nandi-Nāḍī (BNN)** — kāraka-chain progression reading.
- **Nāḍī** — nāḍī-aṁśa (150-fold) naming and pattern flags.
- **Rarities:** **Tri-Pataki chakra** (annual vedha map, K.S. Charak — Moon %9, pañcaka %4, Mars/nodes %6 with the nodes reversed, read as the three kendra rings); **Stri-Jātaka** (female horoscopy, BPHS ch. 80 — Lagna/Moon parity character rule and houses 1/4/5/7/8 from the stronger of Lagna or Moon). *(Mūrti-nirṇaya is intentionally absent — no clean source; see Boundaries.)*

### 13. Cross-school verdict
Instead of a pile of per-school facts, the engine delivers a **single verdict per
life area** (career, marriage, wealth, children, health, education, property,
spirituality): one score, a **confidence grade**, and the exact contributing
reasons drawn from every school above.

---

## HTTP API reference

Base: `https://<host>`  ·  JSON in/out  ·  auth via API key header where enabled
·  rate-limited.

| Method & path | Purpose | Key request fields |
|---|---|---|
| `GET /api/health` | Liveness probe | — |
| `POST /api/geocode` | Place → lat/lon/timezone | `place` |
| `POST /api/natal` | Full natal payload (all schools) | `date`, `time`, `lat`, `lon`, `place?`, `ayanamsa?`, `house_system?` |
| `POST /api/varsha` | Annual (Varṣaphala) chart | natal fields + `year` |
| `POST /api/prashna` | Praśna / horary | `system` (`classical`\|`kp`), `seed`, `moment`, `question` |
| `POST /api/compatibility` | Aṣṭa-kūṭa + Porutham | two birth-data blocks |
| `POST /api/muhurta` | Muhūrta scan + mahā-doṣas | window + place |
| `POST /api/rectify` | Birth-time rectification | natal fields + bracket + events |
| `POST /api/calendar` | Personal day windows | natal fields + date range |
| `POST /api/verdict` | Cross-school verdict per area | natal fields + `topic?` |
| `POST /api/explain` · `POST /api/reading` | Natural-language explanation (LLM) | computed facts + focus |
| `GET /api/asta` · `GET /api/yogas` | Direct sub-reports | natal fields |

**`POST /api/natal` response** is grouped by school:
`core`, `parashara`, `jaimini`, `kp`, `ashtakavarga`, `nadi`, `lal_kitab`,
`transits`, `doshas`, `points`, `chalit`, `sudarshana`, `eclipses`, `upaya`,
`bhrigu_bindu`, `pancha_pakshi`, `synthesis`, `overview`, `narrative`.
The `parashara` block now also carries `ayurdaya`, `medini`, `stri_jataka`,
`raja_yogas`, `topocentric_moon`, and `bhava_bala`. Responses are cached with a
schema version so a calculation change invalidates stale cache entries.

---

## Boundaries

The engine **declares** what it does not compute rather than guessing:

- **Āyurdāya returns a band only** (*alpa / madhya / pūrṇa*) + maraka lords —
  never a date or age of death. The classical methods (Piṇḍa / Aṁśa / Nisarga)
  disagree, so the band is a traditional indication, not a prediction.
- **Aṣṭamaṅgala rite — not computed.** The cowrie/gold-coin divination is a
  physical rite; only its numeric (Saṅkhya) core is computed, from the number the
  querent supplies.
- **Dasa-Porutham Vedha** — the five stars with no listed partner simply *pass*
  (a partner-less star has no vedha); this is stated, not treated as missing data.
- **Phala-Śūla daśā — not implemented.** It exists to predict the *age of death*
  (54–63); withheld for the same reason Āyurdāya is band-only.
- **"Navāṁśa / Nakshatra daśā" — not distinct systems.** Confirmed against JHora's
  own dasa catalogue: "nakshatra daśā" is a *class* (Vimśottari, Aṣṭottarī,
  Kālachakra), and there is no separate constructible Navāṁśa/Nakshatra rāśi daśā. Twelve Jaimini rāśi daśās
  *are* implemented (Chara, Sthira, Thrikona, Brahma, Yogardha, Varnada, Drig,
  Sudasa, Lagna-Kendrādi, Mandooka, Nārāyaṇa, Niryāṇa-Śūla) plus Karaka graha daśā.

---

## Disclaimer

This engine is for study and reflection. Jyotiṣa is a traditional symbolic
system, not a science of certainty. Nothing here is medical, legal, financial, or
psychological advice. Longevity output is a coarse traditional band, never a
prediction of death.

---
---

# 🇷🇺 Русская версия

**Математическая ведическая астрология (Джйотиш).** Этот репозиторий —
**только документация**: описывает *всё, что движок рассчитывает*, и *полный
HTTP API* для вызова. Исходного кода здесь **нет** — сам движок в отдельном
приватном проекте.

**Принцип: считать то, что алгоритмично; никогда не выдумывать то, что нет.**
Где классическое правило нельзя чисто подтвердить источником, движок так и
говорит (см. *Границы*), а не изобретает числа или таблицы.

> **Формат документа.** Первая половина — целиком на английском; вторая
> половина (ниже) — то же содержание целиком на русском. Репозиторий и дальше
> ведётся так: сначала английский, затем полная русская копия.

## Содержание

- [Астрономическая основа](#астрономическая-основа)
- [Что рассчитывается — по школам](#что-рассчитывается--по-школам)
- [Справочник HTTP API](#справочник-http-api)
- [Границы честности](#границы-честности)
- [Отказ от ответственности](#отказ-от-ответственности)

---

## Астрономическая основа

| Аспект | Выбор |
|---|---|
| Эфемериды | Swiss Ephemeris, аналитический режим **Moshier** — без файлов данных; ~0.1″ планеты, ~0.3″ Луна |
| Зодиак | **Сидерический**; аянамша: **Лахири** (по умолч.), **Кришнамурти** (KP), **Раман** |
| Дома | **Знаковые (whole-sign)** по умолч. · **Плацидус** (KP) · **Порфирий/Шрипати** (бхава-чалит) |
| Время | Часовой пояс IANA автоматически по широте/долготе с **историческим** смещением (корректно с летним временем на дату рождения); `LMT` для рождений до введения поясов |
| Восход | Видимый восход (верхний край + рефракция, стандарт дрик-панчанга); совпадает с изданными панчангами до секунд |
| Луна | По умолчанию геоцентрическая; доступна **топоцентрическая** Луна (с параллаксом наблюдателя, сдвиг до ~1°) |
| Узлы | Ось Раху–Кету, ровно 180° (средние узлы) |

---

## Что рассчитывается — по школам

### 1. Базовая карта
- Сидерическая долгота, знак, **накшатра + пада**, знаковый дом 7 грах + Раху/Кету; ретроградность и скорость.
- **Лагна (асцендент)**, её знак и владыка; куспиды в выбранной системе домов.
- Полный **панчанг**: титхи, накшатра, йога, карана, вара (день недели по *управляющему восходу* локального гражданского дня).
- **Топоцентрическая Луна** (с поправкой на параллакс места рождения) как альтернатива геоцентрической позиции.

### 2. Парашара (BPHS)
- **Дробные карты (варги):** D1, D2 (хора), D3 (дреккана), D4, D6 (шаштамша), D7, D8 (аштамша), D9 (навамша), D10, D12, D16, D20, D24, D27, D30, D40, D45, D60 (шаштьямша), плюс D150 и D300 для тонкой работы.
- **Шадбала** — все шесть сил каждой грахи: стхана, диг, кала (включая **найсаргика, аяна, хора, вара, маса, чешта, трибхага, юддха**), итоги в рупах, классические минимумы и ранг.
- **Бхава-бала** — сила домов (обитатели/аспекты, владыка, дигбала).
- **Ишта / кашта пхала** — благая vs вредная способность каждой грахи.
- **Спхуты** — плодородие: **Биджа** (Солнце+Венера+Юпитер) и **Кшетра** (Луна+Марс+Юпитер) с оценкой по нечёт/чёт раши+навамше; долголетие/прашна: **Тришпхута, Чатухспхута, Панчаспхута, Прана, Деха, Мритью** (Prasna Marga VI, сверено с примером источника).
- **Планетные хоры** — 24 владыки часов (владыка дня первым, халдейский порядок), для электива.
- **True-node** — опц. истинные (оскулирующие) Раху/Кету рядом со средними по умолчанию.
- **Шаштьямша (D60), именованные амши** — 60-частное божество/природа по BPHS.
- **Вимшопака-бала** — взвешенное достоинство по группе варг.
- **Авастхи** — баладди, джаградади, диптади и сожжение (астангата).
- **Йоги** — большой каталог (раджа, дхана, даридра, набхаса, лунные, солнечные и именные из Пхаладипики / Саравали / Сарвартха Чинтамани), с оценкой и точной планетной причиной.
- **Граха-юддха** (планетная война), **упаграхи** (Гулика/Манди и теневые точки), указания на **проклятия (шапа)**.
- **Бхава-чалит** (карта куспидов Шрипати/Порфирия) и **Сударшана-чакра** (три-круг Лагна + Луна + Солнце).

### 3. Джаймини
- **Чара-караки** (7- и 8-каракные схемы) → **Атмакарака**.
- **Каракамша** и анализ **лагны Каракамши**.
- **Аруда-пады** всех домов; **Упапада (UL)** для брака.
- **Аргала** (вмешательство) и **раши-дришти** (аспекты знаков).
- **Спец-лагны** (Аруда, Хора, Гхати и др.).
- **Раджа-йоги Джаймини** — самбандха АК–АмК (по раши-дришти или соединению), раджа-йоги Каракамши.
- **Сигнификаторы смерти** (Брахма / Махешвара / Рудра) для работы с долголетием.
- **Джаймини раши-даши** — **Стхира** (фикс. 7/8/9 лет по природе знака, от раши Брахмы), **Трикона** (от сильнейшей триконы, тригональный порядок), **Брахма** (старт по чётности лагны, длительность до места 6-го лорда); **Йогардха** (½·[Чара+Стхира]), **Варнада** (Варнада-лагна по бхаве), **Дриг** (Джаганнатх/Рао — наборы раши-дришти 9/10/11), **Лагна-Кендради** и **Судаса** (богатство, от **Шри-Лагны**; Рао), **Мандука** (скачок +3, BPHS) и **Карака-даша** (грах-даша карак, BPHS); плюс Чара / Нараяна / Шула.

### 4. Кришнамурти Паддхати (KP)
- KP-карта на **куспидах Плацидуса** с **аянамшей Кришнамурти**.
- **Звёздный владыка / саблорд / саб-саблорд** для каждого куспида и планеты (249-частное деление).
- **Сигнификаторы** каждого дома, ранжированные; **Управляющие планеты** момента.
- Вердикты по событиям (брак, карьера, богатство, дети, здоровье, заграница) через значения саблордов куспидов и подтверждение даша-бхукти.
- **KP-хорар (1–249)** — число вопрошающего задаёт саблорд асцендента.

### 5. Аштакаварга
- **Бхиннаштакаварга (BAV)** каждой грахи и **Сарваштакаварга (SAV)** (карта из 337 бинду).
- **Шодхья-пинда** (раши + граха редукции) — очищенный остаток силы.
- **Какшья** — гейтинг транзитов для тайминга.

### 6. Таджика / годовые (Варшапхала)
- **Годовая (варша) карта** на любой год; **Мунтха**, **Варшеша** (владыка года).
- **Мудда-даша** (годовые под-периоды) и **Титхи-Правеша** (годовая карта лунного возвращения).
- **Сахамы** (50+ чувствительных точек типа арабских).
- **Таджика-йоги** (Итташала, Ишарапха и др.) и **Панча-варгия-бала** (Вишва-бала на год).

### 7. Системы даш
- **Вимшоттари** (до 5 уровней: маха → антар → пратьянтар → сукшма → прана), с текущей действующей цепочкой на *любой* момент.
- **Йогини**, **Аштоттари**, **Калачакра** и малые **условные** даши.
- **Джаймини**: **Чара-даша** (варианты Рао и Иранганти, с со-владыками и неравными антардашами), **Нараяна-даша**, **Ниряна-Шула-даша** (долголетие) и **Сударшана-чакра-даша** (1 дом/год от сильнейшего Лагна/Луна/Солнце; Парашара/Рао).

### 8. Транзиты и тайминг
- **Гочара** (транзиты) по натальной карте, с **Ведхой** (обструкцией) и картой транзитов с **гейтингом по Аштакаварге**.
- Полный паспорт **Саде-сати** (7½ Сатурна над Луной); **Кантака / Кантака-шани**.
- Окна **Кала-сарпа**, близость **затмений**, **Бхригу-бинду**.
- **Сарватобхадра-чакра (SBC)** с раши-кольцом и акшара-ведхой; **Кота-чакра**; направленная карта **Курма**.
- **Персональный календарь** — благо/неблагоприятные окна дней на диапазон дат.

### 9. Долголетие и мундан
- **Аюрдая** — долголетие **четырьмя** классическими методами со сверкой: **Пиндаю** (Raman, т.2 — пинда-срок, масштаб дугой от дебилитации), **Амшаю** (Сатьячарья — по навамше, с бхарана-увеличениями) **Нисаргаю** (Saravali 40.20 — фикс. годы 20/1/2/9/18/20/50, сумма 120) и **Джаймини-аюс** (Studies in Jaimini 103-108 — природы Чара/Стхира/Двисвабхава трёх пар); каждый с тремя **Харанами**. **Этика:** движок выдаёт **только грубый диапазон** *альпа/мадхья/пурна* + **мараки**, и никогда — дату или возраст смерти.
- **Мурти-нирная** (транзитная) — мурти каждой транзитной грахи **Сварна/Раджата/Тамра/Лоха** по дому от натальной Луны (1/6/11 золото, 2/5/9 серебро, 3/7/10 медь, 4/8/12 железо), оценивает качество транзита (традиция Muhurta Chintamani).
- **Медини (мундан), погода** — **Три-Нади-чакра** (27 накшатр в Небо / Земля / Патала бустрофедоном-по-3, проверено) читает дожди по тому, где благодетели vs вредители; и правило дождя по **владыке+гендеру накшатр Солнца и Луны**. **Саптанади** (7 нади, бустрофедон от Криттики — сверено с фигурой источника) группирует грахи и отмечает дождевую ведху.

### 10. Прашна и мухурта
- **Классическая прашна (1–108)** — зерно фиксирует асцендент на паде навамши; дома судятся по типу вопроса.
- **KP-хорар (1–249)** — метод саблордов (см. §4).
- **Аштамангала-прашна** (керальская дева-прашна, числовое ядро, *Prasna Marga* гл. VII): трёхзначное **аштамангала-число** вопрошающего, делённое на 30, 27, 7, 12, 9, 5, даёт титхи, накшатру, день недели, раши, планету и элемент; цифры → будущее / настоящее / прошлое; результаты отсчитываются от Джанма-раши и накшатры. (Обряд с раковинами/золотом невычислим — число даёт вопрошающий.)
- **Мухурта** — панчанга-шуддхи плюс **21 маха-доша** (Вишти, Панчака, ганданта, Ганда-мула, дагдха-титхи и др.) для выбора благоприятного времени.
- **Ректификация** — уточнение времени рождения в заданном интервале.

### 11. Доши, средства, быт и совместимость
- **Панель дош** — Мангала (Куджа), Кала-сарпа, Саде-сати, ганданта и частые поражения.
- **Движок упай** — камни, мантры и меры под слабую/поражённую граху.
- **Точки** — MKS (чувствительные градусы типа мритью-бхага), Пушкара-навамша / бхага.
- **Народные панели** — Диша-шула (направление дня), Чогхадия, буква имени (намкаран), **Панча-пакши** (тайминг пяти птиц: птица рождения, полная сетка день-недели × панча-яма активностей, апахара-подптицы, друг/враг, коэффициент силы).
- **Совместимость** — 8-частная **Ашта-кута (Гуна Милан)** и южноиндийский **Даса-Порутхам** (10-частный), включая зигзаг **Раджу**, звёздные пары **Ведха** и **Стри-Диргха**.

### 12. Лал Китаб, BNN, Нади, редкости
- **Лал Китаб** — родовые долги (рина), спящие/бодрствующие планеты, слепые/кривые дома и собственные средства.
- **Бхригу-Нанди-Нади (BNN)** — чтение по цепочке карак.
- **Нади** — именование нади-амши (150-частное) и флаги паттернов.
- **Редкости:** **Три-Патаки-чакра** (годовая карта ведхи, K.S. Charak — Луна %9, панчака %4, Марс/узлы %6 с обратным счётом узлов, читается как три кольца кендр); **Стри-джатака** (женская хорография, BPHS гл. 80 — правило характера по чётности Лагна/Луна и дома 1/4/5/7/8 от сильнейшего из Лагны/Луны). *(Мурти-нирная намеренно отсутствует — нет чистого источника; см. Границы.)*

### 13. Кросс-школьный вердикт
Вместо кучи фактов по школам движок даёт **единый вердикт по сфере жизни**
(карьера, брак, богатство, дети, здоровье, образование, недвижимость,
духовность): одна оценка, **грейд уверенности** и точные причины из всех школ
выше.

---

## Справочник HTTP API

База: `https://<host>` · JSON вход/выход · авторизация по заголовку с API-ключом
там, где включена · с рейт-лимитом.

| Метод и путь | Назначение | Ключевые поля запроса |
|---|---|---|
| `GET /api/health` | Проверка живости | — |
| `POST /api/geocode` | Место → широта/долгота/пояс | `place` |
| `POST /api/natal` | Полный натальный ответ (все школы) | `date`, `time`, `lat`, `lon`, `place?`, `ayanamsa?`, `house_system?` |
| `POST /api/varsha` | Годовая (Варшапхала) карта | натал-поля + `year` |
| `POST /api/prashna` | Прашна / хорар | `system` (`classical`\|`kp`), `seed`, `moment`, `question` |
| `POST /api/compatibility` | Ашта-кута + Порутхам | два блока данных рождения |
| `POST /api/muhurta` | Скан мухурты + маха-доши | окно + место |
| `POST /api/rectify` | Ректификация времени | натал-поля + интервал + события |
| `POST /api/calendar` | Персональные окна дней | натал-поля + диапазон дат |
| `POST /api/verdict` | Кросс-школьный вердикт по сфере | натал-поля + `topic?` |
| `POST /api/explain` · `POST /api/reading` | Объяснение обычным языком (LLM) | вычисленные факты + фокус |
| `GET /api/asta` · `GET /api/yogas` | Прямые под-отчёты | натал-поля |

**Ответ `POST /api/natal`** сгруппирован по школам:
`core`, `parashara`, `jaimini`, `kp`, `ashtakavarga`, `nadi`, `lal_kitab`,
`transits`, `doshas`, `points`, `chalit`, `sudarshana`, `eclipses`, `upaya`,
`bhrigu_bindu`, `pancha_pakshi`, `synthesis`, `overview`, `narrative`.
Блок `parashara` теперь также содержит `ayurdaya`, `medini`, `stri_jataka`,
`raja_yogas`, `topocentric_moon` и `bhava_bala`. Ответы кэшируются с версией
схемы, поэтому изменение расчёта инвалидирует устаревший кэш.

---

## Границы честности

Движок **объявляет**, что не считает, а не угадывает:

- **Аюрдая — только диапазон** (*альпа / мадхья / пурна*) + мараки — никогда
  дата или возраст смерти. Классические методы (Пинда / Амша / Нисарга)
  расходятся, поэтому диапазон — традиционная индикация, не предсказание.
- **Обряд аштамангала — не вычисляется.** Гадание на раковинах/золоте —
  физический обряд; считается только его числовое (санкхья) ядро из числа,
  которое даёт вопрошающий.
- **Ведха в Даса-Порутхам** — пять звёзд без пары просто *проходят* (у звезды без
  пары нет ведхи); это заявлено, а не считается пропуском данных.
- **Пхала-Шула-даша — не реализована.** Она предсказывает *возраст смерти* (54–63);
  не включена по той же причине, по которой аюрдая — только диапазон.
- **«Навамша / Накшатра-даша» — не отдельные системы.** Подтверждено по каталогу
  даш самой JHora: «накшатра-даша» — это *класс* (Вимшоттари, Аштоттари,
  Калачакра), отдельной вычислимой Навамша/Накшатра-раши-даши не существует. Реализованы 12 джаймини раши-даш
  (Чара, Стхира, Трикона, Брахма, Йогардха, Варнада, Дриг, Судаса, Лагна-Кендради,
  Мандука, Нараяна, Ниряна-Шула) плюс Карака грах-даша.

---

## Отказ от ответственности

Движок — для изучения и размышления. Джйотиш — традиционная символическая
система, не наука определённости. Ничто здесь не является медицинской,
юридической, финансовой или психологической консультацией. Вывод о долголетии —
грубый традиционный диапазон, никогда не предсказание смерти.
