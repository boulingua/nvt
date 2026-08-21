# NVT — Dutch course roadmap

**Repo:** `boulingua/nvt` · **Code:** `nvt` · **Accent:** `#198D34` (light) / `#7EE797` (dark), pentagon mark · **Status:** scaffold → *coming soon*
**Author:** S. Le Boulanger · **Template:** `pagegen` · **Framework:** `boulingua-curriculum`

This document is the build plan for the boulingua Dutch course (*Nederlands als Vreemde Taal*), taking the repo from an empty scaffold (LICENSE + README + brand icons) to a live, curriculum-conformant course flipped to *active* on the hub world map. It is normative: every choice below conforms to `pagegen` and to `curriculum`, and nothing here overrides a shared standard.

---

## 1. Overview

**What it is.** A free, openly-licensed Dutch course on the boulingua platform, built for two audiences: learners in the German *Gesamtschule* system taking Dutch as a foreign/border-region language (Dutch is a genuine school and neighbour language in NRW and Niedersachsen), and independent CEFR learners. It follows the boulingua five-step unit model (**Activate → Input → Practise → Apply → Reflect**), with differentiated exercises, answer keys, teacher notes, committed open-format materials (`.odp`/PDF), and native-voice audio.

**Realistic CEFR scope.** Ship `core` (A1–B1) as the conformance floor, then extend to `full` (A1–C1). C2 is explicitly out of scope for the first two years (see §4). Full CEFR *scope* is declared; gaps are declared honestly per `curriculum/docs/conformance.md`, never hidden.

**Why Dutch.** High mutual intelligibility with German gives German-L1 learners fast early wins (huge shared lexicon, similar syntax), which lets the course move quickly through A1–A2 and invest depth at B1–B2 where the *false-friend* and word-order traps bite. It also fills a real gap: quality free Dutch OER for German schools is thin.

**Defining challenges (language-specific).**
1. **de/het gender.** Two article genders (common *de* / neuter *het*) with no fully reliable rule; ~75% of nouns are *de*, but the exceptions must be learned per-noun and they propagate into adjective inflection (*een groot huis* vs *een grote tafel*) and relative/demonstrative pronouns. This is the single biggest retention problem and needs a systematic, drilled, per-noun treatment.
2. **Word order (V2, verb-final, the *tangconstructie*).** Main-clause verb-second with inversion, subordinate-clause verb-final, and the split "bracket" construction of separable verbs and perfect/modal frames (*ik bel je morgen op* / *ik heb hem gisteren gebeld*). German learners get a partial free ride but overgeneralise German patterns.
3. **Pronunciation & spelling-to-sound.** The guttural *g/ch*, the *ui/eu/ij* vowels and diphthongs, *sch(r)*, and the long/short vowel spelling system (open vs closed syllable doubling: *maken → ik maak*, *man → mannen*). Latin script, so no new alphabet — but a focused sound/spelling onboarding is warranted.
4. **False friends & register.** Dense German↔Dutch false-friend field (*bellen*, *mogen*, *durven*, *aardig*), plus *je/jij/u* address and diminutive *-je* pragmatics.
5. **Netherlands vs Belgian (Flemish) variation.** Lexical, pronunciation and some grammatical differences the course must take a clear stance on.

---

## 2. Language & localisation decisions

