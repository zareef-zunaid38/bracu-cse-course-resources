# CSE321 / CSE331 / CSE370 — Sem 9 (Summer 2026) Course Resources

Study resources I put together for Operating Systems (CSE321), Theory of Computation (CSE331), and Database Systems (CSE370) at BRACU, shared for juniors taking these courses next.

**Read this file before digging into anything else — it'll save you time and explain what you're looking at.**

## Start here

The revision guides can be read straight in the browser on the [project site](https://zareef-zunaid38.github.io/bracu-cse-course-resources/). For everything else, pick your course. Each has its own guide that tells you what to read first, in order, and links to everything else.

| Course | Guide | Jump straight to |
|---|---|---|
| **CSE321** — Operating Systems | [CSE321/README.md](CSE321/README.md) | [exam notes](CSE321/cse321_exam_notes.md) · [past papers](CSE321/pyq/pyq_index.md) · [revision guides](CSE321/revision-guides) · [lab guide](CSE321/lab/README.md) |
| **CSE331** — Theory of Computation | [CSE331/README.md](CSE331/README.md) | [exam notes](CSE331/cse331_exam_notes.md) · [past papers](CSE331/pyq/pyq_index.md) · [revision guides](CSE331/revision-guides) · [lecture notes](CSE331/lecture-notes) |
| **CSE370** — Database Systems | [CSE370/README.md](CSE370/README.md) | [exam notes](CSE370/cse370_exam_notes.md) · [slides](CSE370/slides) · [revision guides](CSE370/revision-guides) · [lab guide](CSE370/lab/README.md) |

And for all three: [`task-history/`](task-history/tasks-history.md) — my real quiz and exam scores, what went wrong, and why.

## What this is (and isn't)

This is one student's personal resource archive, shared informally. It is not an official BRACU resource, not endorsed by any faculty member, and not guaranteed to be accurate for your section, semester, or faculty — course content, faculty assignments, and policies change. See the Caution section at the bottom.

## Viewing the files

- **`.md` files** display normally on GitHub. Many faculty documents were originally Word/ODT files. Where they appear here as `.md`, they are text transcriptions made with AI assistance so GitHub can display them. The wording is the faculty's, but check exact numbers and figures against the original if it matters.
- **`.html` files** (the revision guides, and the CSE321 SimpleFS walkthrough) are meant to be read as web pages. GitHub shows them as source code if you open the file here, so use the links in the course guides instead, or the [project site](https://zareef-zunaid38.github.io/bracu-cse-course-resources/), which opens them properly in your browser. They are designed for phones too. If you have no connection, click the file here, choose **Download raw file**, and open the saved file in your browser.
- **`.pdf` and `.pptx` files** open or download from GitHub as usual.

## How each course folder is organised

Every course folder uses the same layout, so once you know one you know all three. Not every course has every folder (e.g. CSE331 has no `slides/` since none were provided; CSE370 has no past papers since none exist for this course/section).

**Study material I wrote**

- `topics/` — topic maps, dependency graphs, and the course's own index page
- `concepts/` (CSE321, CSE370) / `lecture-notes/` (CSE331) — actual study notes: module notes for CSE321, full lecture-by-lecture transcriptions for CSE331 (which has no faculty slides), topic concept notes for CSE370
  - **Credit (CSE331):** lecture notes 1–6 are transcribed from Farhan Labib's handwritten notes; lecture 7 onward is from my own notes.
  - **Coverage note:** CSE370's concept notes only cover the post-midterm topics (19–24) — I didn't realise I'd need them until after the midterm, so for earlier topics use the faculty slides, the `resources/` mapping and the topic map. CSE321's module notes cover all eight modules: modules 1–4 (the midterm topics) were written in June, modules 5–8 (post-midterm) are drafts.
- `resources/` — textbook chapter / practice sheet mapping per topic (links out to free textbooks, doesn't host commercial ones)
- `{code}_exam_notes.md` (course root) — who writes each assessment (central vs. faculty), grading policy notes, and per-quiz/exam pattern breakdowns
- `mock-exams/` — self-built mock question sets
- `revision-guides/` — consolidated per-exam revision guides (HTML; see "Viewing the files")
- `pyq/` — previous-question index and pattern analysis, plus the papers themselves where available. CSE331 also has `pyq/other-faculty-assignments/`: past assignment sheets from other faculty sections, useful for practice but not written by TNF.

**Faculty material (not mine)**

- `slides/` — faculty lecture slides, where shared
- `practice/` — practice sheets. Faculty-issued or faculty-shared for the most part (original material, not textbook excerpts); CSE321 has the questions only, with no solutions. CSE331's folder also holds two study aids I compiled myself, which say so in their front matter.
- `assignments/` — the faculty-issued assignment question PDFs only (CSE321 theory, CSE331 Parts A/B, CSE370 assignments 1–3). No solutions or submissions.
- `docs/` — faculty-issued course documents: course outlines and schedule, plus CSE321's exam questionnaire preparation strategy and CSE331's additional-resources list
- `lab/` (CSE321, CSE370) — faculty lab material; see the lab guide in each course
- `shared-notes/` (CSE331 only) — Turing Machines and Reducibility lecture notes that TNF shared with the class, with authors credited in `resources/cse331_resources.md`
- `reference-demos/` (CSE370 only) — a lab faculty's own demo project, shared for reference

**Other**

- **My xv6 lab work:** [zareef-zunaid38/xv6-riscv](https://github.com/zareef-zunaid38/xv6-riscv) — a fork of MIT's xv6-riscv where I worked through the priority-scheduler exercise from the Lab 5/6 handouts (`lab-tasks/lab05`, `lab-tasks/lab06` in [CSE321/lab](CSE321/lab/README.md)). Treat it as a reference for how the exercise can be approached, not as an answer key.
- [`task-history/`](task-history/tasks-history.md) — my actual session/task log for the semester: real quiz and exam scores, what went wrong, why, and what I'd do differently. Included deliberately, not despite being unflattering in places — one person's mistake is another person's shortcut around the same mistake. Read it as a list of specific, avoidable failure patterns, not as a benchmark to compare yourself against.

## Caution

1. People and policies change. Don't assume anything here — faculty grading style, syllabus scope, PYQ availability — still holds for your section or semester.
2. This is one person's subjective experience. Yours may differ significantly. Feel free to reach out and disagree.

## Questions / corrections

DM me directly, or open an issue on this repo.
