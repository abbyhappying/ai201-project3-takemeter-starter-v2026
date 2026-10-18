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
**Definition:** A post that makes an argument or observation backed by specific, checkable facts (such as songwriting credits, historical chart data, specific lyrics, or direct quotes).
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

**Where the posts came from:** `r/LetsTalkMusic` (a public music discussion subreddit). Posts were copied manually to preserve complete text.

**How I labelled them:** I first cold-labelled 20 posts entirely by myself (marked as `cold` in the notes). For the remaining posts, I used an AI tool for pre-labelling, but I reviewed and corrected every single row to ensure consistency with my definitions (marked as `pre-labelled, checked`). All posts were then manually reviewed for spelling and formatting.



**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `hot_take` | 142 | 57.2% |
| `analysis` | 62 | 24.8% |
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

**Base model:**

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

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