- **Script / writing system.** Latin script, no complex shaping, no transliteration layer. **Decision:** no script-onboarding stage in the Cyrillic/Arabic sense; instead a compact **Level-0 "Klank & Spelling" onboarding block** (2–3 pages, see §5) covering the guttural *g*, diphthongs, and the vowel-doubling spelling rule. Recommended, small.
- **Variant / dialect.** **Decision:** teach **Standaardnederlands / Algemeen Nederlands (Netherlands norm)** as the primary variant — largest learner utility and the norm nearest German school syllabi. Flag notable **Belgian/Flemish** differences in a recurring "Vlaams vs Nederlands" callout and in the glossary, without forking content. Rationale: one authoritative variant keeps audio, materials and can-dos coherent.
- **RTL.** Not applicable (LTR).
- **Web fonts.** The hugo-coder default stack already covers Dutch — the only non-ASCII letters are the diaeresis (*coördinatie*, *reünie*) and the digraph *ij* (rendered as two letters, not U+0133). **Decision:** keep the template font stack; no extra script coverage needed. Ensure `enableEmoji = false` stays and that the *ij* is authored as `i`+`j` (never the ligature) for search/collation.
- **Native-voice / Piper TTS. Available, licence-clear, and now named.** Dutch is among the best-provisioned languages upstream. **Primary: `nl_NL-ronnie-medium` (`CC0`).** **Flemish callouts: `nl_BE-rdh-medium` (`CC0 1.0 Universal`)** — used for the regional-variation callouts, never as a substitute for the Netherlands norm. Where a dialogue needs more than two distinct voices, `nl_NL-mls-medium` (`CC-BY 4.0`) carries **52 speakers** in a single model, at the cost of an attribution line. Note which licence governs: `rhasspy/piper-voices` is MIT, but each model inherits its *training dataset's* terms, and a NonCommercial dataset cannot sit inside CC BY-SA 4.0 content — the three voices above are all clear, checked against their MODEL_CARDs. **Decision:** adopt Piper for all generated audio via the shared `audiogen`/`build_audio.py` pipeline. Voice IDs are read from **`audiogen/voices.yml`** (relocating to `kit/audio/voices.yml` at F1) and are never hand-typed; nothing is registered in `get_voices.sh` by hand any more, because that script is now registry-driven and the "add it alongside the existing fr/de/en entries" mechanism this line used to describe no longer exists. This matches the README's promise of native-voice audio and keeps CI out of the TTS path (audio is committed).
- **Level-0 onboarding.** **Decision:** yes to the small *Klank & Spelling* block (pronunciation + the spelling/doubling rule + *g/ch/sch* + *ij/ui/eu*), delivered as A1-adjacent appendix + the first A1 unit's Input, not as a separate CEFR level.

---

## 3. Instantiation from pagegen

Stand up the site by copying the template and changing only the marked values (per `pagegen/README.md` §"Instantiating a new course").

1. **Copy template into the repo.** Bring the full `pagegen` tree (`hugo.toml`, `go.mod`/`go.sum`, `archetypes/`, `content/`, `layouts/`, `assets/`, `data/`, `scripts/`, `i18n/`, `static/`, `.github/workflows/build-deploy.yml`, legal scaffold, `.gitignore`, `.gitattributes`, `.nojekyll`) into `nvt/`, preserving the existing `brand/`, `LICENSE`, `README.md`. Do **not** track `public/`. **[M]**
2. **Edit `hugo.toml` marked values only:** **[S]**
   - `baseURL = "https://boulingua.github.io/nvt/"`
   - `title = "Nederlands — S. Le Boulanger"`
   - `languageCode = "nl"`, `defaultContentLanguage = "nl"` (UI/legal remain per the boulingua German-legal standard; content language Dutch)
   - `[params].navTitle = "Nederlands"`, `description`, `keywords` (Dutch/NVT/CEFR/OER)
   - `[params].code = "nvt"` (this selects the `#198D34` accent — do **not** touch CSS)
   - `[[params.social]].url = "https://github.com/boulingua/nvt"`
   - `[params.plausible].domain = "boulingua.github.io/nvt"` (kept **last**, after all bare `[params]` keys — the TOML sub-table trap)
   - `[[menu.main]]` rebuilt to mirror `content/` sections (Levels A1…C1, Materials, About, Legal).
3. **Confirm accent data.** `data/accents.yaml` already carries `nvt` (`accent #198D34`, `hover #126626`). No edit needed — verify only. **[S]**
4. **Regenerate the pentagon + favicons.** Run `python brand/make_icon.py` so `brand/icon.svg`/`icon.png` and favicons render the `nvt` accent. **[S]**
5. **Fill the three legal pages** (`impressum`, `datenschutz`, `haftungsausschluss`) ⟨…⟩ placeholders, including the VG Wort METIS disclosure in `/datenschutz/` (§7). Once filled, drop the `|| true` from the legal-placeholder gate. **[M]**
6. **First green build.** `hugo --minify --gc` locally; the gate battery runs (VG Wort coverage warns until codes are drawn; render/manifest gates pass on an empty registry). Enable GitHub Pages (Actions source) and confirm `build-deploy.yml` deploys. **[S]**

