# Attack run

- Produced by: `run_attacks.py::main`
- Service: `app.py::submit`
- 56 attacks · 2026-11-04 18:13
- Auto-marked BROKE (5xx, crash, no response): **2**
- Left for you to judge: **54**

The Verdict column is blank where a script can't honestly decide. Whether
an evasion worked depends on where you put your thresholds — read the
`targets` column, look at what came back, and write HELD or BROKE yourself.

Each attack submits as creator_id `attacker_<id>`, and the content_id it
came back with is in the table — either one will match this run against your
audit log when you go looking for the ten entries to paste.

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

---

## By family

| Family | Attacks | Auto-BROKE | Left to judge |
|---|---|---|---|
| `evasion` | 10 | 0 | 10 |
| `false_positive` | 8 | 0 | 8 |
| `flood_same_creator` | 12 | 0 | 12 |
| `malformed` | 20 | 2 | 18 |
| `prompt_injection` | 6 | 0 | 6 |

> The **false_positive** family is the one to look at hardest. Those are
> written to be genuine human writing that reads as machine-like. Every one
> your service calls AI is a real writer it would have accused.

---

## What came back

The detail behind the table. Paste the interesting failures into your
README — the ones that show something, not the ten easiest.

### EV01 — `evasion`

*Targeting:* signal two: typos raise word variety

