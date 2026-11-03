# Acceptance criteria — Provenance Guard

Five criteria that say what "working" means for this service, written in unit 7
**before** you built it.

**All five are yours.** None are given.

An acceptance criterion names a number or something plainly observable. *"The
detector is accurate"* is an opinion — and perfect AI detection isn't possible
anyway, so it's an opinion about something nobody can deliver. *"Of 10 pieces I
wrote myself, at most 1 is labelled high-confidence AI"* is a criterion.

Under each, write a sentence or two on **why that target**. *"I allow at most 1
false positive in 10 because on a writing site a wrong accusation costs more
than a missed detection"* is a real answer.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

**Cover at least three of these five areas.** They're prompts, not a form.

| Area | A question it could answer |
|---|---|
| False positives | How often may a human's writing get called AI? |
| Score spread | How far apart should clearly-AI and clearly-human text score? |
| Label coverage | Is every one of your three labels actually reachable? |
| The appeal path | Does an appeal always change status and always get recorded? |
| Bad input | What happens when `text` is missing, empty, or enormous? |

Two things worth knowing before you pick numbers:

- **The expensive mistake is one-sided.** Missing some AI text costs the site a
  little. Accusing a real writer costs that person a lot. A criterion that
  treats both errors as equally bad is describing a system nobody would ship.
- **Unit 8 tests across three trials, and the target has to hold across all
  three.** Your first signal runs a model, so identical input can score
  differently twice. A target of "at most 1 in 10" against trials of 2, 1, 2 is
  a **miss**.

---
## 1. False positives occur at most 1 in 10 times

**Why this target:**
Wrongly labeling a human's writing as AI is the most harmful mistake this system can make, because it can lead to an unfair accusation. A 10% ceiling is strict enough to protect writers but realistic for a two-signal detector, and the appeal process covers the cases that slip through.

---

## 2. In a diverse sample of 10 entries, each outcome label should appear at least once

**Why this target:**
If one label never shows up across varied inputs, the thresholds are likely miscalibrated, for example everything collapsing into "unsure." Seeing all three labels confirms the score ranges actually separate different kinds of text.

---

## 3. Empty input text or input text exceeding 500 words is rejected 5 of 5 times

**Why this target:**
Input validation is deterministic, so there is no reason for it to ever fail and the target should be 100%. Empty text can't be scored meaningfully, and very long text slows the model signal.

---

## 4. Clearly-AI and clearly-human text have a minimum final score difference of 0.4 4 of 5 times

**Why this target:**
A 0.4 gap is wide enough to move a text from one confident label to the other, which shows the combined score is actually telling the two apart rather than clustering in the middle. Allowing one miss in five accounts for the natural noise in perplexity and style scoring.

---

## 5. An appeal request is logged and returns a decision 5 of 5 times

**Why this target:**
The appeal is the safety net for false positives, so it has to work every time, since a lost appeal leaves a wrongly labeled writer with no recourse. Logging every request also creates a record for reviewing how often and where the detector gets it wrong.

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 8 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Uncertain cases land in the unsure band

         The system handles uncertain cases well.

         **Why this target:** ...

         > **Revised in unit 8:** Text I judged borderline scores between
         > 0.4 and 0.6 in at least 4 of 5 cases.
         >
         > **Why revised:** "well" gave me nothing to check. I couldn't
         > reach a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "At most 1 of 10 of my own pieces flagged as AI" → "at most 3
            of 10", because 1 of 10 turned out to be hard.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
