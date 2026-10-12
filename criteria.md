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

## 1.

<!-- Your criterion. It must name a number. -->

Overall accuracy on the held-out test split is at least 70%.

**Why this target:**

With 4 labels, a coin-flip guesser gets about 25%. Two of my labels
(`ticket_exchange`, `logistics`) have fairly distinctive vocabulary — prices,
"DM me", seat/row numbers, show times — which should make them easy to tell
apart from the other two. 70% sits well above chance and above what I'd
expect from vocabulary alone, so it's a real bar, not a guarantee.

---

## 2.

<!-- Your criterion. -->

`ticket_exchange` has an F1 of at least 0.85 on the held-out set.

**Why this target:**

This is my easiest label by far — posts in it almost always contain a price,
"selling"/"WTB"/"WTS", or "DM me", words that rarely show up in my other three
labels. If the model can't do very well here, something is wrong with my
labelling rather than the task being genuinely hard, so I'm setting this one
high on purpose.

---

## 3.

<!-- Your criterion. -->

No single label makes up more than 65% of my 208 labelled posts.

**Why this target:**

r/NYCmovies skews toward ticket posts, so `ticket_exchange` is my biggest risk
for dominating the set. The brief caps any label at 70% before the model just
learns to guess it; I'm picking 65% as my own, slightly stricter target
because I want the other three labels to have enough examples to actually be
measurable, not just technically present.

---

## 4.

<!-- Your criterion. -->

`analysis` and `commentary` each have an F1 of at least 0.50 on the held-out set.

**Why this target:**

These two are my hardest boundary — both can be about the same kind of post
(a theater's future, an etiquette complaint), and the only difference is
whether a checkable fact is actually load-bearing in the argument. I expect
these two labels to also have fewer examples than `ticket_exchange` or
`logistics`, so I'm setting a lower bar than Criterion 1's overall number,
rather than pretending this boundary is as easy as the others.

---

## 5.

<!-- Your criterion. -->

Among the model's most-confident third of predictions, accuracy is at least
20 points higher than among its least-confident third.

**Why this target:**

If a confident guess isn't right more often than an unsure one, confidence is
just noise and I shouldn't trust it when deciding whether to double-check a
prediction. 20 points is large enough that it can't be explained away by
normal seed-to-seed variance on a 208-post dataset, so a hit here would mean
something real.

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