**Definition of "instantiated":** the demo/example content is replaced by an empty-but-valid A1 section, the site builds green, and the accent + pentagon show `#198D34`.

---

## 4. Curriculum conformance target

**Framework:** `boulingua-curriculum` (pin `framework_version`). IDs follow `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` per `docs/id-scheme.md`.

- **Target conformance level.** Declare **`core` (A1–B1)** at first release; extend to **`full` (A1–C1)** as B2/C1 land. **`complete` (Pre-A1–C2) is not targeted** — freely-available Dutch material rarely reaches C2, and C2 is declared a gap, not built.
- **In-scope scales — implement.** Prioritise the activity-domain scales with dense official descriptors at A1–B1: `REC` (overall oral/reading comprehension, listening as a member of an audience, reading for information/orientation), `PROD` (overall oral/written production, sustained monologue, reports/essays), `INT` (conversation, information exchange, transactions, written interaction, online conversation), plus `LING` (general/vocabulary range, grammatical accuracy, phonological control — the natural home for the de/het, word-order and pronunciation work), `SOC` (sociolinguistic appropriateness — *je/u*, diminutives), and `PRAG` (coherence/cohesion, propositional precision, spoken fluency).
- **Declare `no-official-descriptor` / thin.** `MED` (mediation) and `PLUR` (plurilingual/pluricultural) are implemented only where descriptors exist and material is natural (some `MED` mediating-a-text and note-taking at B1+); everywhere the CV cell is empty, record `no-official-descriptor` — a silently missing scale is a conformance failure, an absent descriptor is not. `SIGN` receives no IDs (out of scope by the framework).
- **Mapping units → descriptor IDs.** Each unit's front-matter `curriculum` block uses `framework: cefr` with `cefr_level` and `cefr_can_do` prose; the machine-readable link is a per-level **`conformance.yml`** in the repo (mirroring `curriculum/examples/de-a1/conformance.yml`) whose `realizations[].implements_id` resolve to real statements in `curriculum/levels/*.md`. Author Dutch realisations (`nl:` field) beside each ID.
- **Publish the scope/coverage manifest.** Ship a repo-level scope declaration (implemented scales × levels-covered vs `no-official-descriptor`) at the repo root. **Gate.** The reusable workflow `boulingua/.github/.github/workflows/course-build.yml@v1` runs `python .curriculum/scripts/conformance_audit.py resolve --manifest conformance.yml --content content`, which checks format, global uniqueness and that every `implements:` resolves against `schema/scale-registry.yml`. `id-audit.sh` audits the framework's *own* level files and **cannot** validate this repo. Do not wire it here. Keep the `verify_cefr.py`-style drift check in `build-deploy.yml` alongside it, so drift fails the build. **[L, phased with content]**

---

## 5. Content creation plan

Recurring cast & theme for continuity across units: a small ensemble in a Dutch/Flemish-border setting (student **Sanne** in Nijmegen, exchange pupil **Jonas** from Germany, neighbour **meneer De Vries**, the *Vlaams* cousin **Lieke** in Antwerp for the variation callouts). Themes spiral: self/family → school/city → work/travel → society/media → abstract/argument.

Exams are **first-class sibling bundles** (`…-exam/index.md`, `page_type: exam`, shared `unit_nr`), never subsections. PDFs live under `static/downloads/<level>/`.

| Phase | Level | Units | Focus & notes | Effort |
|---|---|---|---|---|
| 0 | **Klank & Spelling** | onboarding (2–3 pp.) | guttural *g/ch/sch*, *ui/eu/ij*, vowel doubling, alphabet/audio | **S** |
| 1 | **A1** | 10 units + 2 exams | greetings, self/family, numbers/time, city, food; introduce *de/het* early, present tense, V2, *er is/zijn* | **M** |
| 2 | **A2** | 12 units + 2 exams | routines, shopping, health, travel, past tenses (perfectum + imperfectum), separable verbs, comparatives | **L** |
| 3 | **B1** | 14 units + 3 exams | work, opinions, media, plans, subordinate clauses (verb-final), relative clauses, *om…te*, conditional | **L** |
| 4 | **B2** | 14 units + 3 exams | argument, abstract topics, passives, *er*-constructions, register, connected discourse | **L** |
| 5 | **C1** | 10 units + 2 exams | nuance, idiom, academic/media texts, stylistic control | **L** |

