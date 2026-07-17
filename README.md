# All Jyotish Astrology — calculations & API reference

**A math-first Vedic astrology (Jyotiṣa) engine.** This repository is
**documentation only** — it describes *everything the engine calculates* and the
*complete HTTP API* for calling it. There is **no source code** here; the engine
itself is a separate private project.

**Design rule: compute what is algorithmic; never fabricate what isn't.** Every
table and formula below is traced to a named classical source and, wherever the
source prints a worked example, verified against it. Where a rule cannot be
sourced cleanly, the engine says so (see *Boundaries*) instead of inventing
numbers.

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
| Moon | Geocentric by default; **topocentric** Moon available (observer parallax, up to ~1° shift) |
| Nodes | Rāhu–Ketu axis exactly 180° apart; **mean** nodes by default, **true** (osculating) nodes optional |

---

## What is calculated — by school

### 1. Core chart

- Sidereal **longitude, sign, nakṣatra + pāda, whole-sign house** for the 7 grahas + Rāhu/Ketu; **retrogression** and daily speed.
- **Ascendant (lagna)** — longitude, sign, lord; house cusps in the chosen system.
- Full **pañcāṅga**: **tithi** (1–30 + nature), **nakṣatra** (+ lord, deity, gaṇa, nāḍī, yoni, pāda), **yoga** (27), **karaṇa** (11 across 60 half-tithis), **vāra** — the weekday taken from the *governing sunrise* of the local civil day, not from midnight.
- **Topocentric Moon** — parallax-corrected for the birthplace, offered as additional data for rectification and nakṣatra-boundary checks (classical Jyotiṣa is geocentric, so it is never the default chart value).
- **True nodes** — osculating Rāhu/Ketu alongside the default mean nodes.

### 2. Parāśara (BPHS)

**Divisional charts (vargas) — 20 total.** D1 (rāśi), D2 (horā), D3 (drekkāṇa),
D4 (chaturthāṁśa), D6 (ṣaṣṭhāṁśa), D7 (saptāṁśa), D8 (aṣṭāṁśa), D9 (navāṁśa),
D10 (daśāṁśa), D12 (dvādaśāṁśa), D16 (ṣoḍaśāṁśa), D20 (viṁśāṁśa), D24
(siddhāṁśa), D27 (bhāṁśa), D30 (triṁśāṁśa), D40 (khavedāṁśa), D45 (akṣavedāṁśa),
D60 (ṣaṣṭyāṁśa) — the complete **ṣoḍaśavarga** — plus **D150** (nāḍīāṁśa) and
**D300** for fine work. Each varga is built as a full chart (lagna + all grahas),
not just planet placements.

**Ṣaḍbala** — all six strengths per graha, in rūpas, with the classical minima and rank:
- **Sthāna-bala**: uccha-bala, saptavargaja-bala, oja-yugma-bala, kendrādi-bala, drekkāṇa-bala
- **Dig-bala** (directional strength)
- **Kāla-bala**: nathonnata, pakṣa, tribhāga, ayana, plus the year/month/day/hora lords and cheṣṭā (motional) and yuddha (planetary-war) components
- **Cheṣṭā-bala**, **Naisargika-bala**, **Dṛk-bala** (aspectual strength)

