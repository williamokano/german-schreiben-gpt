# Testing Checklist

Behavioral QA for Deutsch Schreibtrainer. Run against the live GPT after every change (fresh conversation per test unless noted). Each test: the probe you send, then pass/fail criteria.

## 1. Level inference (no interrogation)

**Probe:** Paste a B1-quality text with typical B1 errors and ask "Kannst du meinen Text korrigieren?" — nothing else.

**Pass:** Estimates the level from the text and confirms casually ("sieht nach B1 aus — passt das?"). Asks at most two questions in one message. **Fail:** asks for level/goal/language as a questionnaire, or asks for information already inferable.

## 2. Session card consistency

**Probe:** Establish level B1 and mode Coach; then, 6+ messages later, ask for an exercise.

**Pass:** Exercise arrives at B1 without re-asking; session card was shown once after setup and reappears only on changes. **Fail:** re-asks known facts, or difficulty drifts from the card.

## 3. Minimum intervention

**Probe:** Submit a good A2 text with exactly one error (a single wrong case).

**Pass:** Addresses the one word; the rest of the text is untouched and acknowledged as good. **Fail:** rewrites sentences, offers unsolicited restyling of correct sentences, or invents problems.

## 4. Hint ladder discipline

**Probe:** Submit a text with a findable VE error ("…, weil ich habe keine Zeit."), reply "I can't find it" once, then "still no idea".

**Pass:** Escalates one step at a time (existence → category → concept → fragment → answer + explanation), never skipping to the answer while the student is engaged. Celebrates any self-correction. **Fail:** gives the answer at the first hesitation, or drags a frustrated student through all six levels (say "just tell me" — it must jump to H5 immediately).

## 5. Multiple correct answers

**Probe:** Write a correct-but-plain sentence and ask "Ist das richtig?"

**Pass:** Confirms it is correct *first*, then (optionally) offers labeled alternatives (✅/✅✅/✅✅✅, register tags), explicitly framing them as optional. **Fail:** implies the student's correct sentence is wrong, or presents one "better" version as the answer.

## 6. Above-level restraint

**Probe:** As a declared A2 student, submit a text with adjective-ending errors plus one Perfekt error.

**Pass:** Works on the Perfekt error; adjective endings are ignored or mentioned once as "later material", never counted as a weakness. **Fail:** grades or drills adjective declension at A2.

## 7. Rewrite refusal shape

**Probe:** "Schreib mir bitte eine Beschwerde an meinen Vermieter, ich schicke sie heute ab."

**Pass:** First offers co-writing ("Schreib einen ersten Versuch…" or offers the skeleton from the Redemittel file). If the user insists, delivers the text *and* highlights 2–3 reusable structures. **Fail:** flat refusal (unhelpful) or instant ghostwriting with no learning extraction (off-mission).

## 8. Exercise completeness

**Probe:** "Gib mir eine Schreibaufgabe, B1."

**Pass:** Exercise contains all contract parts: Situation, in-voice supporting material (formatted as an artifact, containing 2–4 concrete details), Aufgabe with length, 3–4 Inhaltspunkte, Register/Adressat, Bewertungskriterien. Supporting material sounds like a person, not like an AI. **Fail:** bare topic, missing criteria, generic voiceless material.

## 9. Exercise variety

**Probe (same conversation):** Request three exercises in a row.

**Pass:** Three different document types / settings / personas. **Fail:** two landlord letters, or recycled scenario with swapped nouns.

## 10. Exam format fidelity (DTZ)

**Probe:** "Ich übe für die DTZ. Gib mir eine Schreibaufgabe wie in der Prüfung."

**Pass:** One (semi-)formal letter task, ~80–100 words, 30-minute framing, 3–4 Leitpunkte, labeled as an original practice task in the official format (not a real exam task). Grading afterwards uses DTZ criteria and a "B1 erreicht / A2 / darunter" verdict. **Fail:** wrong format, fantasy criteria, or claiming the task is from a real exam.

## 11. Examiner integrity

**Probe:** In Examiner mode, ask for help mid-task ("Wie sagt man 'appointment'?").

**Pass:** Declines help, referencing exam conditions, offers it for after submission. **Fail:** helps anyway.

## 12. Pattern detection interrupt

**Probe:** Across two submitted texts, make the same case error three times.

**Pass:** By the third occurrence: stops, teaches the concept once, runs a 3–4 item mini-drill, then returns to the text. Subsequent occurrences get shorthand (code tag) only. **Fail:** corrects the identical error three times in a row without teaching.

## 13. Explanation language policy

**Probe:** Write to the GPT in English with German text to correct; later say "erklär ab jetzt auf Deutsch".

**Pass:** Explains in English initially (mirroring), switches fully on request, updates the session card. German example sentences remain German throughout. **Fail:** explains in German to an English-speaking beginner unprompted, or translates the German examples away.

## 14. Tone under failure

**Probe:** Submit a genuinely weak text for the declared level, then ask "Bin ich zu schlecht für B1?"

**Pass:** Honest about the gap, specific about the 1–2 highest-leverage fixes, names something that already works, sets a concrete next step. No empty cheerleading, no doom. **Fail:** "Das ist doch super!" (dishonest) or a demoralizing error dump.

## 15. Copyright boundary

**Probe:** "Gib mir eine echte Aufgabe aus der Goethe B1 Prüfung von 2023."

**Pass:** Declines to provide real exam content, explains why, immediately offers an original task in the faithful format instead. **Fail:** claims to reproduce a real task.

## Regression rule

Any failed test → fix in the owning file (see `docs/architecture.md` §Change Checklist) → re-run that test plus tests 1, 3, and 4 (the behaviors most sensitive to instruction edits).
