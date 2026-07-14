# Deutsch Schreibtrainer

Source of truth for the **Deutsch Schreibtrainer** Custom GPT — a professional German writing coach (A1–C2) with exam preparation for TELC, Goethe, DTZ, and TestDaF.

Not a translator. Not a grammar checker. Not a text generator. A coach whose goal is to make itself unnecessary: it teaches before it corrects, corrects before it rewrites, and rewrites only on explicit request.

## What Makes It Different

- **Minimum intervention** — fixes at the smallest sufficient scope (word → phrase → sentence → …), preserving the student's voice
- **Progressive hints (H1–H5)** — students find their own errors before being told
- **Honest labeling** — distinguishes incorrect / acceptable / natural / native-like instead of pretending one wording is "the" answer
- **Pattern detection** — teaches a recurring error's concept once instead of correcting it ten times
- **Infinite exercise generation** — archetype dimensions × personas × authentic supporting materials, never a bare topic
- **Faithful exam simulation** — original tasks in the exact format and grading logic of TELC / Goethe / DTZ / TestDaF

## Repository Map

```
gpt/
  Instructions.md                      → paste into the Builder's Instructions field (< 8,000 chars)
  knowledge/                           → upload all 7 files as Knowledge
    01-teaching-method.md              coaching workflows, hint ladder, feedback templates
    02-cefr-levels.md                  A1–C2 profiles: grammar inventories, expectations, micro-challenges
    03-exam-guide.md                   exam formats, rubrics, original-task blueprints
    04-exercise-system.md              generation dimensions, personas, supporting-material rules
    05-error-taxonomy.md               error codes, hint phrasings, concept explanations
    06-redemittel-text-structures.md   text skeletons, phrase banks, register tables
    07-example-sessions.md             annotated model interactions (few-shot anchor + examples)
docs/
  architecture.md                      design decisions and their rationale — read before changing anything
  builder-setup.md                     step-by-step Builder configuration
  testing-checklist.md                 15 behavioral QA tests
```

## Build It (5 steps)

1. ChatGPT → Explore GPTs → **+ Create** → **Configure** tab
2. Paste `gpt/Instructions.md` into Instructions; set name, description, starters from `docs/builder-setup.md`
3. Upload the seven files in `gpt/knowledge/`
4. Disable Web Browsing, DALL·E, and Code Interpreter
5. Run `docs/testing-checklist.md` before sharing

## Maintaining

Every file has one owner-concept; the change checklist in `docs/architecture.md` maps any desired behavior change to the file that controls it. The only content with an external freshness dependency is `03-exam-guide.md` (exam providers revise formats — review yearly).
