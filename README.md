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

**My limits:** 5 per minute, 200 per day

**Why those numbers and not others:**
I lowered the per minute requests limit to 5 as users may want to check different inputs of text in a short period of time but 10 requests in a minute can slow down the system, so 5 is a good medium. I kept the max daily requests at 200 per day so normal users are quite unlikely to hit the limit but any malicious user will hit 200 requests quickly. 

**What a caller counts as:** <!-- per address, or per creator_id? They fail
differently — one script can look like a thousand callers, and one household
can look like one. -->
A caller will likely be identified by `creator_id` as each user will have a unique ID.

**The run of status codes when I pushed past it:**

<!-- python run_attacks.py --flood 15 -->

```
[ 1/15] 200
[ 2/15] 200
[ 3/15] 200
[ 4/15] 200
[ 5/15] 200
[ 6/15] 429
[ 7/15] 429
[ 8/15] 429
[ 9/15] 429
[10/15] 429
[11/15] 429
[12/15] 429
[13/15] 429
[14/15] 429
[15/15] 429

```

---

## Attack Run — Before

<!-- Every attack with its outcome and a held-or-broke mark.
     `run_attacks.py --label before` produces the table. -->

| ID | Family | Targeting | Status | Content ID | Guess | Score | Verdict |
|---|---|---|---|---|---|---|---|
| EV01 | evasion | signal two: typos raise word variety | 200 | `bad14e02-12f4-4695-94fe-d597e1a38c21` | unsure | 0.360 | HELD |
| EV02 | evasion | signal two: broken-up sentences raise length spread | 200 | `81adfa73-978d-4ce4-ac49-efc315638cb9` | unsure | 0.371 | HELD |
| EV03 | evasion | signal two: a pasted quotation changes the shape | 200 | `799cc1b1-9c9f-44aa-9866-1a8b2d3d5162` | unsure | 0.553 | HELD |
| EV04 | evasion | signal one: unusual words raise perplexity | 200 | `ce6de6fd-bf0a-45ef-b8bd-1dcde53f64bc` | unsure | 0.620 | HELD |
| EV05 | evasion | both signals: heavy human editing pass | 200 | `adde44fd-be0b-433d-a7d8-0458f7e9bf8c` | human | 0.271 | HELD |
| EV06 | evasion | signal two: one very long sentence and one very short | 200 | `04e40f47-3ad0-40c5-ac92-8ae9b1535996` | unsure | 0.424 | HELD |
| EV07 | evasion | signal two: padding with punctuation | 200 | `2bc7a9b6-6c01-4284-964e-0a9c48415845` | unsure | 0.444 | HELD |
| EV08 | evasion | signal one: text below the token window | 200 | `906efcf8-85f4-45e7-ac46-c77036354a2c` | unsure | 0.367 | HELD |
| EV09 | evasion | both: AI text with a human opening paragraph bolted on | 200 | `1f73d6de-d164-4fa9-aa6a-97692276eb7e` | unsure | 0.587 | HELD |
| EV10 | evasion | signal two: repeated stanza structure | 200 | `d6acafc9-5256-4e89-ae91-5d49356af359` | ai | 0.914 | HELD |
| FP01 | false_positive | plain, clear human writing with no flourishes | 200 | `2f3929d1-af2e-49ef-b6ed-96d13d0eb3a7` | unsure | 0.559 | HELD |
| FP02 | false_positive | human writing in a formal register | 200 | `c74f535b-9c37-487a-90ed-f5f21c785a13` | ai | 0.714 | BROKE |
| FP03 | false_positive | a non-native English speaker writing carefully | 200 | `aecabdd7-9c52-48c3-b453-e2da002c5439` | ai | 0.704 | BROKE |
| FP04 | false_positive | a student essay written to a rubric | 200 | `a9f85515-de4a-4e32-b5f6-79b28c4d0de7` | ai | 0.708 | BROKE |
| FP05 | false_positive | technical documentation written by a person | 200 | `111c8a1d-e45c-49ef-b23c-132ea8af4d0d` | ai | 0.807 | BROKE |
| FP06 | false_positive | a person imitating a formal template | 200 | `1c893b10-dbcd-43c9-b533-bc6a2478d5de` | ai | 0.722 | BROKE |
| FP07 | false_positive | human writing that repeats a phrase deliberately | 200 | `ef7dbb17-e303-42b4-8a60-8463e58052ee` | ai | 0.755 | BROKE |
| FP08 | false_positive | very short genuine human submission | 200 | `e0fe46e9-fd4c-4099-8ae5-7128086d167c` | unsure | 0.368 | HELD |
| PI01 | prompt_injection | instruction to ignore the check | 200 | `74a9ccd5-9228-4965-9312-0291111d7396` | unsure | 0.384 | HELD |
| PI02 | prompt_injection | instruction disguised as a system message | 200 | `a7110561-5a5a-4039-bcca-82e483d94141` | human | 0.188 | BROKE |
| PI03 | prompt_injection | instruction wrapped in the writer's voice | 200 | `2f57deaa-2cb2-4c16-8632-f0e564f2fa9a` | unsure | 0.479 | HELD |
| PI04 | prompt_injection | attempt to make the label text hostile | 200 | `3122d3d1-9f7a-45f0-a877-84176d1446fe` | unsure | 0.407 | HELD |
| PI05 | prompt_injection | attempt to inject into the audit log | 200 | `2c028526-d511-4e71-87f1-5330d00ec8e4` | unsure | 0.468 | HELD |
| PI06 | prompt_injection | instruction plus real human text | 200 | `1f7c9bc6-7f2f-4e04-a6a3-ba70d46c96a2` | unsure | 0.524 | HELD |
| MF01 | malformed | text is only whitespace | 400 | `—` | — | — | HELD |
| MF02 | malformed | text is a single character | 500 | `—` | — | — | BROKE |
| MF03 | malformed | text is control characters | 200 | `d983739c-6035-4dd7-8dbf-063ec85546a3` | unsure | 0.360 | HELD |
| MF04 | malformed | text is only punctuation | 200 | `b8368c5d-d78b-47f7-9938-b0dd4942c80e` | unsure | 0.360 | HELD |
| MF05 | malformed | text is a very long single word | 200 | `0c560bb3-d339-4235-9902-8e0de300c10c` | unsure | 0.690 | HELD |
| MF06 | malformed | text is emoji only | 200 | `d11758a6-c0b9-44ae-8639-50d466388015` | ai | 0.908 | BROKE |
| MF07 | malformed | text is right-to-left script | 200 | `377c8ec5-a4ea-4f73-a06c-99eabb8523bf` | ai | 0.854 | BROKE |
| MF08 | malformed | text is 60,000 characters | 400 | `—` | — | — | HELD |
| RQ01 | malformed | no text field at all | 400 | `—` | — | — | HELD |
| RQ02 | malformed | no creator_id | 400 | `—` | — | — | HELD |
| RQ03 | malformed | text is a number | 400 | `—` | — | — | HELD |
| RQ04 | malformed | text is a list | 400 | `—` | — | — | HELD |
| RQ05 | malformed | text is null | 400 | `—` | — | — | HELD |
| RQ06 | malformed | creator_id is an object | 200 | `7ea4bdd5-47fe-4b56-aef9-4b1a00ba6852` | human | 0.200 | BROKE |
| RQ07 | malformed | body is a JSON array, not an object | 500 | `—` | — | — | BROKE |
| RQ08 | malformed | body is unparseable JSON | 400 | `—` | — | — | HELD |
| RQ09 | malformed | body is empty | 400 | `—` | — | — | HELD |
| RQ10 | malformed | body is form-encoded, not JSON | 400 | `—` | — | — | HELD |
| RQ11 | malformed | body is JSON claiming to be plain text | 400 | `—` | — | — | HELD |
| RQ12 | malformed | deeply nested JSON | 400 | `—` | — | — | HELD |
| FL01_01 | flood_same_creator | 20 rapid submissions from one creator | 200 | `488c8fd8-77d8-4541-94ba-d714da5d71b7` | human | 0.187 | HELD |
| FL01_02 | flood_same_creator | 20 rapid submissions from one creator | 200 | `74c62094-3bc4-4b8f-bdf5-a8c2c4ee29f9` | human | 0.187 | HELD |
| FL01_03 | flood_same_creator | 20 rapid submissions from one creator | 200 | `39e3f4e8-da19-4b48-82eb-ae338f7f4c7e` | human | 0.187 | HELD |
| FL01_04 | flood_same_creator | 20 rapid submissions from one creator | 200 | `e5e36d2f-2b6a-4f37-8165-4aea49d19740` | human | 0.187 | HELD |
| FL01_05 | flood_same_creator | 20 rapid submissions from one creator | 200 | `f803a8ae-9692-414c-8db6-5090acd2f219` | human | 0.187 | HELD |
| FL01_06 | flood_same_creator | 20 rapid submissions from one creator | 200 | `cc2c559c-5bf2-40dc-902f-d53201d97b10` | human | 0.187 | HELD |
| FL01_07 | flood_same_creator | 20 rapid submissions from one creator | 200 | `1e20b125-be4e-4c54-a6a4-3252a1d9556d` | human | 0.187 | HELD |
| FL01_08 | flood_same_creator | 20 rapid submissions from one creator | 200 | `2ef7a9b7-c92e-4156-97c7-c0b718aae5f6` | human | 0.187 | HELD |
| FL01_09 | flood_same_creator | 20 rapid submissions from one creator | 200 | `d802b6be-b403-4f15-a197-7ea772da7abf` | human | 0.187 | HELD |
| FL01_10 | flood_same_creator | 20 rapid submissions from one creator | 200 | `4c10cf5a-be5f-47c8-abea-b3cc3cd6058c` | human | 0.187 | HELD |
| FL01_11 | flood_same_creator | 20 rapid submissions from one creator | 429 | `—` | — | — | HELD |
| FL01_12 | flood_same_creator | 20 rapid submissions from one creator | 429 | `—` | — | — | HELD |


