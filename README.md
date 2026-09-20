# CSE321 / CSE331 / CSE370 — Sem 9 (Summer 2026) Course Resources

Study resources I put together for Operating Systems (CSE321), Theory of Computation (CSE331), and Database Systems (CSE370) at BRACU, shared for juniors taking these courses next.

**Read this file before digging into anything else — it'll save you time and explain what you're looking at.**

## What this is (and isn't)

This is one student's personal resource archive, shared informally. It is not an official BRACU resource, not endorsed by any faculty member, and not guaranteed to be accurate for your section, semester, or faculty — course content, faculty assignments, and policies change. See the Caution section at the bottom.

## How it's organized

- `CSE321/`, `CSE331/`, `CSE370/` — one folder per course. Not every course has every folder below (e.g. CSE331 has no `slides/` since none were provided; CSE370 has no `pyq/` since none exist for this course/section).
  - `topics/` — topic maps, dependency graphs, and the course's own index page
  - `concepts/` (CSE321, CSE370) / `lecture-notes/` (CSE331) — actual study notes: pre-filled module notes for CSE321, full lecture-by-lecture transcriptions for CSE331 (which has no faculty slides), topic concept notes for CSE370
    - **Credit (CSE331):** lecture notes 1–6 are transcribed from Farhan Labib's handwritten notes; lecture 7 onward is from my own notes.
    - **Coverage caveat (CSE321, CSE370):** these concise notes only exist for the post-midterm material. I didn't realise I needed them until after the midterm, so the pre-midterm topics don't have one. For those, rely on the faculty slides, the `resources/` mapping, and the topic maps instead.
  - `resources/` — textbook chapter / practice sheet mapping per topic (links out to free textbooks, doesn't host commercial ones)
  - `practice/` — faculty-issued or faculty-shared practice sheets (verified to be original material, not textbook excerpts). CSE321 has the questions only, with no solutions.
  - `assignments/` — the faculty-issued assignment question PDFs only (CSE321 theory, CSE331 Parts A/B, CSE370 assignments 1–3). No solutions or submissions.
  - `docs/` — faculty-issued course documents: course outlines and schedule, plus CSE321's exam questionnaire preparation strategy and CSE331's additional-resources list
  - `lab/` (CSE321, CSE370) — faculty lab material. CSE321: lab outline, `lab-assignments/` (the two prompts), `lab-tasks/` (handouts, example code and practice problems for labs 0.1–6, plus the lottery-scheduling reading materials from lab 6: an OSTEP chapter and a UCR lecture deck), `project/` (the SimpleFS term-project specification, the supplementary animated walkthrough — the spec is authoritative if they differ — and a printout of the submission form's instructions). CSE370: `lab-assignments/` (the three prompts), `sheets/` (lab sheets and their data) and the lab submission template. Lab slides for CSE370 are under `slides/lab/`.
  - `pyq/` — previous-question pattern analysis, plus the actual PYQ PDFs where available (CSE370 doesn't share PYQs — confirmed, not an oversight)
  - `slides/` — faculty lecture slides, where shared
  - `mock-exams/` — self-built mock question sets
  - `revision-guides/` — consolidated per-exam revision guides
  - `{code}_exam_notes.md` (course root) — assessment-creator map (central vs. faculty), grading policy notes, and per-quiz/exam pattern breakdowns
  - `reference-demos/` (CSE370 only) — a lab faculty's own demo project, shared for reference
- A note on `.md` files: many faculty documents were originally Word/ODT files. Where they appear here as `.md`, they are text transcriptions made with AI assistance so GitHub can display them. The wording is the faculty's, but check exact numbers and figures against the original if it matters.
- **My xv6 lab work:** [zareef-zunaid38/xv6-riscv](https://github.com/zareef-zunaid38/xv6-riscv) — a fork of MIT's xv6-riscv where I worked through the priority-scheduler exercise from the Lab 5/6 handouts (`lab-tasks/lab05`, `lab-tasks/lab06` here). Treat it as a reference for how the exercise can be approached, not as an answer key.
- `task-history/` — my actual session/task log for the semester: real quiz and exam scores, what went wrong, why, and what I'd do differently. Included deliberately, not despite being unflattering in places — one person's mistake is another person's shortcut around the same mistake. Read it as a list of specific, avoidable failure patterns, not as a benchmark to compare yourself against.

## Caution

1. People and policies change. Don't assume anything here — faculty grading style, syllabus scope, PYQ availability — still holds for your section or semester.
2. This is one person's subjective experience. Yours may differ significantly. Feel free to reach out and disagree.

## Questions / corrections

DM me directly, or open an issue on this repo.
