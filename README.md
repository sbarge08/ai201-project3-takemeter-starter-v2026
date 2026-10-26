# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does
Community: r/learnprogramming

This project classifies programming-discussion text collected for a dataset intended to represent r/learnprogramming. It distinguishes `technical_help` requests about a specific programming issue or concept from `learning_advice` requests about learning programming, improving skills, or career direction. The notebook fine-tunes DistilBERT on the 200-row labeled CSV and evaluates it on a held-out test split. In the saved seed-42 run, test accuracy was 83.3% and macro F1 was 0.833.



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `technical_help`

**Definition:**
Requests help with a specific programming error, implementation, or technical concept.
**Example 1:**
> “ Can someone nullpointerexception to me in layman's terms?” — the author shows Java code producing a `NullPointerException` and asks why the error occurs.


**Example 2:**
>“Need some help figuring out how to fix the NullPointerException error in my code” — the author identifies the line causing the exception and asks for help understanding the problem.

### `learning_advice`

**Definition:**
Requests guidance about learning programming, improving skills, or pursuing a programming career.

**Example 1:**
> “What Should I Learn Next?” — the author asks whether to continue building projects or move into a specialty such as machine learning.

**Example 2:**
> “New to programming” — the author explains that they are learning HTML/CSS and asks for advice on learning programming and getting a job in the field.

<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
### The hardest boundary

**Which two labels:**
`technical_help` and `learning_advice`

**The decision rule I used every time:**
If the author is trying to solve a specific coding problem, error, or implementation issue, I label it `technical_help`. If the author is asking what to learn, how to improve, or what direction to take in programming, I label it `learning_advice`.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
The dataset is described as posts from r/learnprogramming. The source audit could not verify any original Reddit submission, and the CSV contains no source URLs.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
The CSV notes mark 20 records `cold` (the project convention for unaided labeling) and 180 `pre-labelled`. The audit records no posts as checked. The files do not identify who or what produced the pre-labels, or establish that I personally reviewed them, so I cannot claim that all labels were personally reviewed.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `technical_help` | 100 | 50% |
| `learning_advice` | 100 | 50% |
| **Total** | **200** | **100%** |

**Source-text limitations:** The audit found no verified original post text or URLs. Records 1–40 contain substantial title-plus-body-like text, but completeness is unverified. Records 41–100 appear to be shortened excerpts, and records 101–200 are titles only; all 160 records from 41–200 are flagged incomplete pending source verification. The original bodies for title-only records are unknown.

**Three hard cases**

The dataset audit identified these ambiguous records from the available text. They are not documented as my personal adjudications, and the incomplete source text could change the interpretation.

**1.**
> *Available text (record 70):* “How to understand regex — Hi i want to learn how to use regex because it's so powerful and useful”
>
> *Current label:* `technical_help`
>
> *Plausible alternative:* `learning_advice`, if the full post is mainly asking how to study regex or for learning resources.

**2.**
> *Available text (record 82):* “How can I learn about Promises, Async, Await...etc in JavaScript? — I learned JavaScript last year but I didnt use it often and now I have to work with it.”
>
> *Current label:* `technical_help`
>
> *Plausible alternative:* `learning_advice`, if the complete post asks chiefly for a learning plan or resources rather than an explanation of these concepts.

**3.**
> *Available text (record 88):* “Best way to learn resolving git conflicts? — Hi all. I’ve been using git for quite some time, but never got to level beyond simple push and pull.”
>
> *Current label:* `technical_help`
>
> *Plausible alternative:* `learning_advice`; the available text asks for the best way to learn conflict resolution. The audit recommends reconsidering this label after verifying the complete post.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs; learning rate `2e-5`; batch size 16; maximum length 128 tokens; seed 42. Device: CPU (`torch 2.14.1+cpu`).

**Anything I changed from the defaults, and why:** No training hyperparameters were changed from the notebook settings.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
Train 139 / validation 31 / test 30. The test split contains 15 `technical_help` and 15 `learning_advice` records.

**Saved seed-42 test results:** accuracy **0.833**; macro F1 **0.833**; `technical_help` F1 **0.828** (15 records); `learning_advice` F1 **0.839** (15 records). The notebook's training and results cells completed, and the results cell reported writing `results.json` and `test_split.csv`.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Check whether `labels.csv` existed and, if missing, create it from the template and validate its shape.
- *What came back:* Copilot confirmed it was missing and that the template had 200 data rows with `text`, `label`, and `note` columns.
- *What I changed:* Copilot copied the template to `labels.csv`; the copy was verified byte-identical. No label values were edited.

**Moment 2**

- *What I asked for:* Check the notebook's configured classification labels against my two labels.
- *What came back:* Copilot found the starter `analysis`, `hot_take`, and `reaction` configuration.
- *What I changed:* Copilot replaced only the notebook `LABELS` list with `technical_help` and `learning_advice`; no training code or results were changed.

**Pre-labelling disclosure:**
The CSV marks 180 records `pre-labelled`, but the project files do not say whether AI produced those labels or whether they were individually reviewed. That provenance and review status remain unknown; I do not claim that those labels were personally checked.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
