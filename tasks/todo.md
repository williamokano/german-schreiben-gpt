# Deutsch Schreibtrainer — Build Plan

## Architecture decisions (made before generation)

- [x] Instructions.md must fit the GPT Builder's 8,000-character hard limit → behavior only, maximally dense, with routing pointers to knowledge files
- [x] 7 knowledge files, grouped by retrieval co-occurrence (things used together live together), not by human taxonomy
- [x] Personas folded into the exercise system file (they are only ever used while generating exercises)
- [x] Error taxonomy gets error codes shared across hints, corrections, and pattern detection — one vocabulary system-wide
- [x] Example sessions double as few-shot demonstrations (knowledge) and repo examples (deliverable)
- [x] Session state handled via an explicit "session card" convention (GPTs have no cross-conversation memory)

## Files

- [x] gpt/Instructions.md (≤ 8,000 chars, verified with wc -c)
- [x] gpt/knowledge/01-teaching-method.md
- [x] gpt/knowledge/02-cefr-levels.md
- [x] gpt/knowledge/03-exam-guide.md
- [x] gpt/knowledge/04-exercise-system.md
- [x] gpt/knowledge/05-error-taxonomy.md
- [x] gpt/knowledge/06-redemittel-text-structures.md
- [x] gpt/knowledge/07-example-sessions.md
- [x] docs/architecture.md
- [x] docs/builder-setup.md
- [x] docs/testing-checklist.md
- [x] README.md

## Review

See final summary in conversation. Instructions.md verified under the 8,000-character limit.
All knowledge files complete — no placeholders, no TODOs. Exam formats described from
training knowledge with an explicit "verify against official sites" maintenance note in
docs/builder-setup.md, since providers revise formats.
