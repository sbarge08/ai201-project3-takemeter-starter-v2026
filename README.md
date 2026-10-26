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





## What This Does
Community: r/learnprogramming

This project classifies programming-discussion text collected for a dataset intended to represent r/learnprogramming. It distinguishes `technical_help` requests about a specific programming issue or concept from `learning_advice` requests about learning programming, improving skills, or career direction. The notebook fine-tunes DistilBERT on the 200-row labeled CSV and evaluates it on a held-out test split. In the saved seed-42 run, test accuracy was 83.3% and macro F1 was 0.833.



---

## Label Taxonomy



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


### The hardest boundary

**Which two labels:**
`technical_help` and `learning_advice`

**The decision rule I used every time:**
If the author is trying to solve a specific coding problem, error, or implementation issue, I label it `technical_help`. If the author is asking what to learn, how to improve, or what direction to take in programming, I label it `learning_advice`.


---

## The Dataset



**Where the posts came from:**
The dataset is described as posts from r/learnprogramming. The source audit could not verify any original Reddit submission, and the CSV contains no source URLs.

**How I labelled them:** 20 records were cold-labelled and 180 were AI-prelabelled. The project does not document which, if any, of the AI-prelabelled records I personally reviewed. I do not claim all AI-prelabelled labels or source text were independently verified.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `technical_help` | 100 | 50% |
| `learning_advice` | 100 | 50% |
| **Total** | **200** | **100%** |

**Source-text limitations:** Checking the actual `labels.csv`, 98 of records 1–100 contain the visible ` — ` title/body separator and 2 do not. Records 101–200 have no such separator and appear title-only in the CSV. This text pattern does not verify original post completeness: no original posts or URLs were verified, so the number of genuinely incomplete original posts cannot be reliably determined.

**Three hard cases**
The following are ambiguous in the available text; missing or shortened original text could change their interpretation.

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



**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs; learning rate `2e-5`; batch size 16; maximum length 128 tokens; seed 42. Device: CPU (`torch 2.14.1+cpu`).

**Anything I changed from the defaults, and why:** No training hyperparameters were changed from the notebook settings.

**Split sizes:** 
Train 139 / validation 31 / test 30. The test split contains 15 `technical_help` and 15 `learning_advice` records.

**Saved seed-42 test results:** accuracy **0.833**; macro F1 **0.833**; `technical_help` F1 **0.828** (15 records); `learning_advice` F1 **0.839** (15 records). The notebook's training and results cells completed, and the results cell reported writing `results.json` and `test_split.csv`.



---

## How I Used AI



**Moment 1**

- *What I asked for:* Check whether `labels.csv` existed and, if missing, create it from the template and validate its shape.
- *What came back:* Copilot confirmed it was missing and that the template had 200 data rows with `text`, `label`, and `note` columns.
- *What I changed:* Copilot copied the template to `labels.csv`; the copy was verified byte-identical. No label values were edited.

**Moment 2**

- *What I asked for:* Check the notebook's configured classification labels against my two labels.
- *What came back:* Copilot found the starter `analysis`, `hot_take`, and `reaction` configuration.
- *What I changed:* Copilot replaced only the notebook `LABELS` list with `technical_help` and `learning_advice`; no training code or results were changed.

**Pre-labelling disclosure:**
Twenty records were cold-labelled and 180 were AI-prelabelled. The project does not document how many of the 180 AI-prelabelled records I personally reviewed, so I cannot claim that all their labels or source text were independently verified.

**ChatGPT:** Provided dataset-recovery instructions, helped plan the project, interpreted model-evaluation results, and drafted proposed staff-label adjudications. I personally reviewed and approved the four category-(a) decisions above.

**VS Code Copilot:** Assisted with file editing, validation, running experiments, and writing this report. It also checked that my 30 staff labels matched the source posts without altering my annotations.



---

## Baseline vs. Trained



| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.933 | 0.833 | -0.100 |
| Macro F1 | 0.933 | 0.833 | -0.100 |
| F1 — `learning_advice` | 0.933 | 0.839 | -0.095 |
| F1 — `technical_help` | 0.933 | 0.828 | -0.106 |

**What I predicted before I looked:**
No prediction was recorded before the baseline was run, so I cannot report a before-the-run prediction.

**What the gap actually means:**
On these 30 held-out posts, the zero-shot baseline scored 10 percentage points higher in accuracy and macro F1 than the fine-tuned model. Fine-tuning did not improve these measured scores; this result is limited to this single seed-42 split.



---

## Run Log — Before



| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Accuracy | >=75% | 0.833 | 0.800 | 0.867 | MET |
| 2. Both label F1 | >=0.70 each | tech 0.828; advice 0.839 | tech 0.750; advice 0.833 | tech 0.882; advice 0.846 | MET |
| 3. Dataset balance | Neither >60% | 50% / 50% | 50% / 50% | 50% / 50% | MET |
| 4. Own-taxonomy agreement | >=85% | Not measured | Not measured | Not measured | NOT MEASURABLE |
| 5. Accuracy at confidence >=80% | >=85% | Not available | Not available | Not available | NOT MEASURABLE |

**Model metrics by seed**

| Seed | Test accuracy | Macro F1 | `technical_help` F1 | `learning_advice` F1 |
|---|---:|---:|---:|---:|
| 42 | 0.833 | 0.833 | 0.828 | 0.839 |
| 7 | 0.800 | 0.792 | 0.750 | 0.833 |
| 2024 | 0.867 | 0.864 | 0.882 | 0.846 |

Criterion 4 requires another person to label the same posts under my own taxonomy; there is no such comparison. The staff-agreement exercise uses a different taxonomy and cannot substitute. Criterion 5 requires the accuracy of predictions whose confidence is at least 80%; the saved results report accuracy for the most-confident third, not the exact 80%-confidence subset, so I cannot determine this verdict.

### Confusion matrix



| true \\ predicted | `technical_help` | `learning_advice` |
|---|---:|---:|
| `technical_help` | 12 | 3 |
| `learning_advice` | 2 | 13 |

**My biggest off-diagonal number, and what it means:**
The largest off-diagonal is 3 true `technical_help` posts predicted as `learning_advice`; the reverse direction has 2. The errors are roughly symmetric, and the notebook describes this label boundary as difficult in both directions. This is only the seed-42 split.



---

## Verdicts and Diagnoses



| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | >=75% | MET | 0.800–0.867 across seeds 42, 7, and 2024; all exceed 0.75. |
| 2 | Both label F1 | >=0.70 each | MET | Lowest per-label F1 is 0.750 (`technical_help`, seed 7); all six label/seed scores exceed 0.70. |
| 3 | Dataset balance | Neither label >60% | MET | Full labels.csv has 100/200 (50%) for each label. |
| 4 | Own-taxonomy agreement | >=85% | NOT MEASURABLE | No independent second annotation under my taxonomy is available. |
| 5 | Accuracy at confidence >=80% | >=85% | NOT MEASURABLE | The saved per-seed summaries do not report the exact >=80% confidence subset and its accuracy. |

**Diagnoses**
No measured performance or balance criterion was missed. Criteria 4 and 5 remain unmeasurable for the evidence gaps above; I do not infer them from staff-taxonomy results or the confidence-third summary.



---

## Agreement Report



**Agreement rate:** 26 / 30 = 86.7% (26 agreed, 4 disagreed).
The report compared all 30 posts; there were no unmatched staff posts. The exact disagreements and labels below are taken from `agreement_results.json`.



**Disagreements**
The rule notes below apply the staff taxonomy. I reviewed and approved each adjudication and classified all four as category (a).



**Adjudication categories:**
- **(a) Consistency problem:** The written rule covered the case, but I applied it loosely.
- **(b) Definition gap:** The written rule does not settle the case.
- **(c) Defensible ambiguity:** The rule leaves room for my interpretation, which I can argue from the text.

**1. Sleep and session quality**

> *Exact post:* I tracked sleep against session quality for eleven weeks. Under seven hours the next day's intervals were 6-8 seconds per mile slower, every time bar one. Small sample, one runner, but it's consistent enough that I've stopped scheduling hard days after late shifts.
>
> *My label:* `reaction`; *Staff label:* `analysis`.
>
> *Relevant rule:* Staff rule 3 says recency or personal experience does not override a load-bearing checkable fact. The eleven-week comparison and 6-8 seconds-per-mile difference support the author's scheduling conclusion, matching the staff definition of `analysis`.
>
> *My adjudication:* Staff's analysis is stronger.
> *My category:* (a) — I applied the existing decision rule too loosely.
> *Reason:* The eleven weeks of observations and 6–8-second pace difference support the author's decision about scheduling hard workouts. I had focused on the first-person narrative instead of the evidence supporting the conclusion.

**2. Carbon plates and mileage**

> *Exact post:* Carbon plates aren't the whole story. My times dropped 2% in the shoes and another 2% the following year in the same pair, when the only thing that changed was going from 35 to 50 miles a week.
>
> *My label:* `reaction`; *Staff label:* `analysis`.
>
> *Relevant rule:* Staff rule 1 asks whether the checkable fact carries the argument. The two 2% comparisons and the mileage change support the claim that shoes were not the whole explanation; first-person wording alone does not make this a `reaction` under rule 3.
>
> *My adjudication:* Staff's analysis is stronger.
> *My category:* (a) — I applied the existing decision rule too loosely.
> *Reason:* The two 2% improvements and the mileage change from 35 to 50 miles per week provide evidence that shoes alone do not explain the improvement. This is an argument based on measurements rather than simply an account of an experience.

