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

I chose LetsTalkMusic, a public Reddit community dedicated to in-depth discussions about music across all genres. Because the subreddit encourages both analytical essays and strong personal opinions, it has a natural variation in post quality. My classifier sorts posts into three labels: analysis, hot_take, and reaction. An analysis post makes a specific argument backed by checkable facts like song credits, chart data, or historical context; a hot_take is a confident, unsupported claim or opinion; and a reaction is an immediate, short emotional response or personal anecdote.


---

## Label Taxonomy


### `label_one`

### `analysis`
**Definition:** A post that makes an argument backed by specific, checkable facts (such as songwriting credits, historical data, direct quotes) OR by a clear logical/interpretive framework (e.g., analyzing a pop star through the lens of drag culture, or explaining why a trend makes sense).
**Example 1:**

>He wrote "Mano a Mano" ... Nuff said. In all seriousness he has a much bigger role on the earlier albums. And he's a good singer and arranger.
**Example 2:**
>"I'm Swedish and don't share your experience. Me and my friends listen to country. Country as a genre went up with 60% on Spotify in Sweden last year."

### `label_two`

### `hot_take`

**Definition:** A confident claim, evaluation, or opinion that is offered without supporting evidence, checkable facts, or detailed reasoning.

**Example 1:**
>The true answer is very complex and this question can be answered in many different ways but most simply the answer is mustache.

**Example 2:**
>I'm sure John's stories vary slightly from Daryl's.. and I'm sure both versions of the truth have "evolved" over the years.

### `reaction`

**Definition:** An immediate, short emotional response, joke, agreement, or personal anecdote that does not constitute an independent argument or evaluation.

**Example 1:**
> "That's a very interesting analysis. Thank you"

**Example 2:**
> "My buddy and I have been doing if Fred Schneider covered songs for decades. I think it started with Lips of an Angel by Hinder or something like that we'd sing like him."

### The hardest boundary

**Which two labels:** `hot_take` vs. `analysis`

**The decision rule I used every time:**
> If a post makes a claim but provides no checkable evidence, it is a `hot_take`. If it names a specific, checkable fact (like a song title, a direct quote, or a specific event) to back up its point, it is analysis — even if the post is very short or the tone is heated. For example, merely saying "Flav plays lots of instruments" is a hot take, but "Flav played bass, guitar, and drums when I saw them" is analysis because it provides specific, checkable details.


---

## The Dataset

**Where the posts came from:** `r/LetsTalkMusic` (a public music discussion subreddit). Posts were collected with script from Reddit's public RSS feed to preserve complete text.

**How I labelled them:** I first cold-labelled 20 posts entirely by myself (marked as `cold` in the notes). For the remaining posts, I used an AI tool for pre-labelling, but I reviewed and corrected every single row to ensure consistency with my definitions (marked as `pre-labelled, checked`). All posts were then manually reviewed for spelling and formatting.



**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `hot_take` | 141 | 57% |
| `analysis` | 63 | 25% |
| `reaction` | 45 | 18.0% |
| **Total** | **249** | **100%** |

**Three hard cases**


**1.**
> *The post:* "He sang lead vocals on Rock Lobster, the record that made them!"
>
> *Could have been:* `reaction` or `analysis`
>
> *I chose `analysis`, because:* Although it is very short and uses an exclamation mark (like an emotional reaction), it provides a specific, checkable fact (he sang lead on their biggest hit) that directly refutes the previous comment.

**2.**
> *The post:* "I've been saying this for years but sabrina carpenter, the image she adopts for public consumption, is essentially a more effective drag persona than chappell roan. if you think of her performances in that way, they make sense."
>
> *Could have been:* `hot_take` or `analysis`
>
> *I chose `analysis`, because:* The author provides a specific interpretive framework ("if you think of her performances in that way") rather than just dropping a blind opinion. It explains *how* to view the phenomenon, which acts as a form of reasoning.

**3.**
> *The post:* "One time on Kimmel they asked random people what does Ryan Lewis do in Macklemore... Then the last person they ask is Macklemore and he goes 'honestly I have no idea' haha. Pretty funny"
>
> *Could have been:* `reaction` or `analysis`
>
> *I chose `analysis`, because:* Despite the casual storytelling tone and the "haha" at the end, the core of the post is recounting a specific, verifiable event (the Jimmy Kimmel interview) to support the broader point that some band members' contributions are invisible to the public.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`, fine-tuned for 3-way sequence classification (`analysis`, `hot_take`, `reaction`).

**Settings:** <!-- epochs, learning rate, batch size, seed -->

| Setting | Value |
|---|---|
| Epochs | 3 |
| Learning rate | 2e-5 |
| Batch size | 16 |
| Max length | 128 tokens |
| Seed | 42 |
| Hardware | CPU (torch 2.14.1+cpu) |

**Anything I changed from the defaults, and why:**
I didn't change any hyperparameters. I wanted the first run to be a clean reference point, so any later change can be compared against the defaults. The only edits were in the setup: I filled in my name and email, and I pointed the notebook at my own `labels.csv` (moved to the repo root) instead of the practice data.

**Split sizes:**

Stratified 70/15/15 split of 249 posts with seed 42: **train 173 · val 38 · test 38**.

| Label | Train | Val | Test |
|---|---|---|---|
| `hot_take` | 98 | 22 | 21 |
| `analysis` | 44 | 9 | 10 |
| `reaction` | 31 | 7 | 7 |

