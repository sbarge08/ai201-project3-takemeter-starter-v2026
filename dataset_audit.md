# TakeMeter Dataset Audit

Audit date: 2026-10-18

## Backup and scope

- Created `labels_template.csv.pre-recovery.bak` before this audit.
- The backup is byte-for-byte identical to the current `labels_template.csv`.
- No post text, labels, or notes were changed. No posts were marked as checked.

## Dataset summary

- Posts: **200**
- `technical_help`: **100** (50%)
- `learning_advice`: **100** (50%)
- Header: `text,label,note`

## Post text completeness and source verification

**No original Reddit submission was successfully verified**, so there are no verified-complete posts or successfully verified source URLs to report. No Reddit URLs were present in the CSV. The `r/learnprogramming` Reddit search request returned **403**; a DuckDuckGo search request returned an anti-bot challenge. I stopped at those access restrictions and did not try to bypass them.

Content-only review:

- Posts **1–40** contain substantial title-plus-body-like text. Their completeness is **unverified**; without source comparison, they cannot be certified as complete.
- Posts **41–100** appear to contain shortened excerpts after their titles. Flagged **INCOMPLETE** pending source verification: posts **41–100**.
- Posts **101–200** contain titles only in this dataset. Whether the original submissions had bodies cannot be confirmed; flagged **INCOMPLETE** pending source verification: posts **101–200**.

Thus, **160 posts (41–200) are flagged INCOMPLETE** based on the text available here. Posts 1–40 are not flagged as visibly truncated, but remain source-unverified. The title-only entries are not assumed to have empty original bodies.

## Label review

No labels were changed. Based on the text currently available:

| Post | Current label | Review suggestion |
|---:|---|---|
| 70 — “How to understand regex” | `technical_help` | The excerpt asks about understanding/using a specific technical concept, which fits `technical_help`. Reconsider only if the full post mainly asks for learning resources or study methods. |
| 82 — “How can I learn about Promises, Async, Await...etc in JavaScript?” | `technical_help` | Ambiguous. The excerpt concerns specific JavaScript concepts, so the current label is defensible. Consider `learning_advice` only if the full post is chiefly about learning strategy or resources. |
| 88 — “Best way to learn resolving git conflicts?” | `technical_help` | **Likely correction to review:** `learning_advice` may fit better because the excerpt asks for the best way to learn conflict resolution, rather than presenting a specific conflict or error. Verify against the complete post before deciding. |

These suggestions are provisional because source bodies could not be retrieved. No other definite correction is recommended from the text available; labels on incomplete posts should be reviewed again after their full text is obtained.

## Structural and validation results

Validated with Python's `csv` module using strict parsing:

- Exactly **200** data records and the exact header `text,label,note`.
- Every data record has exactly 3 columns.
- All labels are either `technical_help` or `learning_advice`; no blank text or labels.
- No duplicate post text.
- No invalid CSV quoting, blank records, duplicate header records, or malformed records.
- Posts **1–20** have `note=cold`; posts **21–200** have `note=pre-labelled`.
- Neither label exceeds 60% of the dataset (each is **50%**).

## Training readiness

**Not ready for training as a verified-complete dataset.** Posts 41–200 are flagged INCOMPLETE, no original submissions or source URLs were verified, and posts 1–40 also lack source verification. The CSV itself passes the structural and label-count checks above.