**3. Temperature and running pace**

> *Exact post:* Ran the same loop in 4C and in 24C within a fortnight, same effort by heart rate. 90 seconds slower in the heat over six miles. People keep saying heat costs you 'a bit' and it's more than that.
>
> *My label:* `reaction`; *Staff label:* `analysis`.
>
> *Relevant rule:* Staff rule 3 says personal experience is still `analysis` when it argues from checkable evidence. The same-loop, same-effort comparison and 90-second difference support the claim about heat.
>
> *My adjudication:* Staff's analysis is stronger.
> *My category:* (a) — I applied the existing decision rule too loosely.
> *Reason:* The author compares the same route at two temperatures, describes controlling effort using heart rate, and uses the 90-second difference to support a broader claim about the effect of heat.

**4. Shoe comparison**

> *Exact post:* Three of us ran the same session on the same day, different shoes, within 10 seconds of each other over 5 miles. I'm not saying shoes do nothing, but whatever they do was smaller than the noise between three people.
>
> *My label:* `reaction`; *Staff label:* `analysis`.
>
> *Relevant rule:* Staff rules 1 and 3 distinguish an argument supported by a checkable fact from a personal reaction. The same-session comparison and 10-second range are the evidence supporting the claim about shoe effects.
>
> *My adjudication:* Staff's analysis is stronger.
> *My category:* (a) — I applied the existing decision rule too loosely.
> *Reason:* The comparison of three runners finishing within 10 seconds is used to support a limited conclusion about shoe effects versus observed variation. The small sample weakens the generalizability but does not turn the post into an immediate emotional reaction.

**What the pattern in my disagreements tells me:** In all four cases, I focused on the first-person experience and overlooked that quantitative evidence supported an argument. I therefore applied the `reaction` label too loosely instead of using the staff `analysis` rule.



---

## The Improvement

**What I changed:** Reduced training epochs from 3 to 2; learning rate, batch size, max length, labels, data, split procedure, and seeds stayed unchanged. The hypothesis was that fewer epochs might improve generalization; overfitting had not been established.

**Which diagnosis pointed at it:** The original three-seed results varied by seed, and the zero-shot baseline outperformed the original fine-tuned model on seed 42. Those observations motivated a test, but they do not prove overfitting.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Accuracy | >=75% | 0.833 | 0.600 | 0.833 | MISSED |
| 2. Both label F1 | >=0.70 each | tech 0.839; advice 0.828 | tech 0.333; advice 0.714 | tech 0.857; advice 0.800 | MISSED |
| 3. Dataset balance | Neither >60% | 50% / 50% | 50% / 50% | 50% / 50% | MET |
| 4. Own-taxonomy agreement | >=85% | Not measured | Not measured | Not measured | NOT MEASURABLE |
| 5. Accuracy at confidence >=80% | >=85% | Not available | Not available | Not available | NOT MEASURABLE |

**After-run metrics**

| Seed | Accuracy | Macro F1 | `technical_help` F1 | `learning_advice` F1 |
|---:|---:|---:|---:|---:|
| 42 | 0.833 | 0.833 | 0.839 | 0.828 |
| 7 | 0.600 | 0.524 | 0.333 | 0.714 |
| 2024 | 0.833 | 0.829 | 0.857 | 0.800 |

**Did it help, and how do I know:** No. Mean accuracy fell from 0.833 to 0.756 and mean macro F1 fell from 0.830 to 0.729. Seed 42's accuracy and macro F1 were unchanged; seeds 7 and 2024 both declined. The baseline also outperformed the original fine-tuned model on seed 42: 0.933 vs. 0.833 accuracy and macro F1.





---

## What's Still Broken

The two-epoch run missed the 75% accuracy target on seed 7 (0.600) and the per-label F1 target because `technical_help` F1 was 0.333 on seed 7. The same change did not improve the three-seed averages; I am not treating this as proof that three epochs overfit.

Criterion 4 remains unmeasurable because it requires a second person to label the same posts under my own taxonomy. The staff set uses a different taxonomy. Criterion 5 remains unmeasurable because prediction-level confidence values and correctness flags were not saved, so the exact >=80% subset cannot be reconstructed.

The dataset audit also flags posts 41–200 as incomplete or title-only and could not verify original source posts. That limits confidence in both the model scores and label quality; I have not changed any dataset labels.



**The gap between what I meant my labels to capture and what the model
learned:**
The seed-42 before matrix shows a near-balanced error in both directions (3 `technical_help` predicted as `learning_advice`, 2 in reverse), while the after matrices vary substantially by seed: seed 7 sends 12 technical-help posts to learning-advice, but seed 2024 sends 5 learning-advice posts to technical-help. The changing direction suggests instability across these small test splits; incomplete source text also limits any stronger conclusion about the boundary.





---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
