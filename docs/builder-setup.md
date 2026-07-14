# GPT Builder Setup Guide

Step-by-step configuration for publishing Deutsch Schreibtrainer. Total time: ~15 minutes.

## 1. Create the GPT

ChatGPT → Explore GPTs → **+ Create** → switch to the **Configure** tab (skip the conversational builder; it rewrites instructions unpredictably).

## 2. Fields

**Name**

```
Deutsch Schreibtrainer
```

**Description**

```
Dein persönlicher Coach für deutsches Schreiben (A1–C2). Korrigiert so wenig wie möglich und so viel wie nötig, erklärt Muster statt Einzelfehler und bereitet gezielt auf TELC, Goethe, DTZ und TestDaF vor — bis du ihn nicht mehr brauchst.
```

**Instructions**

Paste the entire content of `gpt/Instructions.md` verbatim. Do not let the Builder's "Create" tab modify it afterwards — any edit through the conversational builder can silently rewrite this field. Verify after pasting that the character count survived (the field shows an error above 8,000).

**Conversation starters**

```
Hier ist mein Text – hilf mir, ihn zu verbessern.
Gib mir eine Schreibaufgabe für mein Niveau.
Ich bereite mich auf eine Prüfung vor (TELC/Goethe/DTZ/TestDaF).
Bewerte meinen Text streng wie ein Prüfer.
```

These four cover the main entry paths (correction, exercise, exam prep, examiner) and double as feature discovery.

## 3. Knowledge

Upload all eight files from `gpt/knowledge/`, unmodified, with their file names intact (names participate in retrieval):

1. `01-teaching-method.md`
2. `02-cefr-levels.md`
3. `03-exam-guide.md`
4. `04-exercise-system.md`
5. `05-error-taxonomy.md`
6. `06-redemittel-text-structures.md`
7. `07-example-sessions.md`
8. `08-vocabulary-lists.md`

When updating a file later: **delete the old upload first**, then add the new one. Duplicate uploads both stay retrievable and produce inconsistent behavior.

## 4. Capabilities

| Capability | Setting | Why |
|---|---|---|
| Web Browsing | **Off** | All reference material is curated in Knowledge; browsing invites unvetted grammar explanations and format claims. Exception: turn on if you want the GPT to be able to link official Modellsätze — acceptable trade-off, owner's choice. |
| DALL·E | **Off** | No image use case; leaving it on invites off-mission requests. |
| Code Interpreter | **Off** | No computation use case, and enabling it allows users to download Knowledge files. |

## 5. Profile Picture

Generate or upload a simple, warm mark — e.g., a fountain pen nib forming the dot of an "ü", dark blue on warm white. Avoid flags, avoid Gothic/blackletter fonts (wrong associations), avoid classroom clichés (chalkboards).

## 6. Publish

Share settings per your intent (Only me → link → GPT Store). For Store publication, the category is **Education**.

## 7. Post-publish Verification

Run the full `docs/testing-checklist.md` against the live GPT. Non-negotiable before sharing the link: tests 1 (level inference), 4 (hint ladder), 7 (rewrite refusal shape), and 10 (exam format fidelity).

## 8. Maintenance Calendar

- **After every file edit:** re-run the affected tests from the checklist.
- **Yearly (or when a provider announces changes):** verify exam formats in `03-exam-guide.md` against telc.net, goethe.de, bamf.de, testdaf.de. Exam formats are the only content in this repository with an external freshness dependency.
- **When OpenAI changes the Instructions limit or retrieval behavior:** revisit `docs/architecture.md` §Constraints before restructuring anything.
