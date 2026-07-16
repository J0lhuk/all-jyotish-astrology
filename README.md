# All Jyotish Astrology — calculations & API reference

**A math-first Vedic astrology (Jyotiṣa) engine.** This repository is
**documentation only** — it describes *everything the engine calculates* and the
*complete HTTP API* for calling it. There is no source code here; the engine
itself is a separate private project.

**Математическая ведическая астрология (Джйотиш).** Этот репозиторий —
**только документация**: описывает *всё, что движок рассчитывает*, и *полный
HTTP API* для вызова. Исходного кода тут нет — сам движок в отдельном приватном
проекте.

Design rule / Принцип: **compute what is algorithmic; never fabricate what
isn't.** Where a classical rule cannot be sourced cleanly, the engine says so
(see *Boundaries*) rather than inventing numbers.

---

## Table of contents

- [Astronomical foundation](#astronomical-foundation--астрономическая-основа)
- [What is calculated](#what-is-calculated--что-рассчитывается)
  - [Core chart](#1-core-chart--базовая-карта)
  - [Parāśara](#2-parāśara-bphs--парашара)
  - [Jaimini](#3-jaimini--джаймини)
  - [Krishnamurti Paddhati (KP)](#4-krishnamurti-paddhati-kp--кп)
  - [Tājika / annual](#5-tājika--annual--таджика--годовые)
  - [Daśā systems](#6-daśā-systems--системы-даш)
  - [Transits & timing](#7-transits--timing--транзиты-и-тайминг)
  - [Cross-school verdict](#8-cross-school-verdict--кросс-школьный-вердикт)
  - [Doṣas, remedies, folk](#9-doṣas-remedies-folk--доши-средства-быт)
  - [Matching](#10-matching--совместимость)
  - [Praśna, muhūrta, rectification](#11-praśna-muhūrta-rectification--прашна-мухурта-ректификация)
  - [Lāl Kitāb, BNN, Nāḍī](#12-lāl-kitāb-bnn-nāḍī)
- [HTTP API reference](#http-api-reference--справочник-api)
- [Boundaries](#boundaries--границы-честности)
- [Disclaimer](#disclaimer--отказ-от-ответственности)

---

## Astronomical foundation / Астрономическая основа

| Aspect / Аспект | Choice / Выбор |
|---|---|
| Ephemeris / Эфемериды | Swiss Ephemeris, **Moshier** analytical mode — no data files; ~0.1″ planets, ~0.3″ Moon |
| Zodiac / Зодиак | **Sidereal / сидерический**; ayanāṁśa: **Lahiri** (default), **Krishnamurti** (KP), Raman |
| Houses / Дома | **Whole-sign / знаковые** (default) · **Placidus** (KP) · **Porphyry/Śrīpati** (bhāva-chalit) |
| Time / Время | IANA timezone auto-detected from lat/lon with the **historical** offset (DST-correct for the birth date); `LMT` for pre-standard-time births |
| Sunrise / Восход | Visible sunrise — upper limb + refraction (drik-pañcāṅga standard); matches published pañcāṅgas to seconds |
| Nodes / Узлы | Rāhu–Ketu axis, exactly 180° apart |

---

## What is calculated / Что рассчитывается

### 1. Core chart / Базовая карта
Sidereal longitudes, sign, nakṣatra + pāda, and whole-sign house of the 7 planets
+ Rāhu/Ketu; the ascendant (lagna) and its lord; the full **pañcāṅga** (tithi,
nakṣatra, yoga, karaṇa, vāra — the weekday from the *governing sunrise* on the
local civil day); the current Vimśottari daśā chain.

RU: Сидерические долготы, знак, накшатра+пада, знаковый дом 9 планет; лагна и её
лорд; полная панчанга (титхи/накшатра/йога/карана/вара от управляющего восхода);
текущая цепочка Вимшоттари-даши.

### 2. Parāśara (BPHS) / Парашара
- **Divisional charts (vargas)** — D1…D60, plus D108/D150/D300; the 16-varga set
  (Ṣoḍaśavarga) and **Vimśopaka bala** (weighted varga strength).
- **Ṣaḍbala** — all six planetary strengths (Sthāna, Dig, Kāla, Cheṣṭā,
  Naisargika, Dṛk) in virūpa/rūpa; plus **Iṣṭa/Kaṣṭa phala** (benevolence vs
  distress).
- **Bhāva Bala** — house strength: Bhāvādhipati (lord's strength) + Bhāva
  Digbala (sign-kind of the cusp) + Bhāva Dṛṣṭi (net aspect). Source: B.V. Raman,
  *Graha and Bhava Balas*, Art. 124–132.
- **Aṣṭakavarga** — BAV per planet, SAV (control total 337), Trikoṇa &
  Ekādhipatya śodhana, **Śodhya Piṇḍa**, and kakṣya transit bindus.
- **Avasthās** — all five systems (Bālādi, Jāgradādi, Dīptādi, Lajjitādi,
  Śayanādi).
- **Yogas** — ~19 algorithmic detectors + a declarative rule DSL + a
  ~484-entry sourced named-yoga library, each fired yoga scored 0–100 with an
  activation window.
- **Bhāva-chalit** — Śrīpati/Porphyry cusp chart: which planets shift house
  vs whole-sign.
- **Sudarśana Chakra** — the three-fold read from Lagna, Moon and Sun with a
  per-house consensus, plus the 1-year-per-house daśā.
- **Upagrahas** (5 kāla-velā + 5 Sun-based), **Graha-yuddha** (planetary war),
  **Bhāvat-bhāvam** (derived houses).
- **Critical points** — Māraṇa-Kāraka-Sthāna, Puṣkara-bhāga; **Ṣaṣṭyaṁśa (D60)**
  named amśas; a 27-entry **nakṣatra knowledge base**.

RU: Дробные карты D1–D300 + Вимшопака; полная Шадбала + ишта/кашта; **Бхава-бала**
(сила домов, Раман 124–132); Аштакаварга (SAV=337, шодханы, шодхья-пинда, какшья);
5 систем авастх; йоги (детекторы+DSL+каталог, оценка 0–100); бхава-чалит;
Сударшана-чакра; упаграхи; граха-юддха; бхават-бхавам; критические точки; D60;
база накшатр.

### 3. Jaimini / Джаймини
Chara kārakas (7/8, Rāhu reverse-degree), Ātmakāraka, Kārakāṁśa, Arūḍha padas
(A1–A12), Upapada, **Argala/Virodha**, **Rāśi-dṛṣṭi** (sign aspects), **9 special
lagnas** (Bhāva/Horā/Ghaṭikā/Prāṇapada/Vighaṭi/Kuṇḍa/Bhṛgu-bindu/Śrī/Indu),
**Brahma/Maheśvara/Rudra death-trio**, **Niryāṇa Śūla daśā**, an ancestral-curse
(śāpa) detector, **Chara daśā** (KN Rao / Sanjay Rath variant), and **Iṣṭa /
Pālana / Guru devatā** from the Kārakāṁśa (12th/6th/9th, Daśāvatāra mapping).

### 4. Krishnamurti Paddhati (KP) / КП
The 249 sub-lord table, Placidus cuspal chart on the **Krishnamurti ayanāṁśa**
(~6′ off Lahiri — enough to move sub-lords), 4-level significators, ruling
planets, 52 house-group event verdicts, and **KP horary 1–249** ("name a
number" → judged on a real Placidus chart for the moment).

### 5. Tājika / annual / Таджика / годовые
Arc-second **solar return** (Varṣaphala), Muntha, Varṣeśa, ~15 Sahamas,
Ithaśāla/Īśrāfa aspects, **Mudda daśā** (360-day), Pañca-vargīya bala; **and
Tithi-Praveśa** — the luni-solar "Vedic birthday" chart (the instant the
Moon–Sun elongation regains its natal value near the solar return), an
independent second opinion on the year.

### 6. Daśā systems / Системы даш
Vimśottari (120y, 5 levels), Aṣṭottarī (108y), Yoginī (36y), Chara (Jaimini,
KN Rao/Rath), Nārāyaṇa, Kālachakra (full BPHS matrix), Mudda (annual), and the
eight **conditional Udu daśās** (Ṣoḍaśottarī 116, Dvādaśottarī 112, Pañcottarī
105, Śatābdikā 100, Caturaśīti 84, Dvisaptati 72, Ṣaṭtriṁśat 36, Ṣaṣṭihāyani 60).

### 7. Transits & timing / Транзиты и тайминг
- **Gochara** — Saturn/Jupiter from Moon, Kaṇṭaka/Aṣṭama Śani, Aṣṭakavarga
  transit bindus, and **gochara-vedha**: a favourable transit house cancelled
  when its paired vedha house is occupied (Sun–Saturn, Moon–Mercury exempt).
- **Sade-sati passport** — dated 12th/1st/2nd phases, the current one, and the
  dhaiya (small panoti, Saturn 4th/8th).
- **Sarvatobhadra / Kota / Kurma Chakras** — vedha, fort-breach, directional.
- **Eclipses** — natal grahaṇa-yoga + upcoming eclipses on the natal Moon/lagna.
- **Asta + stations** — dated combustion windows and retrograde/direct stations.
- **Bhṛgu Bindu** — Moon/Rāhu midpoint + dated slow-graha transits over it.
- **Personal calendar** — the next N days: daśā changes, ingresses, Tārā/Candra
  bala good/caution days.

### 8. Cross-school verdict / Кросс-школьный вердикт
For each life area (career, marriage, wealth, children, health, education,
property, spirituality): a single signed score (−100…+100), a **confidence
grade**, and **every contributing factor in plain language**. Inputs: house-lord
strength (śaḍbala + iṣṭa/kaṣṭa + dignity + house), natural kārakas, house
occupants, SAV bindus, relevant yogas, Jaimini argala. A weighted consensus over
already-computed facts — not an LLM guess.

RU: По каждой сфере — один счёт, уверенность и все факторы-«почему»; взвешенный
консенсус по посчитанным фактам, не выдумка LLM.

### 9. Doṣas, remedies, folk / Доши, средства, быт
- **Doṣa panel** — Kāla-Sarpa (12 named serpents by Rāhu's house), Maṅgala,
  Grahaṇa, Guru-Chāṇḍāla, Pitṛ, Gaṇḍa-Mūla, Kemadruma — status + cancellations.
- **Upāya engine** — functional nature per lagna, then remedies for the weakest
  grahas. **Hard rule:** a gemstone is proposed *only* for a functional
  benefic/yogakāraka; malefics get mantra + japa count, dāna, fast day,
  rudrākṣa, deity only.
- **Folk** — nāmakaraṇa (name syllable), diśā-śūla (direction to avoid),
  choghadiya (8 day + 8 night periods).
- **Pañca-Pakṣī** — birth bird from janma nakṣatra + pakṣa (two sourced
  variants), and the 8:6:5:3:2 day/night timing skeleton.

### 10. Matching / Совместимость
Both systems side by side:
- **North — Aṣṭakūṭa / Guṇa Milan** (8 kūṭas, max 36) + doṣa flags + Maṅgala
  cross-check on both charts.
- **South — Dasa-Porutham** (10): Dina, Gaṇa, Mahendra, Stree-Deergha, Yoni,
  Rāśi, Rāśi-adhipati, Vaśya, **Rajju** (zigzag formula), **Vedha** (pair table;
  stars with no listed pair return *no-data*, never guessed).

### 11. Praśna, muhūrta, rectification / Прашна, мухурта, ректификация
- **Praśna** — classical (1–108 seed) and KP horary (1–249).
- **Muhūrta** — kāla periods (Rāhu Kālam etc.), Abhijit, tārā/candra bala,
  per-event rule scan over a date range.
- **Rectification** — event-based birth-time search scoring daśā-lord vs
  event-house hits (bounded parameters).

### 12. Lāl Kitāb, BNN, Nāḍī
Lāl Kitāb bhū-chakra + six ṛṇas + upāya remedies; Bhṛgu-Nandi-Nāḍī lagna-less
narrative; Nāḍī D150/D300 functional placement (amśa # + sign + lord).

---

## HTTP API reference / Справочник API

Base URL / Базовый URL: `http(s)://<host>:8000`. All bodies are JSON; all
responses are JSON. Interactive OpenAPI docs are served at **`/docs`**
(Swagger UI) and **`/openapi.json`**.

### Common birth-data object / Общий объект рождения

Used by natal, varsha, compatibility, verdict, calendar, reading, rectify.

```json
{
  "date": "1990-05-15",        // YYYY-MM-DD (required)
  "time": "06:33:00",          // HH:MM[:SS], default 12:00:00
  "lat": 55.7558,              // OR provide "city"
  "lon": 37.6173,
  "city": "Moscow",            // geocoded if lat/lon absent
  "tz": "Europe/Moscow",       // IANA name | "LMT" | numeric "+5.5" | null=auto
  "ayanamsa": "LAHIRI",        // LAHIRI | KRISHNAMURTI | RAMAN | …
  "ru": true                   // Russian labels in output
}
```

### Endpoints / Эндпоинты

| Method | Path | Purpose / Назначение |
|---|---|---|
| `GET`  | `/api/health` | Liveness + version. |
| `POST` | `/api/geocode` | `{ "city": "…" }` → `{ name, lat, lon }`. |
| `POST` | `/api/natal` | The whole chart (see keys below). Cached (schema-versioned, 30-day TTL). |
| `POST` | `/api/verdict` | Cross-school life-area verdict. Body = birth-data + optional `"topic"` (one of career/marriage/wealth/children/health/education/property/spirituality); omit for all. |
| `POST` | `/api/calendar` | Personal N-day feed. Body = birth-data + `"days"` (default 30). |
| `POST` | `/api/varsha` | Varṣaphala + Pañca-vargīya bala + Tithi-Praveśa. Body = birth-data + `"year"`. |
| `POST` | `/api/compatibility` | `{ "groom": <birth>, "bride": <birth> }` → `{ ashtakuta, porutham }`. |
| `POST` | `/api/prashna` | `{ "seed": 1..249, "topic": "career", "kp": false, "lat": 55.75, "lon": 37.62 }`. `kp:false` = classical (seed 1..108); `kp:true` = KP horary (seed 1..249). |
| `POST` | `/api/muhurta` | `{ date_start, date_end, event, lat/lon or city, ru }` → best windows. |
| `POST` | `/api/rectify` | `{ date, approx_time, lat/lon or city, tz, events:[{date,kind,weight}], window_minutes (0<..360), step_minutes (0.25..30) }`. `kind` ∈ marriage/childbirth/career/job_change/father_death/mother_death/own_illness/wealth_gain/property/education/foreign. |
| `GET`  | `/api/asta?days=N` | Combustion windows + retrograde/direct stations (chart-independent; `days` ≤ ~1830). |
| `GET`  | `/api/yogas?q=&limit=` | The sourced named-yoga library, searchable. |
| `POST` | `/api/explain` | LLM narration of a computed payload. **Rate-limited**; optional `X-API-Key`. |
| `POST` | `/api/reading` | LLM full reading for one school. **Rate-limited**; optional `X-API-Key`. |

### `/api/natal` response keys / Ключи ответа

```
core          — planets, rasi chart, lagna, moon nakshatra, current dasha
parashara     — shadbala (+ ishta/kashta), bhava_bala, vimshopaka, yogas,
                yoga_scores, avasthas, vargas, dashas, upagrahas,
                graha_yuddha, bhavat_bhavam
ashtakavarga  — sav, sav_total, bav, shodhya_pinda
jaimini       — atmakaraka, chara_karakas, karakamsha, arudha_padas, upapada,
                chara_dasha, niryana_sula, death_significators, special_lagnas,
                rashi_drishti, curses, argala, devatas
kp            — planets, cusps, significators, ruling_planets
nadi          — nadiamsa, bnn
lal_kitab     — bhu-chakra, rinas, remedies
transits      — gochara (+ vedha), sbc, kota, kurma, consensus, sade_sati
doshas        — the dosha panel
points        — marana_karaka_sthana, pushkara
chalit        — bhava-chalit shifts + cusps
sudarshana    — three-fold houses + verdicts
eclipses      — natal grahan + upcoming
upaya         — remedies (with the gemstone gate)
bhrigu_bindu  — point + dated transit triggers
pancha_pakshi — birth bird + timing skeleton
synthesis     — cross-school verdicts for all 8 life areas
overview      — narrative (LLM if enabled)
```

### Example / Пример

```bash
curl -s http://localhost:8000/api/natal \
  -H "Content-Type: application/json" \
  -d '{"date":"1990-05-15","time":"06:33","lat":55.75,"lon":37.62,"tz":"5.5"}' \
  | jq '.synthesis.career'
```
```json
{
  "topic": "career", "label": "Карьера / работа",
  "score": 44.4, "verdict": "очень сильна", "confidence": "высокая",
  "factors": [ { "text": "Карака Сатурн: шадбала 7.0руп, ишта41/кашта17, в кендре(10)", "score": 15.0 }, … ]
}
```

### Auth & rate limits / Авторизация и лимиты
The two LLM routes (`/api/explain`, `/api/reading`) sit behind an optional
per-IP sliding-window rate limit and an optional `X-API-Key` header. Both are
opt-in via server env (`LLM_RATE_PER_MIN`, `JYOTISH_API_KEY`); all other routes
are open compute.

---

## Boundaries / Границы честности

Some classical rules could not be sourced to a single, self-consistent authority.
The engine **refuses to guess** and says so:

- **Āyurdāya (longevity)** — the source exists but harana tables and the
  three-pair selection rule differ across authors. A wrong longevity verdict is
  the one error worth refusing; if ever implemented it will output only a **range**
  (alpa/madhya/pūrṇa) + māraka periods, never a date of death.
- **Pañca-Pakṣī activity grid** — the birth bird (both sourced variants) and the
  8:6:5:3:2 timing skeleton are implemented; the per-weekday *which-bird-does-
  what* grid differs across lineages and is left out until verified.
- **Dasa-Porutham Vedha** — 11 pairs are from a Tamil booklet; the 5 stars it
  leaves unpaired return **no-data**, not an invented pair.
- **Nāḍī D150 names** — the 150 Sanskrit names are not encoded (no clean source);
  functional output (amśa #, sign, lord) is given instead.

RU: Ряд правил не сводится к одному непротиворечивому источнику — движок не
угадывает, а честно помечает пробел (аюрдая — только диапазон, без дат; сетка
активностей Панчапакши; 5 непарных звёзд Ведхи; имена D150).

---

## Disclaimer / Отказ от ответственности

Astrology here is a deterministic computation over classical rules. Treat the
output as a traditional interpretive model — **not** as fact, medical, legal,
financial, or professional advice.

RU: Астрология здесь — детерминированный расчёт по классическим правилам.
Результат — традиционная интерпретационная модель, **не** факт и не
профессиональный совет.