**Other strength & state measures:**
- **Bhāva-bala** — house strength (occupants, aspects, lord's strength, digbala contribution)
- **Iṣṭa / kaṣṭa phala** — each graha's benefic vs malefic capacity
- **Vimśopaka bala** — weighted dignity across a varga group
- **Avasthās** — bālādi (infant→old), jāgradādi (awake/dreaming/sleeping), dīptādi (12 states), and **astaṅgata** (combustion) with per-planet orbs
- **Ṣaṣṭyāṁśa (D60) named aṁśas** — the BPHS 60-part deity/nature per graha
- **Graha-yuddha** (planetary war) — winner/loser by longitude, with the effect on strength
- **Upagrahas** — the five kāla-vela (Kāla, Mṛtyu, Ardhaprahara, Yamaghaṇṭaka, **Gulika/Māndi**) by the rising method, plus the five Sun-derived shadow points (Dhūma, Vyatīpāta, Parivesha, Indrachāpa, Upaketu)
- **Śāpa (curses)** — indications with sphere and remedy

**Yogas** — a catalogue of **484 rules** (plus a 221-rule Adawal set and a
114-rule reference set), covering rāja, dhana, daridra, nābhasa, lunar, solar and
named yogas from Phaladīpikā, Sārāvalī and Sarvārtha Cintāmaṇi. Each detection
returns the exact planetary reason and a strength score, not just a name.

**Chart variants:**
- **Bhāva-chalit** — Śrīpati/Porphyry cusp chart, showing planets that shift house
- **Sudarśana chakra** — the lagna + Moon + Sun tri-wheel, with a per-bhāva consensus across all three vantage points
- **Bhāvat-bhāvam** — the house-from-house derivative reading
- **Sphuṭas** — fertility: **Bīja** (Sun+Venus+Jupiter) and **Kṣetra** (Moon+Mars+Jupiter), judged by odd/even rāśi + navāṁśa; longevity/praśna: **Trisphuṭa** (lagna+Moon+Gulika), **Chatuḥsphuṭa** (+Sun), **Pañcasphuṭa** (+Rāhu), **Prāṇa** (lagna×5+Gulika), **Deha** (Moon×8+Gulika), **Mṛtyu** (Gulika×7+Sun) — Praśna Marga VI, verified against the source's worked example
- **Planetary horas** — the 24 hour-lords (day/night split into 12 each; first hora = weekday lord, then Chaldean order), for electional use

### 3. Jaimini

- **Chara kārakas** — 7- and 8-kāraka schemes → **Ātmakāraka** (AK), Amātya (AmK), Bhrātṛ, Mātṛ, Pitṛ, Putra, Jñāti, Dāra
- **Kārakāṁśa** and the **Kārakāṁśa lagna** analysis
- **Arūḍha padas** for all 12 houses; **Upapada (UL)** for marriage
- **Argala** (intervention) with **Virodhārgala** (blocking) resolution
- **Rāśi-dṛṣṭi** — sign aspects (movable↔fixed except adjacent; dual↔dual)
- **Special lagnas** — Bhāva, Horā, Ghaṭikā, Prāṇapada, Vighaṭi, Kuṇḍa, Indu, **Varṇada**, **Sree Lagna** (lagna + Moon's nakṣatra-fraction × 360°, verified against the source example)
- **Jaimini rāja-yogas** — AK–AmK sambandha (by rāśi-dṛṣṭi or conjunction), Kārakāṁśa rāja-yogas
- **Death significators** — Brahma / Maheśvara / Rudra
- **Dvāra / Bāhya (Paka / Bhoga) rāśis** — for longevity work

**Jaimini rāśi daśās — 12 systems, each from a named source:**

| Daśā | Seed | Order | Span |
|---|---|---|---|
| **Chara** | Lagna | zodiacal by parity | count sign→lord (Rao & Iranganti variants, co-lords, unequal antardaśās) |
| **Sthira** | rāśi of **Brahma** | regular zodiacal | fixed 7 / 8 / 9 yrs (movable / fixed / dual); cycle = 96 yrs |
| **Thrikona** | strongest of Lagna / 5th / 9th | trine triads (1,5,9 · 2,6,10 · 3,7,11 · 4,8,12) | Chara-style count to lord |
| **Brahma** | Brahma's rāśi (odd lagna) or the 7th from it (even) | direct / reverse | count to the placement of that rāśi's 6th lord, −1 |
| **Yogardha** | stronger of Lagna / 7th | sequential by parity | ½·(Chara + Sthira) |
| **Varnada** | Lagna (odd) or the 12th (even) | direct / reverse | count anchor → that bhāva's Varṇada |
| **Drig** | 9th house | three sets seeded by the 9th/10th/11th + their 3 rāśi-dṛṣṭi aspects | Nārāyaṇa spans |
| **Sudasa** (wealth) | **Sree Lagna** | kendra → paṇaphara → apoklima | Nārāyaṇa spans; Saturn-in-seed → forward, Ketu → reversed |
| **Lagna-Kendrādi** | stronger of Lagna / 7th | kendra → paṇaphara → apoklima | Nārāyaṇa spans, same Saturn/Ketu overrides |
| **Mandooka** | stronger of Lagna / 7th | **+3 frog-jumps** ("next 3rd sign, leaving out two") | Sthira 7/8/9 yrs |
| **Nārāyaṇa** (Padakrama) | stronger of Lagna / 7th | zodiacal by parity | count to lord with exalt/debil tweaks |
| **Niryāṇa Śūla** | — | — | 9-yr rāśi daśās, for longevity work |

**Jaimini graha daśās:**
- **Karaka daśā** — Ātmakāraka first, then the remaining kārakas in order; span = signs counted from the Lagna to that kāraka's sign (0 → 12 yrs). BPHS ch.46-178, verified against the book's illustrative table.
- **Mūla daśā** (Lagna Kendrādi Graha daśā) — a Vimśottari derivative. Span = Vimśottari years − (forward count natal sign → mūlatrikoṇa sign, −1); +1 exalted, −1 debilitated; 0 → full years; negative → absolute value. Rāhu reckoned to Aquarius, Ketu to Scorpio. Order: kendra planets first by sign strength, then paṇaphara/apoklima by lagna parity, with Saturn-in-lagna → paṇaphara-first and Ketu-in-lagna → apoklima-first overrides. Verified on the source's worked example (Venus in Gemini → 16 yrs).

### 4. Krishnamurti Paddhati (KP)

- KP chart on **Placidus cusps** with the **Krishnamurti ayanāṁśa**
- **Star-lord / sub-lord / sub-sub-lord** for every cusp and every planet — the full **249-fold** division of the zodiac
- **Significators** of each house, ranked by the classical 4-level hierarchy
- **Ruling Planets (RP)** for any moment
- **Event verdicts** — marriage, career, wealth, children, health, foreign travel — read from cusp sub-lord significations and confirmed against the running daśā-bhukti
- **KP horary (1–249)** — the querent's number seeds the ascendant sub-lord; judged on a real Placidus chart for the question moment

### 5. Aṣṭakavarga

- **Bhinnāṣṭakavarga (BAV)** — the bindu map for each of the 7 grahas
- **Sarvāṣṭakavarga (SAV)** — the combined 337-bindu map across the 12 signs
- **Trikoṇa śodhana** and **Ekādhipatya śodhana** — both reduction stages, applied in order
- **Śodhya piṇḍa** — rāśi-piṇḍa + graha-piṇḍa → the refined strength residue per graha
- **Kakṣyā** — the eight-fold sub-division used to gate transit timing

### 6. Tājika / annual (Varṣaphala)

- **Varṣa (annual) chart** for any year — solar-return based, with the exact return instant
- **Muntha** — the progressed point; **Varṣeśa** — the year lord (by the classical 5-fold contest)
- **Muddā daśā** — annual sub-periods scaled to the year
- **Tithi-Praveśa** — the lunar-return annual chart (an alternative to the solar return)
- **Sahams** — **50** Arabic-part-style sensitive points (Puṇya, Yaśas, Vidyā, Rāja, Mātṛ, Pitṛ, Putra, Jīva, Karma, Roga, Mṛtyu, … ) with the day/night birth inversion
- **Tājika yogas** — Iṭṭhaśāla (applying), Īśarāpha (separating), Nakta, Yamayā, Kambūla, and the rest of the 16-fold set, computed from **Tājika aspects with orbs (deeptāṁśa)**
- **Harṣa bala** and **Pañca-vargīya bala** (Vishwa bala) for the year's chart

### 7. Daśā systems

**Nakṣatra daśās (Moon-based):**
- **Vimśottari** — up to **5 levels** (mahā → antar → pratyantar → sūkṣma → prāṇa), with the running chain resolvable at *any* instant, not just birth
- **Aṣṭottarī** (108-yr), **Yoginī** (36-yr), **Kālachakra** (savya/apasavya, deha/jīva)
- **Conditional udu daśās** — the minor Parāśarī systems, each applied only when its natal condition holds

**Rāśi & graha daśās:** all 12 Jaimini rāśi daśās, Karaka and Mūla graha daśās (see §3), plus:
- **Sudarsana Chakra daśā** — seed = strongest of Lagna / Moon / Sun; **one house per year**; each daśā splits into 12 monthly antardaśās (daśā sign as lagna, houses 1..12). Parāśara via Rao ch.31, verified against the book's Indira Gandhi example.

### 8. Transits & timing

- **Gochara** — transits over the natal chart from the Moon sign, with the classical favourable-house rules
- **Vedha** — the obstruction map that cancels an otherwise-good transit, reported with the blocking planet and house
- **Aṣṭakavarga-gated transits** — each transit qualified by the bindu count of the sign it crosses
- **Sāḍe-sātī** — full passport: the three phases (12th / 1st / 2nd from Moon), entry/exit dates, and the peak
- **Kaṇṭaka-śani** (4th) and **Aṣṭama-śani** (8th)
- **Tārā-bala** (9 tārās from the natal Moon's nakṣatra) and **Chandra-bala** (transit Moon's house from the natal Moon)
- **Mūrti-nirṇaya** — each transiting graha's **Suvarṇa / Rajata / Tāmra / Loha** form, from its house from the natal Moon (1/6/11 gold — extremely favourable; 2/5/9 silver — favourable; 3/7/10 copper — average; 4/8/12 iron — extremely unfavourable)
- **Kāla-sarpa** windows, **eclipse** proximity, **Bhṛgu-bindu**, **asta** (combustion) and retrograde stations
- **Sarvatobhadra Chakra (SBC)** — with the rāśi ring and **akṣara-vedha**
- **Kota chakra** (siege model) and **Kūrma** directional map
- **Tri-Pataki chakra** — the annual vedha map: Moon progressed by year mod 9, Sun/Mercury/Jupiter/Venus/Saturn by year mod 4, Mars/Rāhu/Ketu by year mod 6 (nodes counted in reverse); vedha read across the three kendra rings
- **Personal calendar** — favourable / unfavourable day windows across a date range, combining transits, tārā/chandra-bala and the running daśā

### 9. Longevity & mundane

**Āyurdāya — four classical methods, cross-checked, band-only:**

| Method | Basis |
|---|---|
| **Piṇḍāyu** | each graha's piṇḍa term scaled by its arc from debilitation (full at exaltation, half at debilitation) — Raman, *How to Judge a Horoscope* vol.2 |
| **Aṁśāyu** | Satyacharya's navāṁśa method: longitude′ ÷ 200 = navāṁśas, mod 12 = whole years, remainder = fraction; **Bharaṇa** increases (×3 exalted/retrograde, ×2 vargottama/own) |
| **Naisargāyu** | fixed natural years Sun..Saturn = 20 / 1 / 2 / 9 / 18 / 20 / 50 (sum = 120) — Sāravalī 40.20 |
| **Jaimini āyus** | Chara/Sthira/Dual natures of three factor-pairs (lagna & 8th lords, lagna & Moon, lagna & Horā-lagna), reconciled per Raman art.108 |

Each of the first three applies the three **Haraṇas** — Chakrapātha (western-half
reduction by house and planet nature), Śatrukṣetra (enemy sign, −⅓; Mars and
retrogrades exempt), Astaṅgata (combustion, −½). **Ethics:** the engine returns
**only a coarse band** — *alpa / madhya / pūrṇa* — plus the **maraka (killer)
lords**, and **never** a date or age of death.

**Medini (mundane) — weather and rainfall:**
- **Tri-Nāḍī chakra** — 27 nakṣatras in Heaven / Earth / Pātāla by a boustrophedon-of-three, verified cell-for-cell against the source figure; rainfall read from where benefics vs malefics fall (all planets in one nāḍī, Sun+Jupiter together, all-male → flood risk, female+eunuch → hail, and the Heaven/Earth/Pātāla benefic-malefic split rules)
- **Saptanāḍī chakra** — 28 nakṣatras (incl. Abhijit) across 7 nāḍīs (Chanda/Saturn, Vāyu/Sun, Dahan/Mars, Saumya/Jupiter, Neera/Venus, Jala/Mercury, Amṛta/Moon) by a boustrophedon **starting from Krittika**; verified — it reproduces the source figure's Chanda list {Krittika, Viśākhā, Anurādhā, Bharaṇī} and Vāyu list {Aśvinī, Rohiṇī, Svāti, Jyeṣṭhā} exactly. A nāḍī holding 2+ grahas is in **vedha**, each with its own rainfall effect.
- **Sun–Moon nakṣatra rule** — each nakṣatra's owner (Sun or Moon) and gender (male / female / eunuch); the pairing of the Sun's and Moon's nakṣatras indicates the rainfall

### 10. Praśna & muhūrta

- **Classical praśna (1–108)** — the seed fixes the ascendant to a navāṁśa-pāda; the question type maps to primary/secondary houses, judged on occupants, house lords and the Moon as messenger
- **KP horary (1–249)** — the sub-lord method (see §4)
- **Aṣṭamaṅgala-praśna** — the Saṅkhya (numeric) core of the Kerala deva-praśna, *Praśna Marga* ch.VII: the querent's three-digit **Aṣṭamaṅgala number** ÷ 30, 27, 7, 12, 9, 5 yields the tithi, nakṣatra, weekday, rāśi, planet and bhūta; the digits map to future / present / past; results are reckoned from the Janma rāśi and nakṣatra (with tārā class). The cowrie/gold-coin *rite* is not computable — the number is the input.
- **Muhūrta** — pañcāṅga-śuddhi plus the **21 mahā-doṣas** (Viṣṭi/Bhadrā, Pañcaka, gaṇḍānta, Gaṇḍa-mūla, dagdha-tithi, and the rest)
- **Kāla periods of the day** — Rāhu-kālam, Yamaganda, Gulika-kāla (the 1/8 divisions by weekday)
- **Rectification** — birth-time refinement within a stated bracket, scored against supplied life events

### 11. Doṣas, remedies, folk & compatibility

- **Doṣa panel** — Maṅgala (Kuja), Kāla-sarpa, Sāḍe-sātī, gaṇḍānta and the common afflictions, each with the cancellation rules
- **Upāya engine** — gemstones, mantras and remedial measures keyed to the weak or afflicted graha
- **Points** — **Marana-Kāraka-Sthāna** (each planet's "killing" house from the lagna), **Puṣkara navāṁśa / bhāga**
- **Folk panels** — **Diśā-śūla** (the day's barred direction), **Choghaḍiyā** (the 8 day/night slots), **nāmakaraṇa** (the name's syllable from the Moon's nakṣatra-pāda)
- **Pañca-pakṣī** (five-bird timing) — the birth bird from nakṣatra + pakṣa; the full **weekday × pañca-yama** activity grid (Ruling / Eating / Walking / Sleeping / Dying) for both pakṣas, day and night; **apahāra** sub-birds within each yama; friend / enemy relations; and a power factor = str(main act)/5 × str(sub act)/5. Durations 8:6:5:3:2. Verified cell-for-cell against the source's detailed chart.
- **Compatibility** — the 8-fold **Aṣṭa-kūṭa (Guṇa Milan)**: Varṇa (1), Vaśya (2), Tārā (3), Yoni (4), Graha-Maitrī (5), Gaṇa (6), Bhakūṭa (7), Nāḍī (8) = 36 points, with the classical doṣa checks; **and** the South-Indian **Dasa-Porutham** (10-fold): Dina, Gaṇa, Mahendra, **Strī-Dīrgha**, Yoni, Rāśi, Rāśi-adhipati, Vaśya, **Rajju** (zig-zag boustrophedon over blocks of 9), **Vedha** (11 obstruction pairs)

### 12. Lāl Kitāb, BNN, Nāḍī, rarities

- **Lāl Kitāb** — ancestral debts (ṛṇa), sleeping / awake planets, blind and one-eyed houses, the "main error", and its own distinct remedies
- **Bhṛgu-Nandi-Nāḍī (BNN)** — the kāraka-chain progression reading
- **Nāḍī** — **nāḍī-aṁśa** (150-fold) naming from the Chandra-Kala-Nadi scheme, saṁskāra flags, the Ketu pattern, and the fixed-scenario read
- **Tri-Pataki chakra** (see §8) — annual vedha of the Moon with the classical per-planet effects
- **Stri-Jātaka** (female horoscopy, BPHS ch.80) — the Lagna/Moon parity character rule (both even → steadfast and feminine; both odd → masculine bearing; mixed → both), and houses 1 (appearance) / 4 (conduct) / 5 (children) / 7 (husband) / 8 (widowhood) read from the stronger of Lagna or Moon. Reported with the chapter's own modern caveat that an independent woman's chart applies to her alone.
- **Mūrti-nirṇaya** (see §8) — the transit's gold/silver/copper/iron form
- **Iṣṭa-devatā** — the deity from the Kārakāṁśa's 12th, with the Jaimini chain

### 13. Cross-school verdict

Instead of a pile of per-school facts, the engine delivers a **single verdict per
life area** — career, marriage, wealth, children, health, education, property,
spirituality. Each verdict carries **one score**, a **confidence grade**, and the
exact contributing reasons drawn from every school above, so a user can see
*which* testimony drove the answer and *where the schools disagree*.

Also available: a **yoga-score** ranking (which yogas actually matter in this
chart), a **consensus** timeline across daśā systems, and a natural-language
**narrative** layer.

---

## HTTP API reference

Base: `https://<host>` · JSON in/out · optional `X-API-Key` on LLM-backed routes
· per-IP rate limiting.

| Method & path | Purpose | Key request fields |
|---|---|---|
| `GET /api/health` | Liveness probe | — |
| `POST /api/geocode` | Place → lat/lon/timezone | `city` |
| `POST /api/natal` | Full natal payload (all schools) | `date`, `time`, `lat`/`lon` or `city`, `tz?`, `ayanamsa?`, `ru?` |
| `POST /api/varsha` | Annual (Varṣaphala) chart | natal fields + `year` |
| `POST /api/prashna` | Praśna / horary | `seed`, `topic`, `kp?`, `lat`, `lon` |
| `POST /api/compatibility` | Aṣṭa-kūṭa + Porutham | `groom`, `bride` (two birth blocks) |
| `POST /api/muhurta` | Muhūrta scan + mahā-doṣas | `date_start`, `date_end`, `event`, place |
| `POST /api/rectify` | Birth-time rectification | natal fields + bracket + events |
| `POST /api/calendar` | Personal day windows | natal fields + date range |
| `POST /api/verdict` | Cross-school verdict per area | natal fields + `topic?` |
| `POST /api/explain` · `POST /api/reading` | Natural-language interpretation (LLM) | `kind`, `depth`, `facts` / `school` |
| `GET /api/asta` · `GET /api/yogas` | Direct sub-reports | natal fields |

**`POST /api/natal` response** is grouped by root system, with no duplication —
shared data lives under `core`, each school under its own key:

`core` · `parashara` · `jaimini` · `ashtakavarga` · `kp` · `nadi` · `lal_kitab` ·
`transits` · `doshas` · `points` · `chalit` · `sudarshana` · `eclipses` ·
`upaya` · `bhrigu_bindu` · `pancha_pakshi` · `synthesis` · `overview` ·
`narrative`

The `jaimini` block carries the kārakas, kārakāṁśa, arūḍhas, upapada, argala,
rāśi-dṛṣṭi, special lagnas, **Sree Lagna**, rāja-yogas, death significators,
**all 12 rāśi daśās**, **Karaka** and **Mūla** graha daśās, **Sudarsana Chakra
daśā**, the four-method **āyurdāya**, **medini**, **stri_jataka**, **sphutas**,
**true_nodes** and **gochara_bala**.

Responses are cached under a **schema version** — any change to a calculation
invalidates every stale cache entry automatically, with no manual purge.

---

## Boundaries

The engine **declares** what it does not compute rather than guessing:

- **Āyurdāya returns a band only** (*alpa / madhya / pūrṇa*) + maraka lords —
  never a date or age of death. The four methods can disagree; the band is a
  traditional indication, not a prediction.
- **Phala-Śūla daśā — not implemented.** It exists to predict the *age of death*
  (54–63); withheld for the same reason Āyurdāya is band-only.
- **Aṣṭamaṅgala rite — not computed.** The cowrie/gold-coin divination is a
  physical rite; only its numeric (Saṅkhya) core is computed, from the number the
  querent supplies.
- **Mṛtyu-bhāga (fatal degrees) — not implemented.** The available source table
  is OCR-corrupted (digits split and ambiguous) and published variants differ; a
  wrong "fatal degree" table is worse than none.
- **"Navāṁśa / Nakṣatra daśā" — not distinct systems.** Confirmed against both
  Rao's textbook and JHora's own daśā catalogue: *nakṣatra daśā* is a **class**
  (Vimśottari, Aṣṭottarī, Kālachakra), not one system, and no separate
  constructible Navāṁśa or Nakṣatra rāśi daśā exists.
- **Dasa-Porutham Vedha** — the five stars with no listed partner simply *pass*
  (a partner-less star has no vedha); this is stated, not treated as missing data.

---

## Disclaimer

This engine is for study and reflection. Jyotiṣa is a traditional symbolic
system, not a science of certainty. Nothing here is medical, legal, financial or
psychological advice. Longevity output is a coarse traditional band, never a
prediction of death.

---
---

# 🇷🇺 Русская версия

**Математическая ведическая астрология (Джйотиш).** Этот репозиторий —
**только документация**: описывает *всё, что движок рассчитывает*, и *полный
HTTP API* для вызова. Исходного кода здесь **нет** — сам движок в отдельном
приватном проекте.

**Принцип: считать то, что алгоритмично; никогда не выдумывать то, чего нет.**
Каждая таблица и формула ниже возведена к названному классическому источнику и,
где источник печатает разобранный пример, сверена с ним. Где правило нельзя
подтвердить чисто, движок так и говорит (см. *Границы*), а не изобретает числа.

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
| Луна | По умолчанию геоцентрическая; доступна **топоцентрическая** (параллакс наблюдателя, сдвиг до ~1°) |
| Узлы | Ось Раху–Кету ровно 180°; **средние** узлы по умолчанию, **истинные** (оскулирующие) — опционально |

---

## Что рассчитывается — по школам

### 1. Базовая карта

- Сидерическая **долгота, знак, накшатра + пада, знаковый дом** 7 грах + Раху/Кету; **ретроградность** и суточная скорость.
- **Лагна (асцендент)** — долгота, знак, владыка; куспиды в выбранной системе.
- Полный **панчанг**: **титхи** (1–30 + природа), **накшатра** (+ владыка, божество, гана, нади, йони, пада), **йога** (27), **карана** (11 по 60 полутитхи), **вара** — день недели по *управляющему восходу* локального гражданского дня, а не от полуночи.
- **Топоцентрическая Луна** — с поправкой на параллакс места; как дополнительные данные для ректификации и проверки границ накшатр (классика геоцентрична, поэтому это никогда не значение карты по умолчанию).
- **Истинные узлы** — оскулирующие Раху/Кету рядом со средними.

### 2. Парашара (BPHS)

**Дробные карты (варги) — 20 штук.** D1 (раши), D2 (хора), D3 (дреккана),
D4 (чатуртхамша), D6 (шаштамша), D7 (саптамша), D8 (аштамша), D9 (навамша),
D10 (дашамша), D12 (двадашамша), D16 (шодашамша), D20 (вимшамша), D24
(сиддхамша), D27 (бхамша), D30 (тримшамша), D40 (кхаведамша), D45
(акшаведамша), D60 (шаштьямша) — полная **шодашаварга** — плюс **D150**
(надиамша) и **D300** для тонкой работы. Каждая варга строится как полная карта
(лагна + все грахи), а не только позиции планет.

**Шадбала** — все шесть сил каждой грахи, в рупах, с классическими минимумами и рангом:
- **Стхана-бала**: уччха-бала, саптаваргаджа-бала, оджа-югма-бала, кендради-бала, дреккана-бала
- **Диг-бала** (сила направления)
- **Кала-бала**: натхонната, пакша, трибхага, аяна, плюс владыки года/месяца/дня/хоры, чешта (сила движения) и юддха (планетная война)
- **Чешта-бала**, **Найсаргика-бала**, **Дрик-бала** (сила аспектов)

**Прочие меры силы и состояния:**
- **Бхава-бала** — сила домов (обитатели, аспекты, сила владыки, вклад дигбалы)
- **Ишта / кашта пхала** — благая vs вредная способность каждой грахи
- **Вимшопака-бала** — взвешенное достоинство по группе варг
- **Авастхи** — баладди (младенец→старик), джаградади (бодрствует/спит), диптади (12 состояний) и **астангата** (сожжение) с орбисами по планетам
- **Шаштьямша (D60), именованные амши** — 60-частное божество/природа по BPHS
- **Граха-юддха** (планетная война) — победитель/проигравший по долготе, с эффектом на силу
- **Упаграхи** — пять кала-вела (Кала, Мритью, Ардхапрахара, Ямагхантака, **Гулика/Манди**) методом восхода, плюс пять теневых точек от Солнца (Дхума, Вьятипата, Паривеша, Индрачапа, Упакету)
- **Шапа (проклятия)** — указания со сферой и средством

**Йоги** — каталог из **484 правил** (плюс набор Адавала на 221 и справочный на
114), покрывающий раджа, дхана, даридра, набхаса, лунные, солнечные и именные
йоги из Пхаладипики, Саравали и Сарвартха Чинтамани. Каждое срабатывание
возвращает точную планетную причину и оценку силы, а не просто название.

**Варианты карты:**
- **Бхава-чалит** — карта куспидов Шрипати/Порфирия, показывает планеты, меняющие дом
- **Сударшана-чакра** — три-круг Лагна + Луна + Солнце, с консенсусом по каждой бхаве от всех трёх точек отсчёта
- **Бхават-бхавам** — производное чтение «дом от дома»
- **Спхуты** — плодородие: **Биджа** (Солнце+Венера+Юпитер) и **Кшетра** (Луна+Марс+Юпитер), оценка по нечёт/чёт раши + навамше; долголетие/прашна: **Тришпхута** (лагна+Луна+Гулика), **Чатухспхута** (+Солнце), **Панчаспхута** (+Раху), **Прана** (лагна×5+Гулика), **Деха** (Луна×8+Гулика), **Мритью** (Гулика×7+Солнце) — Prasna Marga VI, сверено с разобранным примером источника
- **Планетные хоры** — 24 владыки часов (день/ночь делятся на 12 каждый; первая хора = владыка дня, далее халдейский порядок), для электива

### 3. Джаймини

- **Чара-караки** — 7- и 8-каракные схемы → **Атмакарака** (АК), Аматья (АмК), Бхратри, Матри, Питри, Путра, Гьяти, Дара
- **Каракамша** и анализ **лагны Каракамши**
- **Аруда-пады** всех 12 домов; **Упапада (UL)** для брака
- **Аргала** (вмешательство) с разрешением **Виродхаргалы** (блокировки)
- **Раши-дришти** — аспекты знаков (подвижные↔фиксированные кроме смежного; двойственные↔двойственные)
- **Спец-лагны** — Бхава, Хора, Гхатика, Пранапада, Вигхати, Кунда, Инду, **Варнада**, **Шри-Лагна** (лагна + доля Луны в накшатре × 360°, сверено с примером источника)
- **Раджа-йоги Джаймини** — самбандха АК–АмК (по раши-дришти или соединению), раджа-йоги Каракамши
- **Сигнификаторы смерти** — Брахма / Махешвара / Рудра
- **Двара / Бахья (Пака / Бхога) раши** — для работы с долголетием

**Джаймини раши-даши — 12 систем, каждая из названного источника:**

| Даша | Старт | Порядок | Длительность |
|---|---|---|---|
| **Чара** | Лагна | зодиакально по чётности | счёт знак→владыка (варианты Рао и Иранганти, со-владыки, неравные антардаши) |
| **Стхира** | раши **Брахмы** | обычный зодиакальный | фикс. 7 / 8 / 9 лет (подвижный / фикс. / двойств.); цикл = 96 лет |
| **Трикона** | сильнейшая из Лагна / 5 / 9 | тригональные триады (1,5,9 · 2,6,10 · 3,7,11 · 4,8,12) | счёт до владыки как в Чара |
| **Брахма** | раши Брахмы (нечёт. лагна) или 7-й от него (чёт.) | прямой / обратный | счёт до места владыки 6-го от этого раши, −1 |
| **Йогардха** | сильнейший из Лагна / 7 | последовательно по чётности | ½·(Чара + Стхира) |
| **Варнада** | Лагна (нечёт.) или 12-й (чёт.) | прямой / обратный | счёт якорь → Варнада этой бхавы |
| **Дриг** | 9-й дом | три набора от 9/10/11 домов + их 3 раши-дришти аспекта | длительности Нараяны |
| **Судаса** (богатство) | **Шри-Лагна** | кендра → панапхара → апоклима | длительности Нараяны; Сатурн в старте → вперёд, Кету → обратно |
| **Лагна-Кендради** | сильнейший из Лагна / 7 | кендра → панапхара → апоклима | длительности Нараяны, те же исключения Сатурн/Кету |
| **Мандука** | сильнейший из Лагна / 7 | **скачки +3** («следующий 3-й знак, пропуская два») | Стхира 7/8/9 лет |
| **Нараяна** (Падакрама) | сильнейший из Лагна / 7 | зодиакально по чётности | счёт до владыки с поправками экз./деб. |
| **Ниряна-Шула** | — | — | 9-летние раши-даши, для долголетия |

**Джаймини грах-даши:**
- **Карака-даша** — Атмакарака первый, затем остальные караки по порядку; длительность = счёт знаков от Лагны до знака этой караки (0 → 12 лет). BPHS гл.46-178, сверено с иллюстративной таблицей книги.
- **Мула-даша** (Лагна Кендради Грах-даша) — производная от Вимшоттари. Длительность = годы Вимшоттари − (прямой счёт натальный знак → знак мулатриконы, −1); +1 в экзальтации, −1 в дебилитации; 0 → полные годы; отрицательное → по модулю. Раху считается к Водолею, Кету к Скорпиону. Порядок: сначала планеты в кендрах по силе знака, затем панапхара/апоклима по чётности лагны, с исключениями Сатурн-в-лагне → панапхара первой и Кету-в-лагне → апоклима первой. Сверено с разобранным примером (Венера в Близнецах → 16 лет).

### 4. Кришнамурти Паддхати (KP)

- KP-карта на **куспидах Плацидуса** с **аянамшей Кришнамурти**
- **Звёздный владыка / саблорд / саб-саблорд** для каждого куспида и каждой планеты — полное **249-частное** деление зодиака
- **Сигнификаторы** каждого дома, ранжированные по классической 4-уровневой иерархии
- **Управляющие планеты (RP)** на любой момент
- **Вердикты по событиям** — брак, карьера, богатство, дети, здоровье, заграница — читаются по значениям саблордов куспидов и подтверждаются действующей даша-бхукти
- **KP-хорар (1–249)** — число вопрошающего задаёт саблорд асцендента; судится по реальной карте Плацидуса на момент вопроса

### 5. Аштакаварга

- **Бхиннаштакаварга (BAV)** — карта бинду каждой из 7 грах
- **Сарваштакаварга (SAV)** — сводная карта из 337 бинду по 12 знакам
- **Триконa-шодхана** и **Экадхипатья-шодхана** — обе стадии редукции, применяются по порядку
- **Шодхья-пинда** — раши-пинда + граха-пинда → очищенный остаток силы по грахе
- **Какшья** — восьмеричное подделение для гейтинга транзитного тайминга

### 6. Таджика / годовые (Варшапхала)

- **Годовая (варша) карта** на любой год — по солнечному возвращению, с точным моментом возврата
- **Мунтха** — прогрессивная точка; **Варшеша** — владыка года (по классическому 5-частному состязанию)
- **Мудда-даша** — годовые под-периоды, масштабированные к году
- **Титхи-Правеша** — годовая карта лунного возвращения (альтернатива солнечному)
- **Сахамы** — **50** чувствительных точек типа арабских (Пунья, Яшас, Видья, Раджа, Матри, Питри, Путра, Джива, Карма, Рога, Мритью, …) с инверсией для дневного/ночного рождения
- **Таджика-йоги** — Итташала (сходящийся), Ишарапха (расходящийся), Накта, Ямая, Камбула и остальной 16-частный набор, вычисляются по **таджика-аспектам с орбисами (диптамша)**
- **Харша-бала** и **Панча-варгия-бала** (Вишва-бала) для карты года

### 7. Системы даш

**Накшатра-даши (от Луны):**
- **Вимшоттари** — до **5 уровней** (маха → антар → пратьянтар → сукшма → прана), с действующей цепочкой на *любой* момент, не только на рождение
- **Аштоттари** (108-летняя), **Йогини** (36-летняя), **Калачакра** (савья/апасавья, деха/джива)
- **Условные уду-даши** — малые парашари-системы, каждая применяется только при выполнении её натального условия

**Раши- и грах-даши:** все 12 джаймини раши-даш, Карака- и Мула-грах-даши (см. §3), плюс:
- **Сударшана-чакра-даша** — старт = сильнейший из Лагна / Луна / Солнце; **один дом в год**; каждая даша делится на 12 месячных антардаш (знак даши как лагна, дома 1..12). Парашара через Рао гл.31, сверено с примером Индиры Ганди из книги.

### 8. Транзиты и тайминг

- **Гочара** — транзиты по натальной карте от знака Луны, с классическими правилами благоприятных домов
- **Ведха** — карта обструкции, отменяющая иначе хороший транзит, с указанием блокирующей планеты и дома
- **Транзиты с гейтингом по Аштакаварге** — каждый транзит квалифицируется числом бинду пересекаемого знака
- **Саде-сати** — полный паспорт: три фазы (12-й / 1-й / 2-й от Луны), даты входа/выхода и пик
- **Кантака-шани** (4-й) и **Аштама-шани** (8-й)
- **Тара-бала** (9 тар от накшатры натальной Луны) и **Чандра-бала** (дом транзитной Луны от натальной)
- **Мурти-нирная** — форма каждой транзитной грахи **Сварна / Раджата / Тамра / Лоха** по её дому от натальной Луны (1/6/11 золото — крайне благоприятно; 2/5/9 серебро — благоприятно; 3/7/10 медь — средне; 4/8/12 железо — крайне неблагоприятно)
- Окна **Кала-сарпа**, близость **затмений**, **Бхригу-бинду**, **асты** (сожжение) и станции ретроградности
- **Сарватобхадра-чакра (SBC)** — с раши-кольцом и **акшара-ведхой**
- **Кота-чакра** (модель осады) и направленная карта **Курма**
- **Три-Патаки-чакра** — годовая карта ведхи: Луна прогрессирует на год mod 9, Солнце/Меркурий/Юпитер/Венера/Сатурн на год mod 4, Марс/Раху/Кету на год mod 6 (узлы обратным счётом); ведха читается по трём кольцам кендр
- **Персональный календарь** — благо/неблагоприятные окна дней на диапазон, объединяет транзиты, тара/чандра-балу и действующую дашу

### 9. Долголетие и мундан

**Аюрдая — четыре классических метода со сверкой, только диапазон:**

| Метод | Основа |
|---|---|
| **Пиндаю** | пинда-срок каждой грахи, масштабированный дугой от дебилитации (полный в экзальтации, половина в дебилитации) — Raman, *How to Judge a Horoscope* т.2 |
| **Амшаю** | навамша-метод Сатьячарьи: долгота′ ÷ 200 = навамши, mod 12 = целые годы, остаток = дробь; **бхарана**-увеличения (×3 экз./ретро, ×2 варготтама/своё) |
| **Нисаргаю** | фикс. природные годы Солнце..Сатурн = 20 / 1 / 2 / 9 / 18 / 20 / 50 (сумма = 120) — Saravali 40.20 |
| **Джаймини-аюс** | природы Чара/Стхира/Двойств. трёх пар факторов (владыки лагны и 8-го, лагна и Луна, лагна и Хора-лагна), примиряются по Raman ст.108 |

Первые три применяют три **Хараны** — Чакрапатха (редукция западной половины по
дому и природе планеты), Шатрукшетра (вражеский знак, −⅓; Марс и ретро
исключены), Астангата (сожжение, −½). **Этика:** движок выдаёт **только грубый
диапазон** — *альпа / мадхья / пурна* — плюс **мараки (лорды-убийцы)**, и
**никогда** дату или возраст смерти.

**Медини (мундан) — погода и дожди:**
- **Три-Нади-чакра** — 27 накшатр в Небо / Земля / Патала бустрофедоном-по-3, сверено ячейка-в-ячейку с фигурой источника; дожди читаются по тому, где благодетели vs вредители (все планеты в одной нади, Солнце+Юпитер вместе, все мужские → риск наводнения, женские+бесполые → град, и правила разделения благо/вредителей по Небо/Земля/Патала)
- **Саптанади-чакра** — 28 накшатр (вкл. Абхиджит) по 7 нади (Чанда/Сатурн, Ваю/Солнце, Дахан/Марс, Саумья/Юпитер, Нира/Венера, Джала/Меркурий, Амрита/Луна) бустрофедоном **от Криттики**; сверено — воспроизводит список Чанда {Криттика, Вишакха, Анурадха, Бхарани} и Ваю {Ашвини, Рохини, Свати, Джьештха} точно. Нади с 2+ грахами — в **ведхе**, у каждой свой дождевой эффект.
- **Правило накшатр Солнца–Луны** — владыка каждой накшатры (Солнце или Луна) и гендер (муж. / жен. / бесполый); пара накшатр Солнца и Луны указывает на дожди

### 10. Прашна и мухурта

- **Классическая прашна (1–108)** — зерно фиксирует асцендент на паде навамши; тип вопроса задаёт первичные/вторичные дома, судятся по обитателям, владыкам домов и Луне как посланнику
- **KP-хорар (1–249)** — метод саблордов (см. §4)
- **Аштамангала-прашна** — санкхья (числовое) ядро керальской дева-прашны, *Prasna Marga* гл.VII: трёхзначное **аштамангала-число** вопрошающего ÷ 30, 27, 7, 12, 9, 5 даёт титхи, накшатру, день недели, раши, планету и бхуту; цифры → будущее / настоящее / прошлое; результаты отсчитываются от Джанма-раши и накшатры (с классом тары). Обряд с раковинами/золотом невычислим — число даёт вопрошающий.
- **Мухурта** — панчанга-шуддхи плюс **21 маха-доша** (Вишти/Бхадра, Панчака, ганданта, Ганда-мула, дагдха-титхи и остальные)
- **Кала-периоды дня** — Раху-калам, Ямаганда, Гулика-кала (деления 1/8 по дню недели)
- **Ректификация** — уточнение времени рождения в заданном интервале, оценка по предоставленным событиям жизни

### 11. Доши, средства, быт и совместимость

- **Панель дош** — Мангала (Куджа), Кала-сарпа, Саде-сати, ганданта и частые поражения, каждое с правилами отмены
- **Движок упай** — камни, мантры и меры под слабую или поражённую граху
- **Точки** — **Марана-Карака-Стхана** (для каждой планеты — её «убийственный» дом от лагны), **Пушкара-навамша / бхага**
- **Народные панели** — **Диша-шула** (закрытое направление дня), **Чогхадия** (8 дневных/ночных слотов), **намакаран** (слог имени по паде накшатры Луны)
- **Панча-пакши** (тайминг пяти птиц) — птица рождения по накшатре + пакше; полная сетка **день недели × панча-яма** активностей (Правит / Ест / Ходит / Спит / Умирает) для обеих пакш, день и ночь; **апахара**-подптицы внутри каждой ямы; отношения друг / враг; коэффициент силы = str(осн. акт)/5 × str(под-акт)/5. Длительности 8:6:5:3:2. Сверено ячейка-в-ячейку с детальной таблицей источника.
- **Совместимость** — 8-частная **Ашта-кута (Гуна Милан)**: Варна (1), Вашья (2), Тара (3), Йони (4), Граха-Майтри (5), Гана (6), Бхакут (7), Нади (8) = 36 баллов, с классическими проверками дош; **и** южноиндийский **Даса-Порутхам** (10-частный): Дина, Гана, Махендра, **Стри-Диргха**, Йони, Раши, Раши-адхипати, Вашья, **Раджу** (зигзаг-бустрофедон по блокам из 9), **Ведха** (11 пар обструкции)

### 12. Лал Китаб, BNN, Нади, редкости

- **Лал Китаб** — родовые долги (рина), спящие / бодрствующие планеты, слепые и кривые дома, «главная ошибка» и собственные средства
- **Бхригу-Нанди-Нади (BNN)** — чтение по цепочке карак
- **Нади** — именование **нади-амши** (150-частное) по схеме Чандра-Кала-Нади, флаги самскар, паттерн Кету и чтение фиксированного сценария
- **Три-Патаки-чакра** (см. §8) — годовая ведха Луны с классическими эффектами по планетам
- **Стри-джатака** (женская хорография, BPHS гл.80) — правило характера по чётности Лагна/Луна (обе чётные → стойкость и женственность; обе нечётные → мужская манера; смешанно → и то, и другое), и дома 1 (внешность) / 4 (нрав) / 5 (дети) / 7 (муж) / 8 (вдовство) от сильнейшего из Лагны или Луны. Выдаётся с собственной современной оговоркой главы: карта самостоятельной женщины применяется к ней одной.
- **Мурти-нирная** (см. §8) — форма транзита золото/серебро/медь/железо
- **Ишта-девата** — божество от 12-го от Каракамши, с джаймини-цепочкой

### 13. Кросс-школьный вердикт

Вместо кучи фактов по школам движок даёт **единый вердикт по сфере жизни** —
карьера, брак, богатство, дети, здоровье, образование, недвижимость,
духовность. Каждый вердикт несёт **одну оценку**, **грейд уверенности** и точные
причины из всех школ выше, так что пользователь видит, *какое* свидетельство
дало ответ и *где школы расходятся*.

Также доступны: ранжирование **йога-score** (какие йоги реально значимы в этой
карте), **консенсус**-таймлайн по системам даш и слой **нарратива** обычным
языком.

---

## Справочник HTTP API

База: `https://<host>` · JSON вход/выход · опциональный `X-API-Key` на
LLM-маршрутах · рейт-лимит по IP.

| Метод и путь | Назначение | Ключевые поля запроса |
|---|---|---|
| `GET /api/health` | Проверка живости | — |
| `POST /api/geocode` | Место → широта/долгота/пояс | `city` |
| `POST /api/natal` | Полный натальный ответ (все школы) | `date`, `time`, `lat`/`lon` или `city`, `tz?`, `ayanamsa?`, `ru?` |
| `POST /api/varsha` | Годовая (Варшапхала) карта | натал-поля + `year` |
| `POST /api/prashna` | Прашна / хорар | `seed`, `topic`, `kp?`, `lat`, `lon` |
| `POST /api/compatibility` | Ашта-кута + Порутхам | `groom`, `bride` (два блока рождения) |
| `POST /api/muhurta` | Скан мухурты + маха-доши | `date_start`, `date_end`, `event`, место |
| `POST /api/rectify` | Ректификация времени рождения | натал-поля + интервал + события |
| `POST /api/calendar` | Персональные окна дней | натал-поля + диапазон дат |
| `POST /api/verdict` | Кросс-школьный вердикт по сфере | натал-поля + `topic?` |
| `POST /api/explain` · `POST /api/reading` | Интерпретация обычным языком (LLM) | `kind`, `depth`, `facts` / `school` |
| `GET /api/asta` · `GET /api/yogas` | Прямые под-отчёты | натал-поля |

**Ответ `POST /api/natal`** сгруппирован по корневым системам, без дублирования —
общие данные в `core`, каждая школа под своим ключом:

`core` · `parashara` · `jaimini` · `ashtakavarga` · `kp` · `nadi` · `lal_kitab` ·
`transits` · `doshas` · `points` · `chalit` · `sudarshana` · `eclipses` ·
`upaya` · `bhrigu_bindu` · `pancha_pakshi` · `synthesis` · `overview` ·
`narrative`

Блок `jaimini` содержит караки, каракамшу, аруды, упападу, аргалу, раши-дришти,
спец-лагны, **Шри-Лагну**, раджа-йоги, сигнификаторы смерти, **все 12
раши-даш**, **Карака-** и **Мула-** грах-даши, **Сударшана-чакра-дашу**,
четырёхметодную **аюрдаю**, **medini**, **stri_jataka**, **sphutas**,
**true_nodes** и **gochara_bala**.

Ответы кэшируются под **версией схемы** — любое изменение расчёта автоматически
инвалидирует устаревший кэш, без ручной чистки.

---

## Границы честности

Движок **объявляет**, что не считает, а не угадывает:

- **Аюрдая — только диапазон** (*альпа / мадхья / пурна*) + мараки — никогда
  дата или возраст смерти. Четыре метода могут расходиться; диапазон —
  традиционная индикация, не предсказание.
- **Пхала-Шула-даша — не реализована.** Она существует для предсказания
  *возраста смерти* (54–63); не включена по той же причине, по которой аюрдая —
  только диапазон.
- **Обряд аштамангала — не вычисляется.** Гадание на раковинах/золоте —
  физический обряд; считается только его числовое (санкхья) ядро из числа,
  которое даёт вопрошающий.
- **Мритью-бхага (фатальные градусы) — не реализована.** Доступная таблица
  источника испорчена OCR (цифры разбиты и неоднозначны), а опубликованные
  варианты расходятся; неверная таблица «фатальных градусов» хуже, чем никакой.
- **«Навамша / Накшатра-даша» — не отдельные системы.** Подтверждено и по
  учебнику Рао, и по каталогу даш самой JHora: *накшатра-даша* — это **класс**
  (Вимшоттари, Аштоттари, Калачакра), а не одна система, и отдельной вычислимой
  Навамша- или Накшатра-раши-даши не существует.
- **Ведха в Даса-Порутхам** — пять звёзд без пары просто *проходят* (у звезды без
  пары нет ведхи); это заявлено, а не считается пропуском данных.

---

## Отказ от ответственности

Движок — для изучения и размышления. Джйотиш — традиционная символическая
система, не наука определённости. Ничто здесь не является медицинской,
юридической, финансовой или психологической консультацией. Вывод о долголетии —
грубый традиционный диапазон, никогда не предсказание смерти.
