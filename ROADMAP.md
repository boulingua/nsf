# NSF — Norwegian course roadmap

Build plan for **Norsk som fremmedspråk (NSF)** — the boulingua Norwegian course
(repo code `nsf`, signature accent `#6D8618` light / `#CEE77E` dark). The repo is
today an empty scaffold (LICENSE, README, brand icons) and is flagged *coming
soon* on the boulingua world map. This document is the end-to-end plan to build
the **content and the website** from that scaffold to a live course, conforming
to the pagegen template and the boulingua curriculum framework.

---

## 1. Overview

**What it will be.** A free, openly-licensed Norwegian (**Bokmål**) course on the
boulingua platform, following the five-step unit model (Activate → Input →
Practise → Apply → Reflect), curriculum-aligned to the CEFR, with committed
downloadable materials (`.odp` decks, PDF worksheets) and native-voice audio.

**Target learners.** German-*Gesamtschule* pupils taking Norwegian as a third/foreign
language and independent adult self-learners. Instruction/metalanguage is German
and English glosses where helpful; the target language is Norwegian throughout.

**Realistic CEFR scope.** Ship **A1 → B1** first (conformance target `core`); leave
B2/C1 as a declared, honest gap to revisit once the core is stable. Freely-available
OER rarely sustains B2+ for a smaller language, and mediation/plurilingual material
is thin — declaring the gap is correct, not a defect.

**Defining challenges (language-specific).**
1. **Two written standards.** Norwegian is officially Bokmål **and** Nynorsk. A
   foreign-language course cannot teach both well; picking one is unavoidable and
   load-bearing for every unit, deck and audio file.
2. **Scandinavian mutual intelligibility.** The boulingua world map credits
   Norwegian across **Denmark and Sweden** (Bokmål is largely readable to Danes/Swedes).
   The course must live up to that framing with an explicit receptive-Scandinavian
   thread, without turning into a Danish/Swedish course.
3. **Æ Ø Å + pronunciation-vs-spelling gap.** Latin script plus three extra letters
   (æ ø å) and a pitch-accent (two tonemes) system that is **not** written. Spelling
   is only a light onboarding cost; pronunciation and TTS fidelity are the real work.
4. **TTS availability.** Piper's shipped boulingua voice set has no Norwegian voice
   yet; a native Bokmål voice must be sourced and verified before audio can ship.
5. **Dialect reality.** Norway has no single spoken standard; audio and models must
   commit to one accessible spoken norm (Eastern/Oslo Bokmål) and say so.

---

## 2. Language & localisation decisions

Each item states a concrete, opinionated recommendation.

- **Variant — Bokmål.** *Recommendation: Bokmål.* It is the written form of ~85–90 %
  of Norwegians, the default in most textbooks/media, and the best-resourced for TTS.
  Nynorsk gets a short awareness appendix (what it is, how to recognise it), not a
  parallel track. `languageCode = "nb"`.
- **Script / writing system.** Latin + **æ ø å** (alphabet is 29 letters, æ ø å last).
  No RTL, no complex shaping. *Recommendation:* treat as a **light level-0 onboarding
  stage** (see below), not a full script course.
