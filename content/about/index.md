---
title: "About this course"
author: "S. Le Boulanger"
date: 2026-09-07
description: "What Norsk som fremmedspråk is, why it teaches Bokmål, how it is aligned to the CEFR, what it deliberately leaves out, and how the materials are made."
---

## What this site is

**Norsk som fremmedspråk (NSF)** is a free, openly-licensed Norwegian course
for the German *Gesamtschule* system and for independent adult learners. It is
one of the language courses on the [boulingua](https://github.com/boulingua)
platform and follows the same standard as its sister sites for German, English
and French: one page bundle per unit, a model examination as a first-class
sibling of the last unit in each level, downloadable open-format materials, and
native-voice audio with a transcript beside every recording.

The course covers **CEFR A1, A2 and B1**, preceded by a short pronunciation
stage. The written variety is **Bokmål**. The spoken norm behind the audio is
**Eastern (Oslo) Bokmål**.

## Who it is written for

Two audiences, and the course tries not to pretend they are one.

The first is a **school audience**: pupils taking Norwegian as a third or
further foreign language, and the teachers who prepare those lessons. For them
the unit is a lesson-shaped object — five moves, a deck, a worksheet, an answer
key, teacher notes on timing and differentiation — and the model exam is the
thing the term points at.

The second is the **independent adult learner** working alone. For them the
same page has to be readable end to end without a classroom around it, which
is why every unit is a complete article rather than a set of prompts, why the
answer keys are on the page, and why the audio always ships with its written
text.

The classroom metalanguage is German, with English glosses where an English
equivalent is clearer than a German one. The target language is Norwegian from
the first unit.

## Why Bokmål

Norwegian has two official written standards, Bokmål and Nynorsk, and a
foreign-language course has to choose. Carrying both would double every unit,
every deck and every audio file, and would halve the quality of each.

The course teaches **Bokmål**: it is the written form of roughly 85–90 % of
Norwegians, the default in most textbooks and national media, and by a wide
margin the better-resourced of the two for speech synthesis. Nynorsk is not
ignored — it is treated as a **recognition** skill, with a B1 unit and an
appendix that explain what it is and give a Bokmål reader the correspondences
needed to read it. Nobody is asked to write it.

## Why Eastern Bokmål for the audio

Norway has no spoken standard, and is unusually comfortable with that:
broadcasters, teachers and politicians use their own regional varieties in
public, and no institution arbitrates. A course that ships audio nevertheless
has to record something.

This one records **Eastern (Oslo) Bokmål**, because it is the spoken variety
closest to the written norm being taught and the one a learner will meet most
in national broadcasting — and it says so rather than presenting the choice as
neutral. Where a major dialect difference is one a learner will actually
notice, the pronunciation material names it: the uvular *r* of the south-west,
the retroflex "thick" *l* inland, the diphthongs retained in the west.

## The tonemes

Norwegian has two pitch accents that distinguish real word pairs — *bønder*
and *bønner*, *tanken* and *tanken* — and they are not written. The course
treats them **descriptively**: they are explained in the pronunciation stage,
audible in every recording, and covered properly in the pronunciation
appendix. They are never a graded production requirement at A1, A2 or B1.
Learners at these levels benefit from knowing Norwegian has a melody; they do
not benefit from being marked down for not producing it.

## The Scandinavian thread, scoped

Written Bokmål is largely readable to Danish and Swedish readers, and the
reverse holds. That is genuinely useful, and the course builds it deliberately:
a neighbour-languages unit at A1, a receptive Danish and Swedish reading unit
at A2, a Nordic unit at B1, and an appendix that sets out the systematic
correspondences and the *falske venner*.

The claim is **receptive, not productive**. Reading a Swedish notice or a
Danish recipe after a year of Norwegian is a realistic outcome and the course
teaches towards it. Speaking Danish is not, and the course does not imply
otherwise. Spoken Danish is difficult for Norwegians themselves, and pretending
otherwise would be the easiest way to make the whole thread untrustworthy.

## The five-step unit

Every unit walks the same path: **Activate → Input → Practise → Apply →
Reflect**. Activate surfaces what the learner already has; Input presents the
model text, dialogue or grammar; Practise rehearses it under control; Apply is
the productive task the differentiation scaffolds; Reflect returns the learner
to the can-do statements the unit was written against.

The shape is fixed on purpose. A predictable structure lets a learner know
what is coming and lets a teacher substitute material at any step without
redesigning the lesson.

## Curriculum alignment

The course is aligned to the CEFR through the shared boulingua curriculum
framework, which is grounded in the **Companion Volume (2020)**. Every unit
declares `curriculum.framework: cefr`, its `cefr_level`, and `cefr_can_do`
statements that reference descriptor IDs of the form
`{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}`.

The declared conformance target is **`core` — A1 to B1**. That means every
in-scope scale carrying an official descriptor at A1, A2 or B1 is implemented.
Where the Companion Volume leaves a cell empty, the course records
`no-official-descriptor` rather than inventing a descriptor to fill it: a
declared empty cell is honest, a silently missing scale is a defect. The
mediation and plurilingual scales are the thinnest part of the framework at
these levels, and the coverage manifest shows exactly where.

**B2 and C1 are outside the current target.** The reason is supply, not
ambition: openly-licensed and public-domain Norwegian material suitable for
B2 work is scarce, and a level built on thin sources would serve nobody. The
scope will be reconsidered once A1 to B1 is complete and stable.

## Materials and audio

Slide decks and worksheets are generated locally from the branded LaTeX
templates of the boulingua *slidegen* and *sheetgen* projects, in this
course's signature colour, and are **committed** to the repository as editable
`.odp` files and printable PDFs. Continuous integration verifies that the
downloads exist and are attributed; it never builds them, so no TeX
installation sits in the deployment path.

Audio is synthesised with an openly-licensed native Bokmål voice and published
as OGG/Opus, one file per segment, each with its written transcript displayed
beneath the player. Audio is never the only carrier of content: a learner
without sound, or a classroom without speakers, loses nothing but the sound.
Where a segment does not clear the *æ/ø/å* and minimal-pair check, it ships as
transcript only and the gap is logged rather than quietly dropped.

## Sources

Only public-domain or openly-licensed source material is used, and it is cited
at the point of use. No copyrighted text, image or recording is reproduced.
Where a cultural topic needs a real artefact — a timetable, a notice, a public
information page — the course uses one that may lawfully be reused, or writes
its own.

## Licence and reuse

- **Code** (layouts, scripts, configuration): **MIT**.
- **Teaching content** (units, exams, appendices, materials): **CC BY 4.0**.

Attribute to *S. Le Boulanger*. Adapt it, translate the glosses, print the
worksheets, run it in a classroom that is nothing like the one it was written
for. That is what the licence is for.

## Legal

[Impressum](/impressum/) · [Datenschutz](/datenschutz/) ·
[Haftungsausschluss](/haftungsausschluss/)
