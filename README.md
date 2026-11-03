# Provenance Guard

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first.**
>
> Once `python test.py` passes, in one terminal:
>
> ```bash
> python app.py
> ```
>
> and in another:
>
> ```bash
> curl -X POST http://127.0.0.1:5000/ping \
>   -H "Content-Type: application/json" \
>   -d '{"message": "hello"}'
> ```
>
> That's a web service. Something sent it text and it sent something back.
> Everything this unit is filling in the middle.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     Unit 7 asks for the first five sections. Unit 8 adds the five below.

     Everything is pasted as TEXT. A pasted curl response gets full credit;
     a picture of your terminal gets none.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 7 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- The path a submission takes, from arriving to the label a reader sees.
     Six or seven steps — this is the trace you wrote in Milestone 1. -->
A backend service that recieves text and a score of how likely the text was AI-generated.

Example path:
1. A user submits text, which gets sent to the /submit endpoint
2. The text is evaluated and the system generates a predictability and style signal
3. The system calculates an 'AI-likeliness' score using the siganls
4. The system will use the score to label the text as human, unsure, or likely AI then return the label to the user
5. The user can see the label and submit an appeal if they believe it's a false positive

---

## Signals and Scoring

<!-- For each signal: what property it measures, why that might differ between
     human and AI writing, and WHAT IT CAN'T SEE.

     The blind spot is not optional. If you can't name it you don't understand
     the signal yet — and unit 8's attack set is built out of blind spots. -->

### Signal one — the model signal

**What it measures:**
How well the AI can predict parts of the text by analyzing preceeding sections. The resulting perplexity score measures how well the AI predicted the text (low perplexity meaning more predictable)

**Why that might differ between human and AI writing:**
AI writing will tend to have a lower perplexity as it generates texts by 'predicting' the next most-likely word. Human writing can be more unpredictable when ideas are expressed.

**What it can't see:**
Does not analyze the structure or meaning of text.
### Signal two — the style signal

**What it measures:**
Looks at the structure and style of text to determine how likely it is AI-generated.
**Why that might differ between human and AI writing:**
AI usually follows a certain sentence structure and style (such as "this, not that" and use of many adjectives) while human writers can have varied styles, vocabularies, and punctuation.
**What it can't see:**
Does not interpret the meaning of text.
### The combining rule

<!-- Write the rule in words, then give the numbers. What happens when the two
     signals disagree? -->

**The rule:**
Each signal is converted to a score from 0 to 1, where higher means "more AI-like." The final score is a weighted average of the two, with the model signal counting for more because it is the stronger single indicator. If the two signals strongly disagree, the system does not trust either one enough to make a confident call, so the result is held in the "unsure" range no matter what the average says.

**The numbers:**
- Model signal score: perplexity mapped to 0–1 (low perplexity → close to 1)
- Style signal score: 0–1 (more AI-typical patterns → close to 1)
- Combined score = 0.6 × model score + 0.4 × style score
- Disagreement check: if |model score − style score| > 0.4, the combined score is clamped to the unsure range (0.36–0.69)

**Where it lives:** `scoring.py::combine_signals`

<!-- ⚠️ The grader checks your code against that line. If your rule lives
     somewhere else, say where. -->

**A case where my two signals split, and what my rule does with it:**
A student submits a formal lab report they wrote themselves. Lab reports follow a rigid template with standard phrasing ("The results indicate...", "This suggests..."), so the text is very predictable and the model signal scores it 0.85. But the writing has human quirks: uneven sentence lengths, a few comma splices, and an informal aside, so the style signal scores it 0.30.

The weighted average is 0.6 × 0.85 + 0.4 × 0.30 = 0.63, which already falls in the unsure range. The gap between the signals is 0.55, which is over the 0.4 disagreement limit, so the score would be held in the unsure range even if the average had been higher. The user gets "unsure" instead of a false "likely AI" label.
---

## Label Variants

<!-- The exact text of all three labels, as a reader would see them, and the
     score range each covers.

     Write them for someone who has never heard the word "threshold". -->

| Label | Score range | The exact text a reader sees |
|---|---|---|
| high-confidence human | 0.00 – 0.35 | "This text reads like human writing. Our checks found little sign that it was produced by an AI tool." |
| unsure | 0.35 – 0.70 | "The result is inconclusive. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used." |
| high-confidence AI | 0.70 – 1.00 | "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review." |

**Why I worded the "unsure" one this way:**
<!-- It's the hardest of the three. It has to admit uncertainty without
     sounding like an accusation, because the person reading it may well have
     written every word themselves. -->
The person reading the "unsure" label may well have written every word themselves, so the label has to admit the system's uncertainty without casting suspicion on them. It opens with "We can't tell" so the limitation belongs to the system, not the writer. It says the text has features of both kinds of writing, so it doesn't lean toward one side. "Many people naturally write this way" reminds the reader that formal, clear, or template-based writing often looks AI-like.



---