- **Web fonts.** The hugo-coder stack (system sans + the theme's mono for code)
  fully covers Latin-1 Supplement, so æ ø å render with **no extra web font**.
  *Recommendation:* keep the template font stack; do **not** add a webfont. Add one
  verification check that æ/ø/å and typographic quotes render in both themes.
- **RTL.** Not applicable — omit any bidi handling.
- **Pitch accent / tonemes.** Not orthographic; handle in the **pronunciation stage
  and audio**, not in spelling drills. Document tonemes descriptively (e.g.
  *bønder* vs *bønner* minimal-pair note) without demanding pupils produce them at A1.
- **Native voice / Piper TTS.** *Recommendation:* use the National Library of Norway's
  openly-licensed Bokmål voice packaged in `rhasspy/piper-voices`
  (`no_NO-talesyntese-medium`, NB Talesyntese, permissive licence). **Action item:**
  verify the exact voice path/licence in piper-voices, add it to `audiogen/get_voices.sh`
  (`"no_NO-talesyntese-medium|no/no_NO/talesyntese/medium"`), and smoke-test on an
  æ/ø/å + toneme minimal-pair sentence. **Fallback:** if quality is inadequate, ship
  text-only transcripts for the affected segments and record the gap — never block a
  unit on audio.
- **Level-0 script/pronunciation onboarding.** *Recommendation:* a small **`Uttale`
  (pronunciation) stage** of 2–3 editorial pages: alphabet + æ ø å, vowel length,
  *kj/skj/sj*, silent letters, and a descriptive toneme note. Treated as course
  section, its long-form pages carry VG Wort marks.
- **Locale strings.** Norwegian date/number formatting via `languageCode = "nb"`;
  keep the shared boulingua i18n keys, add Norwegian UI strings where the theme
  surfaces them.

---

## 3. Instantiation from pagegen

Stand up the buildable site by copying the template and changing only the marked values.

1. **Copy the template.** Instantiate `pagegen` into this repo's working tree
   (hugo.toml, layouts, archetypes, scripts, content skeleton, `.github/workflows/`,
   data/, brand/, legal pages), preserving the existing `nsf/LICENSE`, `README.md`
   and `brand/`.
2. **Edit `hugo.toml`** (only the marked values):
   - `baseURL = "https://boulingua.github.io/nsf/"`
   - `title = "Norsk som fremmedspråk — S. Le Boulanger"`
   - `languageCode = "nb"` · `defaultContentLanguage = "nb"`
   - `[params].navTitle = "Norsk"`
   - `description` = Norwegian-course one-liner · `keywords = "norsk,bokmål,CEFR,curriculum,OER,norwegian"`
   - `[[params.social]].url = "https://github.com/boulingua/nsf"`
   - `[params.plausible].domain = "boulingua.github.io/nsf"`
   - `[params].code = "nsf"` — selects the accent from `data/accents.yaml`
   - `[[menu.main]]` — replace example sections with the real ones:
     Uttale, Nivå A1, Nivå A2, Nivå B1, Materials, About, Legal (keep the plausible
     sub-table **last**, per the TOML sub-table trap warning).
3. **Regenerate the brand mark.** Confirm `data/accents.yaml` carries
   `code: nsf → accent: "#6D8618"` (it already does), then run
   `python brand/make_icon.py` to regenerate the pentagon icon + favicons from the
   accent. Verify against the existing `nsf/brand/icon.svg`.
4. **Fill legal placeholders.** Replace the ⟨…⟩ placeholders in `impressum`,
   `datenschutz`, `haftungsausschluss`; keep the VG Wort METIS disclosure in
   Datenschutz. Once filled, drop the `|| true` on the legal-placeholder gate.
5. **First green build.** `hugo --minify --gc` clean; run the gate battery locally
   (`scripts/verify_*`); push to `main`; confirm `build-deploy.yml` deploys to
   **GitHub Pages** at the baseURL. This is the MVP-0 milestone (empty but live).

---

## 4. Curriculum conformance target

- **Target level: `core` (A1–B1).** Implement **every in-scope scale that carries an
  official CEFR descriptor at A1, A2 or B1**; declare B2/C1/C2/Pre-A1 as out of the
  current target. A cell the CV leaves empty is satisfied by recording
  `no-official-descriptor` — a silently missing scale is the only real failure.
- **Descriptor IDs.** Every Norwegian can-do maps to a framework ID
  `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` drawn from `curriculum/schema/scale-registry.yml`
  (domains REC/PROD/INT/MED/PLUR/LING/SOC/PRAG; SIGN out of scope). Units carry these
  as `curriculum.framework: cefr`, `cefr_level`, `cefr_can_do`, and reference the IDs.
- **In-scope vs no-descriptor plan.** Prioritise the activity scales that anchor
  A1–B1 teaching (REC oral/reading comprehension, PROD oral/written production, INT
  conversation/information-exchange/written-interaction, LING general range, SOC
  appropriateness, PRAG). Mediation (MED) and plurilingual (PLUR) scales get **at least
  the descriptors the CV defines at these levels**; where the CV has none, record
  `no-official-descriptor` rather than skip.
- **Machine-readable scope/coverage manifest.** Publish `conformance.yml` in the repo
  (modelled on `curriculum/examples/de-a1/conformance.yml`): `framework`,
  `framework_version`, `language: nb`, `declared_conformance: core`, and a `realizations`
  list mapping each Norwegian `implements_id` to its Bokmål realization. Add a coverage
  manifest listing every implemented scale and, per scale, levels covered vs
  `no-official-descriptor`.
- **Audit gate.** Every `implements_id` must resolve to a real (scale, level) in the
  framework; wire `curriculum/scripts/id-audit.sh` into CI (or a pre-commit check) so
  format, global uniqueness and resolvability all pass before a unit merges.

---

## 5. Content creation plan

Phased by CEFR level. Effort tags: **S** ≈ ≤1 day, **M** ≈ 2–4 days, **L** ≈ ≥1 week.
Recurring cast/theme: a small ensemble in **Oslo/Bergen across the seasons** (e.g.
*Maja*, *Jonas*, an exchange pupil, a *hytte* trip) reused across units for continuity
and vocabulary spiralling.

- **Phase 0 — Uttale onboarding (script/pronunciation stage).** 2–3 editorial pages:
  alphabet + æ ø å, vowel length & minimal pairs, *kj/skj/sj/rs*, silent letters,
  descriptive toneme note; each with audio. **Acceptance:** pages ≥1800 chars where
  marked, audio renders, æ/ø/å correct in both themes. **Effort: M.**
- **Phase A1 — 12 units + 1 exam.** Greetings & introductions, numbers/dates, family,
  home & *hytte*, food/shopping, daily routine, weather & seasons, town/directions,
  free time, present tense & word order (V2), *en/ei/et* gender basics, first
  Scandinavian-neighbours awareness unit. **Acceptance:** each unit maps to A1
  descriptor IDs; five-step structure; answer keys + teacher notes; committed deck +
  worksheet + audio; exam is a first-class sibling bundle. **Effort: L.**
- **Phase A2 — 12 units + 1 exam.** Past tenses (preteritum/perfektum), travel,
  health, work & school, plans & future, comparatives, subordinate-clause word order,
  culture (17. mai, friluftsliv), a receptive Danish/Swedish reading unit.
  **Acceptance:** as A1, at A2 descriptor IDs. **Effort: L.**
- **Phase B1 — 12–14 units + 1 exam.** Opinions & argument, media & environment,
  the *hytte*/nature culture, formal vs informal register, longer narratives,
  passive, connectors; a mediation-focused unit; a Nynorsk-recognition unit.
  **Acceptance:** as above at B1 IDs, incl. at least the CV's B1 mediation descriptors.
  **Effort: L.**
- **Exams as first-class bundles.** One model exam per level (`…-exam/` sibling of the
  last unit, shared `unit_nr`), `page_type: exam`, `duration_min`, `total_points`,
  `notenschluessel`, committed PDF under `static/downloads/<level>/`.
- **Appendices (S–M each).** Glossary (Bokmål↔DE/EN), common errors (German-L1 &
  English-L1 interference), grammar reference (V2, gender, tenses), pronunciation &
  toneme guide, **Scandinavian mutual-intelligibility guide** (Bokmål vs Danish/Swedish),
  Nynorsk awareness note, writing rubrics, teaching workflow.
- **Global acceptance criteria per phase.** Clean `hugo` build; id-audit passes;
  every editorial page ≥1800 chars has a registered VG Wort mark; downloads present
  and attributed; only openly-licensed/public-domain source material, cited.

---

## 6. Website & materials

- **Section landings.** `_index.md` per section (`Uttale`, `Nivå A1/A2/B1`,
  `Materials`) using shortcodes only — `{{< hero >}}`, `{{< kicker >}}`, `{{< lead >}}`,
  `{{< card-grid >}}`/`{{< card >}}` — never raw HTML.
- **Materials pipeline (committed).** Generate decks and worksheets locally from the
  branded **slidegen** (`.odp` + PDF) and **sheetgen** (PDF worksheets) LaTeX
  templates using the `nsf` accent; commit under `static/materials` and
  `static/downloads`; front matter references `presentation`/`worksheet` `{file,
  thumbnail}`. CI only verifies — no TeX Live in the deploy path.
- **Native-voice audio (audiogen/Piper).** Add the Norwegian voice to
  `get_voices.sh`, then
  `python build_audio.py /path/to/nsf voices/no_NO-talesyntese-medium.onnx nb 'content/*/units/*/index.md'`.
  Output OGG/Opus under `static/materials/audio/<unit>/` + `data/audio/<unit>.json`;
  the unit layout renders a **„Lytt" (Listen)** player with transcript beneath (audio
  **and** text, for accessibility). **Decision:** ship audio only where the voice
  passes the æ/ø/å + minimal-pair smoke test; otherwise transcript-only and log the gap.
