# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1.Criterion: Overall accuracy must be at least 0.75 (75%).

**Why this target:**
With hot_take making up 57% of the dataset, a baseline model that always guesses hot_take would score 57%. A target of 75% is 18 points above this majority baseline, proving the model has actually learned to distinguish the minority classes (analysis and reaction) rather than just exploiting class imbalance.



---

## 2.The macro-averaged F1 score must be at least 0.55.



**Why this target:**

Macro F1 treats all classes equally, meaning a low score in reaction cannot be hidden by a high score in hot_take. Because natural language has inherent label noise (especially between hot_take and analysis), 0.55 represents a strong, balanced performance across all three classes without demanding unattainable perfection.

---

## 3.The F1 score specifically for the reaction label must be at least 0.4.

**Why this target:**
reaction is the smallest class (~18% of data) and has very few examples in the test set, making its F1 score highly volatile. A 0.4 F1 proves the model successfully recognizes the distinct surface features of reactions, rather than suffering from majority-class bias and defaulting to hot_take for everything.

---

## 4.The model's accuracy on its most confident 1/3 of predictions must be at least 15% higher than its accuracy on the least confident 1/3.


**Why this target:**
If the model is well-calibrated, the predictions it is highly confident about should be right much more often than the ones it is unsure about. A gap of 15% proves the model's internal probability scores actually mean something.


---

## 5.Training Convergence (Loss Reduction)
**Criterion:** The training loss must drop by at least 40% from the first epoch to the final (3rd) epoch.



**Why this target:**
This proves the model is actually learning from the data rather than just randomly initializing and failing to update its weights. A 40% drop over 3 epochs indicates a healthy learning rate.



---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
