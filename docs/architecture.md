# Architecture

Design record for the Deutsch Schreibtrainer Custom GPT. Read this before changing anything.

## Platform Constraints (these drove every decision)

1. **Instructions field: 8,000 characters, hard limit.** Instructions are the only text guaranteed to be in context on every turn. Knowledge files are retrieved — not always, not fully.
2. **Knowledge retrieval is chunked semantic search.** The model sees *sections* of files matched against the conversation, not whole files. Retrieval quality depends on: descriptive file names, self-contained sections with keyword-rich headings, and co-location of concepts that are used together.
3. **No runtime, no state, no code.** Session memory exists only inside one conversation. Cross-conversation memory does not exist; anything that must persist within a session needs an explicit textual convention.

## Consequences

### Instructions = behavior contract, Knowledge = reference shelf

`gpt/Instructions.md` (6.5k chars, ~1.5k headroom) contains *only* rules that must hold on every single turn: identity, the intervention ladder, the hint ladder, mode definitions, the correction protocol shape, and a routing table telling the model which knowledge file answers which need. Anything the model needs *sometimes* (grammar reference, exam formats, personas, phrase banks) lives in knowledge, where it is retrieved exactly when the conversation is about it — which is also when retrieval works best.

The routing table in Instructions matters more than it looks: it converts "maybe the model searches knowledge" into "the model knows a file exists for this and searches deliberately."

### Seven knowledge files, grouped by co-use, not by taxonomy

| File | Retrieval trigger (what conversations pull it in) |
|---|---|
| 01-teaching-method | correcting a text, giving feedback, hinting |
| 02-cefr-levels | any level mention, difficulty calibration |
| 03-exam-guide | exam names, "Prüfung", Examiner mode |
| 04-exercise-system | "gib mir eine Aufgabe", exercise generation |
| 05-error-taxonomy | specific German errors in student text |
| 06-redemittel-text-structures | "wie schreibe ich…", phrases, letter conventions |
| 07-example-sessions | tone/format anchoring (retrieved alongside most coaching turns) |

Splits and merges made against the original brief:

- **Personas merged into the exercise system.** Personas are only ever used while generating an exercise; a separate personas file would need to be co-retrieved with the exercise file every time — a retrieval failure mode with zero upside.
- **Hint phrasings merged into the error taxonomy.** A hint is always *about* an error category. Storing "how to hint about Kasus" next to "what a Kasus error is" means one retrieval serves the whole move.
- **No standalone grammar reference.** The GPT is a coach, not a textbook; the error taxonomy contains exactly the grammar needed to explain the errors students actually make, at H5 depth. A full grammar file would dilute retrieval and invite lecturing.
- **No static exercise bank.** The brief asked for infinite generation; a bank would compete with the generator in retrieval and anchor the model on repeating stored items. The three worked examples inside 04 exist as *format* anchors, explicitly framed as such.
- **Example sessions kept as a knowledge file** (not just repo docs) deliberately: retrieved transcripts act as few-shot demonstrations, which shape tone and formatting more reliably than any amount of instruction prose.

### One shared vocabulary: error codes

Codes (V2, VE, K, R, …) defined in 05 are referenced in Instructions, used in feedback templates (01), and drive pattern detection. This is the system's spine: it makes recurring errors *visible and nameable* to the student, and gives the model a compact bookkeeping device inside a stateless conversation.

### Session state via the session card

GPTs cannot store state, but they reliably maintain conventions they can see. The session card (`📋 Niveau: … · Ziel: … · Modus: … · Erklärsprache: …`) is printed into the conversation, making the state part of the context window — self-refreshing memory. Re-showing it after changes keeps it near the context tail where attention is strongest.

## File Format Conventions

- Knowledge files open with one line stating what they are and who reads them — this line is chunk-level context if retrieval slices the top of the file.
- Sections are self-contained: a chunk landing mid-file still makes sense (headings carry the topic; tables carry their own column headers).
- Tables and decision trees over prose wherever the content is enumerable — better chunk density, less room for the model to paraphrase-drift.
- German linguistic content stays German; meta-instructions are English (instruction-following is measurably strongest in English, and the file language does not leak into student-facing output because Instructions set the language policy).

## Known Limits & Mitigations

- **Retrieval can miss.** Core behavior never depends on knowledge; a session with zero retrievals still behaves correctly (less richly).
- **Exam formats drift.** Providers revise exams. The exam guide instructs the model to recommend official Modellsätze once per exam-prep session; `docs/builder-setup.md` schedules a yearly review.
- **Character budget.** Instructions have ~1.5k characters of headroom. Any addition must be paid for; prefer moving detail into knowledge and adding one routing line.

## Change Checklist

1. Behavior change → edit `gpt/Instructions.md`, then `wc -c` it (must stay < 8,000).
2. Reference change → edit the owning knowledge file; keep sections self-contained.
3. New error category → add to 05 (code table + playbook), nothing else needs touching.
4. New exam → add a section + blueprint to 03; add the name to the Instructions exam list.
5. Re-upload changed files in the Builder (delete old version first — the Builder does not deduplicate).
6. Run `docs/testing-checklist.md` before shipping.