- **Thumbnails.** `scripts/render_thumbs.py` for deck/worksheet previews; committed.
- **Downloads.** Surfaced via the standard end-of-article downloads render; verified
  by `verify_downloads.py` (present + attributed).

---

## 7. VG Wort — pixel assignment for ALL content pages

**Required and non-skippable.** Every content page ≥1800 rendered characters — every
unit, every exam, every appendix, and long-form editorial pages (About/course
overview, the Uttale pages, the Scandinavian guide) — gets its **own** VG Wort
Zählmarke, one mark per work on exactly one URL, per `pagegen/docs/vgwort-standard.md`.

- **Codes.** Draw **fresh public codes** (32-hex) from the author's **T.O.M.** account —
  never invent codes, never expose the private code.
- **Register.** Add each in `data/vgwort.yaml` keyed by `url:` (base-stripped
  `RelPermalink`) or `path:` (`content/<File.Path>`), with `public_id`/`pixel_url`,
  `min_chars: 1800`, `author`, `registered_at`.
- **Render.** Via the single shared resolver `layouts/_partials/vgwort/url.html` —
  `<head>` preload + eager body `<img>` (no JS, no consent gate, `loading="eager"`,
  hidden via `visibility:hidden`/off-screen, direct `met.vgwort.de`).
- **Never mark.** Home, materials hub, tag/category indexes, `/page/N/`, and the three
  templated legal pages.