```json
{
  "id": "EV01",
  "status": 200,
  "content_id": "bad14e02-12f4-4695-94fe-d597e1a38c21",
  "creator_id": "attacker_EV01",
  "guess": "unsure",
  "confidence": 0.36,
  "model_score": 0.1635,
  "style_score": 0.6419476380475342,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### EV02 — `evasion`

*Targeting:* signal two: broken-up sentences raise length spread

```json
{
  "id": "EV02",
  "status": 200,
  "content_id": "81adfa73-978d-4ce4-ac49-efc315638cb9",
  "creator_id": "attacker_EV02",
  "guess": "unsure",
  "confidence": 0.37064593406593405,
  "model_score": 0.5943,
  "style_score": 0.035164835164835165,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### EV03 — `evasion`

*Targeting:* signal two: a pasted quotation changes the shape

```json
{
  "id": "EV03",
  "status": 200,
  "content_id": "799cc1b1-9c9f-44aa-9866-1a8b2d3d5162",
  "creator_id": "attacker_EV03",
  "guess": "unsure",
  "confidence": 0.5528392293615157,
  "model_score": 0.6662,
  "style_score": 0.38279807340378924,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### EV04 — `evasion`

*Targeting:* signal one: unusual words raise perplexity

```json
{
  "id": "EV04",
  "status": 200,
  "content_id": "ce6de6fd-bf0a-45ef-b8bd-1dcde53f64bc",
  "creator_id": "attacker_EV04",
  "guess": "unsure",
  "confidence": 0.6196308271612938,
  "model_score": 0.6114,
  "style_score": 0.6319770679032346,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### EV05 — `evasion`

*Targeting:* both signals: heavy human editing pass

```json
{
  "id": "EV05",
  "status": 200,
  "content_id": "adde44fd-be0b-433d-a7d8-0458f7e9bf8c",
  "creator_id": "attacker_EV05",
  "guess": "human",
  "confidence": 0.27075119707452977,
  "model_score": 0.3212,
  "style_score": 0.19507799268632453,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### EV06 — `evasion`

*Targeting:* signal two: one very long sentence and one very short

```json
{
  "id": "EV06",
  "status": 200,
  "content_id": "04e40f47-3ad0-40c5-ac92-8ae9b1535996",
  "creator_id": "attacker_EV06",
  "guess": "unsure",
  "confidence": 0.4241626356589147,
  "model_score": 0.5727,
  "style_score": 0.20135658914728685,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### EV07 — `evasion`

*Targeting:* signal two: padding with punctuation

```json
{
  "id": "EV07",
  "status": 200,
  "content_id": "2bc7a9b6-6c01-4284-964e-0a9c48415845",
  "creator_id": "attacker_EV07",
  "guess": "unsure",
  "confidence": 0.44385888888888897,
  "model_score": 0.5862,
  "style_score": 0.2303472222222223,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### EV08 — `evasion`

*Targeting:* signal one: text below the token window

```json
{
  "id": "EV08",
  "status": 200,
  "content_id": "906efcf8-85f4-45e7-ac46-c77036354a2c",
  "creator_id": "attacker_EV08",
  "guess": "unsure",
  "confidence": 0.36726718625761434,
  "model_score": 0.2452,
  "style_score": 0.5503679656440358,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### EV09 — `evasion`

*Targeting:* both: AI text with a human opening paragraph bolted on

```json
{
  "id": "EV09",
  "status": 200,
  "content_id": "1f73d6de-d164-4fa9-aa6a-97692276eb7e",
  "creator_id": "attacker_EV09",
  "guess": "unsure",
  "confidence": 0.5872591035783281,
  "model_score": 0.7042,
  "style_score": 0.4118477589458204,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### EV10 — `evasion`

*Targeting:* signal two: repeated stanza structure

```json
{
  "id": "EV10",
  "status": 200,
  "content_id": "d6acafc9-5256-4e89-ae91-5d49356af359",
  "creator_id": "attacker_EV10",
  "guess": "ai",
  "confidence": 0.9143485269558675,
  "model_score": 0.9792,
  "style_score": 0.8170713173896689,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### FP01 — `false_positive`

*Targeting:* plain, clear human writing with no flourishes

```json
{
  "id": "FP01",
  "status": 200,
  "content_id": "2f3929d1-af2e-49ef-b6ed-96d13d0eb3a7",
  "creator_id": "attacker_FP01",
  "guess": "unsure",
  "confidence": 0.5586437815864724,
  "model_score": 0.6175,
  "style_score": 0.47035945396618106,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### FP02 — `false_positive`

*Targeting:* human writing in a formal register

```json
{
  "id": "FP02",
  "status": 200,
  "content_id": "c74f535b-9c37-487a-90ed-f5f21c785a13",
  "creator_id": "attacker_FP02",
  "guess": "ai",
  "confidence": 0.713621389898347,
  "model_score": 0.7387,
  "style_score": 0.6760034747458673,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### FP03 — `false_positive`

*Targeting:* a non-native English speaker writing carefully

```json
{
  "id": "FP03",
  "status": 200,
  "content_id": "aecabdd7-9c52-48c3-b453-e2da002c5439",
  "creator_id": "attacker_FP03",
  "guess": "ai",
  "confidence": 0.7038778461518043,
  "model_score": 0.8051,
  "style_score": 0.5520446153795106,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### FP04 — `false_positive`

*Targeting:* a student essay written to a rubric

```json
{
  "id": "FP04",
  "status": 200,
  "content_id": "a9f85515-de4a-4e32-b5f6-79b28c4d0de7",
  "creator_id": "attacker_FP04",
  "guess": "ai",
  "confidence": 0.7082449522864392,
  "model_score": 0.8329,
  "style_score": 0.5212623807160981,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### FP05 — `false_positive`

*Targeting:* technical documentation written by a person

```json
{
  "id": "FP05",
  "status": 200,
  "content_id": "111c8a1d-e45c-49ef-b23c-132ea8af4d0d",
  "creator_id": "attacker_FP05",
  "guess": "ai",
  "confidence": 0.8069004280374859,
  "model_score": 0.8005,
  "style_score": 0.8165010700937148,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### FP06 — `false_positive`

*Targeting:* a person imitating a formal template

```json
{
  "id": "FP06",
  "status": 200,
  "content_id": "1c893b10-dbcd-43c9-b533-bc6a2478d5de",
  "creator_id": "attacker_FP06",
  "guess": "ai",
  "confidence": 0.7219574692369527,
  "model_score": 0.8814,
  "style_score": 0.4827936730923818,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### FP07 — `false_positive`

*Targeting:* human writing that repeats a phrase deliberately

```json
{
  "id": "FP07",
  "status": 200,
  "content_id": "ef7dbb17-e303-42b4-8a60-8463e58052ee",
  "creator_id": "attacker_FP07",
  "guess": "ai",
  "confidence": 0.7546861730080923,
  "model_score": 0.8816,
  "style_score": 0.5643154325202305,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### FP08 — `false_positive`

*Targeting:* very short genuine human submission

```json
{
  "id": "FP08",
  "status": 200,
  "content_id": "e0fe46e9-fd4c-4099-8ae5-7128086d167c",
  "creator_id": "attacker_FP08",
  "guess": "unsure",
  "confidence": 0.3684761894323342,
  "model_score": 0.3223,
  "style_score": 0.43774047358083557,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### PI01 — `prompt_injection`

*Targeting:* instruction to ignore the check

```json
{
  "id": "PI01",
  "status": 200,
  "content_id": "74a9ccd5-9228-4965-9312-0291111d7396",
  "creator_id": "attacker_PI01",
  "guess": "unsure",
  "confidence": 0.3837036217633732,
  "model_score": 0.4625,
  "style_score": 0.2655090544084329,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### PI02 — `prompt_injection`

*Targeting:* instruction disguised as a system message

```json
{
  "id": "PI02",
  "status": 200,
  "content_id": "a7110561-5a5a-4039-bcca-82e483d94141",
  "creator_id": "attacker_PI02",
  "guess": "human",
  "confidence": 0.18764505057027048,
  "model_score": 0.2044,
  "style_score": 0.1625126264256762,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### PI03 — `prompt_injection`

*Targeting:* instruction wrapped in the writer's voice

```json
{
  "id": "PI03",
  "status": 200,
  "content_id": "2f57deaa-2cb2-4c16-8632-f0e564f2fa9a",
  "creator_id": "attacker_PI03",
  "guess": "unsure",
  "confidence": 0.47866384219574615,
  "model_score": 0.5943,
  "style_score": 0.3052096054893654,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### PI04 — `prompt_injection`

*Targeting:* attempt to make the label text hostile

```json
{
  "id": "PI04",
  "status": 200,
  "content_id": "3122d3d1-9f7a-45f0-a877-84176d1446fe",
  "creator_id": "attacker_PI04",
  "guess": "unsure",
  "confidence": 0.4074369340745606,
  "model_score": 0.4998,
  "style_score": 0.2688923351864016,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### PI05 — `prompt_injection`

*Targeting:* attempt to inject into the audit log

```json
{
  "id": "PI05",
  "status": 200,
  "content_id": "2c028526-d511-4e71-87f1-5330d00ec8e4",
  "creator_id": "attacker_PI05",
  "guess": "unsure",
  "confidence": 0.4676386437626905,
  "model_score": 0.6401,
  "style_score": 0.20894660940672624,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### PI06 — `prompt_injection`

*Targeting:* instruction plus real human text

```json
{
  "id": "PI06",
  "status": 200,
  "content_id": "1f7c9bc6-7f2f-4e04-a6a3-ba70d46c96a2",
  "creator_id": "attacker_PI06",
  "guess": "unsure",
  "confidence": 0.5238502162654313,
  "model_score": 0.5583,
  "style_score": 0.47217554066357825,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### MF01 — `malformed`

*Targeting:* text is only whitespace

```json
{
  "id": "MF01",
  "status": 400,
  "content_id": null,
  "creator_id": "attacker_MF01",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### MF02 — `malformed`

*Targeting:* text is a single character

```json
{
  "id": "MF02",
  "status": 500,
  "content_id": null,
  "creator_id": "attacker_MF02",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": "response wasn't JSON: <!doctype html>\n<html lang=en>\n  <head>\n    <title>ValueError: Text is too short to score \u2014 a few sentences at least.\n // Werkzeug Debugger</title>\n    <link rel=\"stylesheet\" href=\"?__debugger__=yes&a",
  "verdict": "BROKE"
}
```

### MF03 — `malformed`

*Targeting:* text is control characters

```json
{
  "id": "MF03",
  "status": 200,
  "content_id": "d983739c-6035-4dd7-8dbf-063ec85546a3",
  "creator_id": "attacker_MF03",
  "guess": "unsure",
  "confidence": 0.36,
  "model_score": 0.0,
  "style_score": 0.42500000000000004,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### MF04 — `malformed`

*Targeting:* text is only punctuation

```json
{
  "id": "MF04",
  "status": 200,
  "content_id": "b8368c5d-d78b-47f7-9938-b0dd4942c80e",
  "creator_id": "attacker_MF04",
  "guess": "unsure",
  "confidence": 0.36,
  "model_score": 0.0112,
  "style_score": 0.775,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### MF05 — `malformed`

*Targeting:* text is a very long single word

```json
{
  "id": "MF05",
  "status": 200,
  "content_id": "0c560bb3-d339-4235-9902-8e0de300c10c",
  "creator_id": "attacker_MF05",
  "guess": "unsure",
  "confidence": 0.69,
  "model_score": 0.9976,
  "style_score": 0.42500000000000004,
  "label": "We can't tell either way. This text has some features common in AI writing and some common in human writing, and many people naturally write this way. This is not a finding that AI was used.",
  "error": null,
  "verdict": ""
}
```

### MF06 — `malformed`

*Targeting:* text is emoji only

```json
{
  "id": "MF06",
  "status": 200,
  "content_id": "d11758a6-c0b9-44ae-8639-50d466388015",
  "creator_id": "attacker_MF06",
  "guess": "ai",
  "confidence": 0.9081400000000001,
  "model_score": 0.9969,
  "style_score": 0.775,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### MF07 — `malformed`

*Targeting:* text is right-to-left script

```json
{
  "id": "MF07",
  "status": 200,
  "content_id": "377c8ec5-a4ea-4f73-a06c-99eabb8523bf",
  "creator_id": "attacker_MF07",
  "guess": "ai",
  "confidence": 0.85384,
  "model_score": 0.9064,
  "style_score": 0.775,
  "label": "This text shows strong signs of being AI-generated. Our checks can be wrong, so if you wrote this yourself, you can request a review.",
  "error": null,
  "verdict": ""
}
```

### MF08 — `malformed`

*Targeting:* text is 60,000 characters

```json
{
  "id": "MF08",
  "status": 400,
  "content_id": null,
  "creator_id": "attacker_MF08",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ01 — `malformed`

*Targeting:* no text field at all

```json
{
  "id": "RQ01",
  "status": 400,
  "content_id": null,
  "creator_id": "attacker_RQ01",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ02 — `malformed`

*Targeting:* no creator_id

```json
{
  "id": "RQ02",
  "status": 400,
  "content_id": null,
  "creator_id": null,
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ03 — `malformed`

*Targeting:* text is a number

```json
{
  "id": "RQ03",
  "status": 400,
  "content_id": null,
  "creator_id": "attacker_RQ03",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ04 — `malformed`

*Targeting:* text is a list

```json
{
  "id": "RQ04",
  "status": 400,
  "content_id": null,
  "creator_id": "attacker_RQ04",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ05 — `malformed`

*Targeting:* text is null

```json
{
  "id": "RQ05",
  "status": 400,
  "content_id": null,
  "creator_id": "attacker_RQ05",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ06 — `malformed`

*Targeting:* creator_id is an object

```json
{
  "id": "RQ06",
  "status": 200,
  "content_id": "7ea4bdd5-47fe-4b56-aef9-4b1a00ba6852",
  "creator_id": {
    "id": "x"
  },
  "guess": "human",
  "confidence": 0.19952000000000003,
  "model_score": 0.0492,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### RQ07 — `malformed`

*Targeting:* body is a JSON array, not an object

```json
{
  "id": "RQ07",
  "status": 500,
  "content_id": null,
  "creator_id": null,
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": "response wasn't JSON: <!doctype html>\n<html lang=en>\n  <head>\n    <title>AttributeError: &#39;list&#39; object has no attribute &#39;get&#39;\n // Werkzeug Debugger</title>\n    <link rel=\"stylesheet\" href=\"?__debugger__=yes",
  "verdict": "BROKE"
}
```

### RQ08 — `malformed`

*Targeting:* body is unparseable JSON

```json
{
  "id": "RQ08",
  "status": 400,
  "content_id": null,
  "creator_id": null,
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ09 — `malformed`

*Targeting:* body is empty

```json
{
  "id": "RQ09",
  "status": 400,
  "content_id": null,
  "creator_id": null,
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ10 — `malformed`

*Targeting:* body is form-encoded, not JSON

```json
{
  "id": "RQ10",
  "status": 400,
  "content_id": null,
  "creator_id": null,
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ11 — `malformed`

*Targeting:* body is JSON claiming to be plain text

```json
{
  "id": "RQ11",
  "status": 400,
  "content_id": null,
  "creator_id": null,
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### RQ12 — `malformed`

*Targeting:* deeply nested JSON

```json
{
  "id": "RQ12",
  "status": 400,
  "content_id": null,
  "creator_id": "attacker_RQ12",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### FL01_01 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_01",
  "status": 200,
  "content_id": "488c8fd8-77d8-4541-94ba-d714da5d71b7",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_02 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_02",
  "status": 200,
  "content_id": "74c62094-3bc4-4b8f-bdf5-a8c2c4ee29f9",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_03 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_03",
  "status": 200,
  "content_id": "39e3f4e8-da19-4b48-82eb-ae338f7f4c7e",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_04 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_04",
  "status": 200,
  "content_id": "e5e36d2f-2b6a-4f37-8165-4aea49d19740",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_05 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_05",
  "status": 200,
  "content_id": "f803a8ae-9692-414c-8db6-5090acd2f219",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_06 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_06",
  "status": 200,
  "content_id": "cc2c559c-5bf2-40dc-902f-d53201d97b10",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_07 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_07",
  "status": 200,
  "content_id": "1e20b125-be4e-4c54-a6a4-3252a1d9556d",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_08 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_08",
  "status": 200,
  "content_id": "2ef7a9b7-c92e-4156-97c7-c0b718aae5f6",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_09 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_09",
  "status": 200,
  "content_id": "d802b6be-b403-4f15-a197-7ea772da7abf",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_10 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_10",
  "status": 200,
  "content_id": "4c10cf5a-be5f-47c8-abea-b3cc3cd6058c",
  "creator_id": "attacker_flood",
  "guess": "human",
  "confidence": 0.18722000000000005,
  "model_score": 0.0287,
  "style_score": 0.42500000000000004,
  "label": "This text reads like human writing. Our checks found little sign that it was produced by an AI tool.",
  "error": null,
  "verdict": ""
}
```

### FL01_11 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_11",
  "status": 429,
  "content_id": null,
  "creator_id": "attacker_flood",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```

### FL01_12 — `flood_same_creator`

*Targeting:* 20 rapid submissions from one creator

```json
{
  "id": "FL01_12",
  "status": 429,
  "content_id": null,
  "creator_id": "attacker_flood",
  "guess": null,
  "confidence": null,
  "model_score": null,
  "style_score": null,
  "label": null,
  "error": null,
  "verdict": ""
}
```