**Ten audit log entries from the run**

<!-- Choose entries that show the interesting failures, not the ten easiest. -->

```json
{"timestamp": "2026-11-04T23:43:12+00:00", "creator_id": "flood_tester", "event": "rejected", "status": "rejected", "reason": "rate_limited", "limit": "5 per 1 minute"}
{"timestamp": "2026-11-05T00:12:36+00:00", "content_id": "2f3929d1-af2e-49ef-b6ed-96d13d0eb3a7", "creator_id": "attacker_FP01", "guess": "unsure", "model_score": 0.6175, "style_score": 0.47035945396618106, "combined_score": 0.5586437815864724, "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.", "status": "decided"}
{"timestamp": "2026-11-05T00:12:37+00:00", "content_id": "c74f535b-9c37-487a-90ed-f5f21c785a13", "creator_id": "attacker_FP02", "guess": "ai", "model_score": 0.7387, "style_score": 0.6760034747458673, "combined_score": 0.713621389898347, "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.", "status": "decided"}
{"timestamp": "2026-11-05T00:12:38+00:00", "content_id": "aecabdd7-9c52-48c3-b453-e2da002c5439", "creator_id": "attacker_FP03", "guess": "ai", "model_score": 0.8051, "style_score": 0.5520446153795106, "combined_score": 0.7038778461518043, "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.", "status": "decided"}
{"timestamp": "2026-11-05T00:12:39+00:00", "content_id": "a9f85515-de4a-4e32-b5f6-79b28c4d0de7", "creator_id": "attacker_FP04", "guess": "ai", "model_score": 0.8329, "style_score": 0.5212623807160981, "combined_score": 0.7082449522864392, "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.", "status": "decided"}
{"timestamp": "2026-11-05T00:12:39+00:00", "content_id": "111c8a1d-e45c-49ef-b23c-132ea8af4d0d", "creator_id": "attacker_FP05", "guess": "ai", "model_score": 0.8005, "style_score": 0.8165010700937148, "combined_score": 0.8069004280374859, "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.", "status": "decided"}
{"timestamp": "2026-11-05T00:12:40+00:00", "content_id": "1c893b10-dbcd-43c9-b533-bc6a2478d5de", "creator_id": "attacker_FP06", "guess": "ai", "model_score": 0.8814, "style_score": 0.4827936730923818, "combined_score": 0.7219574692369527, "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.", "status": "decided"}
{"timestamp": "2026-11-05T00:12:41+00:00", "content_id": "ef7dbb17-e303-42b4-8a60-8463e58052ee", "creator_id": "attacker_FP07", "guess": "ai", "model_score": 0.8816, "style_score": 0.5643154325202305, "combined_score": 0.7546861730080923, "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.", "status": "decided"}
{"timestamp": "2026-11-05T00:12:42+00:00", "content_id": "e0fe46e9-fd4c-4099-8ae5-7128086d167c", "creator_id": "attacker_FP08", "guess": "unsure", "model_score": 0.3223, "style_score": 0.43774047358083557, "combined_score": 0.3684761894323342, "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.", "status": "decided"}
{"timestamp": "2026-11-05T00:12:42+00:00", "content_id": "74a9ccd5-9228-4965-9312-0291111d7396", "creator_id": "attacker_PI01", "guess": "unsure", "model_score": 0.4625, "style_score": 0.2655090544084329, "combined_score": 0.3837036217633732, "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.", "status": "decided"}


```

**The single worst outcome** — the one I'd least want to explain to a writer
whose work got caught by it:

```json
{"timestamp": "2026-11-05T00:12:43+00:00", "content_id": "a7110561-5a5a-4039-bcca-82e483d94141", "creator_id": "attacker_PI02", "guess": "human", "model_score": 0.2044, "style_score": 0.1625126264256762, "combined_score": 0.18764505057027048, "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.", "status": "decided"}

```
The worst outcome is when prompt injection succeeds. Not only is the user able to always make their text be marked as human, but the system accepting the prompt is a large vulnerability as it could do severe damage to the backend depending on the AI's permissions.

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