- **Record + verify.** Log each mark in the private usage registry (outside the repo);
  CI gates: coverage audit (warns on unmarked ≥1800-char pages), render verify
  (blocking — each registered pixel appears on exactly one URL), hub guard (blocking —
  no pixel on the materials hub).
- **Estimated total.** ≈ **50 marks**: ~3 Uttale + (12+1)×3 levels = 39 units/exams +
  ~8 appendices + ~2 editorial (About/overview) ≈ **50 pages**. Draw a small buffer
  (~55) from T.O.M. to cover splits/late appendices.

---

## 8. Milestones & sequencing

1. **M0 — Site live (empty).** Instantiate from pagegen (§3), first green build, Pages
   deploy. Legal pages filled. *Dep: none.* **S–M.**
2. **M1 — Voice + pipeline proven.** Norwegian Piper voice sourced, verified, wired into
   audiogen; slidegen/sheetgen produce an `nsf`-branded sample deck + worksheet;
   thumbnails render. *Dep: M0.* **M.**
3. **M2 — Uttale stage + conformance skeleton.** Pronunciation section published;
   `conformance.yml` + coverage manifest committed; `id-audit.sh` green in CI;
   VG Wort marks registered for the Uttale pages. *Dep: M1.* **M.**
4. **M3 — A1 MVP live (flip candidate).** Full A1 level (12 units + exam), materials,
   audio, appendices-so-far, all A1 marks registered, all gates green. *Dep: M2.* **L.**
5. **M4 — A2 complete.** *Dep: M3.* **L.**
6. **M5 — B1 complete + `core` conformance declared.** All appendices done; coverage
   manifest shows every in-scope A1–B1 scale implemented or `no-official-descriptor`.
   *Dep: M4.* **L.**

**Definition of done / ready to flip from *coming soon* to *active* on the world map:**
- At minimum **A1 fully live** (M3): section landings, 12 units + model exam, committed
  decks/worksheets, native-voice audio (or logged transcript-only), appendices for A1.
- `hugo --minify --gc` clean; full gate battery green (VG Wort coverage/render/hub,
  downloads, legal placeholders filled, attribution); `id-audit.sh` passes.
- `conformance.yml` present with `declared_conformance` and a published coverage manifest.
- Every ≥1800-char page carries a registered VG Wort mark logged in the usage registry.
- README status updated from *scaffold* to active; the boulingua hub world-map entry
  flipped. (Full `core` completion is M5; the map flip may happen at A1 with the scope
  honestly stated.)

---

## 9. Open decisions & risks (language-specific)

- **Bokmål vs Nynorsk** — *decided: Bokmål*, Nynorsk awareness-only. Risk: school
  contexts that require Nynorsk exposure; mitigate with the recognition appendix/unit.
- **Piper Norwegian voice quality** — the shipped set has none; NB Talesyntese must be
  verified for æ/ø/å and naturalness. Risk: poor quality blocks audio. *Mitigation:*
  transcript-only fallback, never block a unit; log the gap.
- **Scandinavian world-map credit** — the map credits Norwegian across DK/SE. Risk of
  over-claiming. *Mitigation:* explicit receptive-Scandinavian units + a
  mutual-intelligibility appendix scoping the claim as *receptive*, not productive.
- **Dialect commitment** — no spoken standard exists. *Decision:* Eastern/Oslo Bokmål
  for audio, stated openly; note major dialect variation without teaching it at A1–B1.
- **Tonemes** — descriptive-only at A1–B1; risk of overwhelming beginners if pushed too
  early. Keep in Uttale + audio, not in graded production.
- **Mediation/plurilingual thinness** — likely `no-official-descriptor` gaps and few
  B1 mediation descriptors. *Mitigation:* declare gaps honestly in the manifest; do not
  lower the framework to hide them.
- **B2/C1 scope** — deferred. Risk of scope creep; keep `core` firm until A1–B1 is
  complete and stable, then reassess `full`.