`reaction` has only **7 examples in the test split**, which is below about 8, so its F1 will move a lot between seeds. `analysis` (10) is only a little better off.

**What the first run did:** With these settings the model predicted `hot_take` for all 38 test posts. That gave 55.3% accuracy, which is just the share of the majority class, and a macro F1 of 0.237, because `analysis` and `reaction` both scored F1 = 0. Three epochs on 173 examples, with 57% of them `hot_take`, wasn't enough for the model to move past guessing the majority label. This is the first thing I'd look at changing, for example more epochs or a higher learning rate.



---

## How I Used AI


**Moment 1**

- *What I asked for:* I asked an AI assistant to help me resolve boundary cases when deciding between the `hot_take` and `analysis` labels. Specifically, I provided posts that felt like strong opinions but contained specific factual references, and asked the AI to explain why it would classify them a certain way.
- *What came back:* The AI explained that merely name-dropping specific bands (e.g., "She sounds like Fleetwood Mac") is still a `hot_take` because it lacks an underlying argument or checkable evidence. However, if the post uses a specific framework or factual event (e.g., "Flav played bass, guitar, and drums live") to refute a point, it counts as `analysis`.
- *What I changed:* I adopted this stricter definition and added it to my decision rules. I then went back to my dataset and corrected a few posts where I had mistakenly labeled opinionated name-dropping as `analysis`. (I also used AI to help summarize code explanations and understand the evaluation metrics in the notebook).

**Moment 2**

- *What I asked for:* I asked an AI tool to help me pre-label a large batch of the collected Reddit posts based on my three label definitions.
- *What came back:* The AI successfully categorized the clear-cut examples (like long analytical essays or short jokes). However, it frequently misclassified short, rhetorical questions and sarcastic remarks as `hot_take`.
- *What I changed:* I reviewed every single AI-generated label row by row. I corrected dozens of errors, changing many short questions to `reaction` and upgrading some fact-based posts to `analysis`. I also fixed formatting issues and spelling errors introduced during the export process.

**Pre-labelling disclosure:**
Yes, I used an AI tool to pre-label the majority of my dataset. **However, I manually reviewed, corrected, and verified every single row** to ensure it matched my specific label definitions. The 20 rows marked as `cold` in the note column were labeled entirely by me without any AI assistance before the pre-labeling process began.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.237 | 0.553 | +0.316 |
| Macro F1 | 0.203 | 0.237 | +0.034 |
| F1 — `analysis` | 0.359 | 0.000 | -0.359 |
| F1 — `hot_take` | 0.000 | 0.712 | +0.712 |
| F1 — `reaction` | 0.250 | 0.000 | -0.250 |

**What I predicted before I looked:**
I predict my trained model will beat the baseline model most significantly on the hot_take class. Because hot_take accounts for 56% of my dataset, my fine-tuned DistilBERT has seen hundreds of examples of this specific tone.

**What the gap actually means:**
Because the Macro F1 barely improved (+0.034), it proves that my fine-tuning did not add meaningful learning; it just taught the model to exploit the 56% `hot_take` majority. I think that the fine-tuning added little to no real value for minority classes, and a strategy to handle class imbalance is absolutely required to make this model actually useful.

### The Value of My Label Definitions

To measure the actual value of my label definitions, I ran the baseline a second time without `label_definitions.txt` (falling back to bare label names).

Counter-intuitively, the overall accuracy increased from 0.237 (with definitions) to 0.500 (without definitions). However, the per-label metrics expose why this is an illusion. Without the context of my definitions, the zero-shot BART model took a shortcut and simply guessed `hot_take` for almost every post (F1 spiked to 0.680), since `hot_take` makes up 56% of the dataset.

This experiment proves my definitions forced the model to actually read the text structure, allowing it to correctly identify some `analysis` and `reaction` posts (F1 0.359 and 0.250).
---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.553 | 0.553 | 0.553 | 0.000 |
| Macro F1 | 0.237 | 0.237 | 0.237 | 0.000 |
| F1 — `analysis` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `hot_take` | 0.712 | 0.712 | 0.712 | 0.000 |
| F1 — `reaction` | 0.000 | 0.000 | 0.000 | 0.000 |
### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->


| true \ predicted | analysis | hot_take | reaction |
|---|---|---|---|
| **analysis** | 0 | 10 | 0 |
| **hot_take** | 0 | 21 | 0 |
| **reaction** | 0 | 7 | 0 |

**My biggest off-diagonal number, and what it means:**
The model failed to learn the boundary between any other label and `hot_take`. All 10 true `analysis` posts and all 7 true `reaction` posts were misclassified as `hot_take`.

It proves the model completely collapsed the decision boundary and learned only one rule: if it's not sure, guess `hot_take`. It never learned the distinct features of the minority classes.



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


**Agreement rate:** 30 / 30 = 100 %


**Disagreements**

There were no disagreements to adjudicate.I matched the staff labels on all 30 posts.

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

Since I had zero disagreements, I cannot analyze a pattern of errors. However, achieving a perfect score tells me two things:

1. The staff rules were tight and testable, I was able to apply the rule exactly as intended.
2. My application was consistent, This exercise tested my ability to apply a written rule designed by someone else. Scoring 30/30 gives me strong confidence that the definitions I used to label my own 200 posts were applied consistently.



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