**Appendices (each ≥1800 chars where prose):** de/het reference list & strategies; verb-conjugation tables; word-order/*tangconstructie* guide; pronunciation & spelling reference; false-friends (DE↔NL); Vlaams–Nederlands differences; glossary; CEFR self-assessment grids. **[M each]**

**Acceptance criteria per phase:** every unit has all five steps populated; differentiated exercises + answer key + teacher notes; a committed slide deck and worksheet (with thumbnails); native-voice audio for vocab/dialogue/text; a resolving `curriculum` block + `conformance.yml` realisations; a VG Wort mark registered (§7); sources openly-licensed/PD and cited. A level is "done" when its exam bundle(s) exist, all units are `materials_status: ready`, and the §4 conformance gate + the gate battery pass.

---

## 6. Website & materials

- **Section landings via shortcodes.** Every `_index.md` (`page_type: section`) uses the shared shortcodes (`hero`, `lead`, `kicker`, `card-grid`/`card`, `callout`, `details`, `downloads`) — **never raw HTML**. Landings for each level + the Materials hub.
- **Materials pipeline.** Decks from **`slidegen`** (`beamerthemeboulingua`), worksheets from **`sheetgen`** (`boulingua-sheet.sty`), driven by `scripts/build_materials_latex.py`. Outputs are **committed** under `static/materials/` (+ `.odp` open format) with PDFs indexed from `static/downloads/<level>/`; CI **only verifies** (`verify_downloads.py`) — no TeX Live in the deploy path.
- **Native-voice audio.** `audiogen`/`scripts/build_audio.py` with **`nl_NL-ronnie-medium`** (primary, CC0) and **`nl_BE-rdh-medium`** (Flemish callouts, CC0 1.0 Universal); audio committed under `static/`/`data/audio`. Both are rows in `audiogen/voices.yml` (`kit/audio/voices.yml` from F1) — run `bash get_voices.sh` to fetch them. Never hand-add an entry to that script and never retype a voice ID: a transliterated ID is how a 404 gets written into a download script.
- **Thumbnails.** `scripts/render_thumbs.py` generates deck/worksheet thumbnails referenced by `presentation.thumbnail` / `worksheet.thumbnail` front-matter.
- **Downloads.** Per-level download hub; every file attributed (`pdf_attribution.py`) with CC BY-SA 4.0 + author.

---

## 7. VG Wort — pixel assignment for ALL content pages (required, non-skippable)

Per `pagegen/docs/vgwort-standard.md`, **every** editorial page ≥ 1800 characters gets exactly one VG Wort Zählmarke on exactly one URL: every **unit**, every **exam**, every **appendix**, and long-form editorial pages (`about`, `get-started`) — **but never** the home page, the Materials hub, tag/level/topic indexes, paginated continuations, or the templated legal pages (Impressum/Datenschutz/Haftungsausschluss).

Process for each qualifying page:
1. **Draw a fresh public code** (32-hex "Öffentlicher Identifikationscode") from the author's **T.O.M.** account — never invent codes, never expose the private code.
2. **Register** it in `data/vgwort.yaml`, matched by `url:` (base-stripped `RelPermalink`) or `path:` (`content/<File.Path>`), with `pixel_url`, `min_chars: 1800`, `author`, `registered_at`.
3. **Render** via the single shared resolver `layouts/_partials/vgwort/url.html` → `<head>` preload + eager off-screen `<img>` (no JS, no consent gate, `loading="eager"`, `visibility:hidden`, `met.vgwort.de` un-proxied).
4. **Record** the mark in the private usage registry (kept **outside** the repo): `Used`, `Projekt=nvt`, `Sprache=Nederlands`, `Niveau`, `Kurstitel`, `URL`, `Pixel_URL`.
5. **Verify** via the gates: coverage audit (`verify_vgwort_coverage.py`, warns on unregistered ≥1800-char pages), render verify (`verify_rendered_pixels.py`, blocking — token present, one URL site-wide), manifest gate, and the hub guard (asserts `met.vgwort.de` absent from `/materials/`).
6. Add `^https?://([a-z0-9-]+\.)*vgwort\.de` to the `lychee` exclude; disclose METIS counting in `/datenschutz/`.

**Estimated total marks needed:** ~60 units + ~12 exams + ~8 appendices + ~2 editorial ≈ **80–90 public codes** across the full A1–C1 build (≈ 26 for the `core`/A1–B1 MVP). Draw codes per phase as pages are authored.

---

## 8. Milestones & sequencing

- **M0 — Instantiation (weeks 1–2).** §3 complete; green build on Pages; accent/pentagon correct; legal pages filled. *Dep:* none.
- **M1 — Curriculum wiring (weeks 2–4).** Repo `conformance.yml` scaffolding for A1; the §4 conformance gate passes; `verify_cefr` gate wired. *Dep:* M0.
- **M2 — A1 MVP live + coming-soon flip candidate (weeks 4–10).** Level-0 onboarding + all A1 units + A1 exams + core appendices; materials + audio committed; VG Wort marks drawn/registered; **`core` declared partial (A1)**. First public "active" candidate. *Dep:* M1, slidegen/sheetgen/audiogen configured for Dutch.
- **M3 — A2 (weeks 10–18).** Full A2; **`core` A1–A2** honest coverage. *Dep:* M2.
- **M4 — B1 → `core` complete (weeks 18–28).** B1 units/exams; declare **`core` (A1–B1)** met for in-scope scales. *Dep:* M3.
- **M5 — B2 + C1 → `full` (months 8–14).** Extend to **`full` (A1–C1)**; C2 declared out of scope. *Dep:* M4.

**Definition of "done / ready to flip from coming-soon to active on the world map":**
- Site builds green with the full gate battery passing (VG Wort render/manifest/hub, downloads, legal placeholders enforced).
- At minimum the **A1 level is complete** (units + exams + onboarding + core appendices), all `materials_status: ready`, with committed materials, thumbnails and native audio.
- Curriculum scope manifest published and green under the §4 conformance gate; conformance level declared (`core`, at least partial A1) honestly.
- Every qualifying content page carries a registered, rendering VG Wort mark recorded in the usage registry.
- Legal pages filled; `/datenschutz/` discloses METIS. Then update the hub `website` world-map entry for `nvt` from *coming soon* to *active*.

---

## 9. Open decisions & risks (language-specific)

- **de/het pedagogy — decide the system.** Colour-coding, per-noun tagging in the glossary, and spaced drilling vs. rule-of-thumb teaching (diminutives → *het*, plurals → *de*, etc.). *Risk:* under-treating this tanks B1 accuracy. **Recommendation:** systematic per-noun + a dedicated appendix + recurring drill.
- **Netherlands vs Flemish balance.** How much Vlaams to surface. *Risk:* over-forking. **Recommendation:** NL primary, Flemish as callouts + glossary + `nl_BE-rdh-medium` (CC0) for the Flemish audio callouts.
- **German-L1 leverage vs false friends.** Contrastive DE↔NL framing accelerates but breeds interference errors. *Risk:* overgeneralised German word order. **Recommendation:** explicit false-friend + word-order appendices and contrastive callouts; keep audience broader than German-only in prose.
- **Piper voice quality for the guttural *g* and diphthongs.** Availability and licence are settled (§2 — `nl_NL-ronnie-medium`, CC0); what is unverified is whether the synthesis flattens *g/ch/ui*, and that is exactly the sound set the Level-0 block teaches. *Risk:* the onboarding audio mispronounces its own subject. **Recommendation:** audition sample audio before bulk generation; the fallback ladder is `nl_NL-alex-medium` or `nl_NL-pim-medium` (both CC0, same locale), then transcript-only — never a German voice, however close the phonology looks. Annotate the pronunciation appendix with human-checked notes.
- **CEFR ceiling.** C1 material for Dutch OER is scarce; C2 unrealistic. *Risk:* over-promising. **Recommendation:** declare `full` (A1–C1) as the ambition, C2 as a documented gap.
- **Sourcing.** Openly-licensed/PD Dutch texts and imagery only, correctly cited. *Risk:* copyright creep. **Recommendation:** maintain a `references/` provenance page; verify licences before commit.