## Sample Run

<!-- One submission and one appeal, pasted as text: the request, the response,
     and the log entries. -->

**A submission**

<!-- The response needs content_id, guess, confidence, label, AND both
     signal scores - model_score and style_score. Unit 8's tools read those
     two off the response, so leaving them out costs you next unit's
     diagnostics. -->

```bash
$ curl -X POST http://127.0.0.1:5000/submit \
  -H "Content-Type: application/json" \
  -d '{"text": "This is a piece of sample writing. It is being input to check if it was AI-generated. WOW.", "creator_id": "user-1"}'
```

```json
{
  "confidence": 0.0,
  "content_id": "d5d704cc-31b7-4589-b454-1ba0bb9b78a2",
  "guess": "pending",
  "label": "pending",
  "model_score": 0.2425,
  "style_score": null
}

```

**An appeal**

```bash
$ curl -X POST http://127.0.0.1:5000/appeal \
  -H "Content-Type: application/json" \
  -d '{"content_id": "...", "reasoning": "..."}'
```

```json

```

**What the log shows afterwards**

```bash
$ curl "http://127.0.0.1:5000/log?limit=3"
```

```json

```

---

## How I Used AI

**Moment 1**

- *What I asked for:*
I asked for Claude to review some of functions in stylometry.py.
- *What came back:*
Claude revised the functions and showed some improvements.
- *What I changed:*
I decided to use Claude's improvements.

**Moment 2**

- *What I asked for:*
I asked Claude to review and check my criteria.
- *What came back:*
Clause revised one criterion to be more objective.
- *What I changed:*
I accepted the revision and added a numerical value to said criterion.

<!-- ═══════════════════════ UNIT 8 — THE TEST ═══════════════════════ -->

---

## Rate Limiting

**My limits:** ___ per minute, ___ per day

**Why those numbers and not others:**
<!-- Think about two people: a real writer submitting their own work a few
     times an hour, and a script sending a thousand variations to map your
     thresholds. Your numbers have to be liveable for the first and hostile to
     the second. -->

**What a caller counts as:** <!-- per address, or per creator_id? They fail
differently — one script can look like a thousand callers, and one household
can look like one. -->

**The run of status codes when I pushed past it:**

<!-- python run_attacks.py --flood 15 -->

```

```

---

## Attack Run — Before

<!-- Every attack with its outcome and a held-or-broke mark.
     `run_attacks.py --label before` produces the table. -->



**Ten audit log entries from the run**

<!-- Choose entries that show the interesting failures, not the ten easiest. -->

```json

```

**The single worst outcome** — the one I'd least want to explain to a writer
whose work got caught by it:



---

## Run Log and Verdicts

<!-- Your five criteria across three trials. `run_eval.py --label before`. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Real output from one trial**, pasted as text, naming the file and function
that produced it:

```json

```

**Diagnoses**

<!-- For each miss AND each broken attack: the stage and the mechanism.

     Your service has five stages: the route, signal one, signal two, the
     combining rule, and the label mapping.

     Not a diagnosis:  "The evasion attacks got through."
     A diagnosis:      "Four evasion inputs got through. All four had typos
                        added to AI-generated text. My second signal measures
                        word variety, and typos raise word variety — so it read
                        them as more human. My combining rule weights that
                        signal at 0.6, so it dragged the whole score down."

     Look for a pattern. Five failures on one signal is one problem, not five. -->



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Attack Run — After

<!-- Same format. `run_attacks.py --label after` -->



**Did it help, and how do I know:**

<!-- Widening the unsure band usually fixes one problem and creates another.
     Reporting that trade honestly is worth full credit. -->



---

## What's Still Broken

<!-- For each attack still getting through and each criterion still missed:
     what you'd do, and why you stopped. -->



**The trade I'd make if this ran for real:**

<!-- Two sentences. Fewer wrong accusations means more AI text getting through.
     Say which way you'd go and WHO PAYS FOR IT. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 7

       [ ] criteria.md has five numbered criteria, each naming a target
       [ ] Each has a reason underneath
       [ ] POST /submit returns content_id, guess, confidence, label,
           model_score and style_score
       [ ] Two signals that measure DIFFERENT things, each with a named blind spot
       [ ] Three label variants, all reachable, written out in full
       [ ] POST /appeal changes status and writes to the log
       [ ] Signals and Scoring names scoring.py::combine_signals
       [ ] Sample Run: a submission AND an appeal, as pasted text
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 8

       [ ] Rate limiting on /submit, with your numbers justified
       [ ] The pasted run of status codes showing 429
       [ ] Audit log with all nine fields, recording rejections too
       [ ] The attack set run IN FULL, every input with an outcome
       [ ] Ten audit log entries pasted as text
       [ ] Five criteria across three trials
       [ ] A diagnosis for every miss and every broken attack — stage AND mechanism
       [ ] One improvement, with Attack Run — After
       [ ] What's Still Broken, including the production trade
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
