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

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->
     The community I chose is the Reddit NYC Movies forum: https://www.reddit.com/r/NYCmovies/
     My classifier sorts posts into 4 categories: `ticket_exchange` (buying,
     selling, trading, or giving away a ticket), `logistics` (practical
     questions about seeing a show — seats, showtimes, worth the trip — with
     no argument or transaction attached), `analysis` (a reasoned argument
     backed by a specific, checkable fact), and `commentary` (a personal
     reaction or opinion about the movie-going experience, supported or not).



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `ticket_exchange`

**Definition:**
Buying, selling, trading, or giving away a ticket.

**Example 1:**
>“Ticket for Musk at AMC Lincoln Square this afternoon Sorry for the last minute post, but I can’t leave work to catch this and want to offer the ticket for anyone that can use it on super short notice. Free, just DM me and I can screen shot the pass.”

**Example 2:**
>“selling 1x ticket to sakamoto movies TODAY 9/26 metrograph hey reddit,i am selling 1x ticket to each of these two nearly sold out sakamoto movies today at metrograph Ryuichi Sakamoto: Diaries Saturday, 26 September 2026 05:45 PM Seat(s): Row H (exact will be shared when I transfer the ticket)Tokyo Melody Saturday, 26 September 2026 04:10 PM Seat(s): Row I (exact will be shared when I transfer the ticket) the price of each ticket is $11 or best offer! pay by zelle/venmo/cashapp/paypal”

### `logistics`

**Definition:**
A practical question about seeing a show — seats, showtimes, whether it's worth going — with no argument or transaction attached.

**Example 1:**
>“THE ODYSSEY in 70mm IMAX worth it for 11:00 p.m. showing? I live in Long Island, but frequently visit the city anyway on weekends. I have a ticket for tonight’s showing at 11:00 p.m. Is it worth the trip if it gets me home by about 5:00 a.m.?”

**Example 2:**
>“Which seats are preferred for the 35mm showing? It says screen 3 but I can’t tell which row I should do for optimal viewing. I do plan on getting an aisle seat though.”

### `analysis`

**Definition:**
A reasoned argument backed by a specific, checkable fact that is doing real work in the claim.

**Example 1:**
>“The theater was already in desperate need of renovations when it closed, and would probably need a full overhaul now. And the reason pulled out is because the it didn’t make financial sense to invest in renovations without a more favorable lease, but the landlord wasn’t interested.”

**Example 2:**
>“At MoMA if there’s a series I’m not interested in, that means a month or so without seeing anything I want. On the other hand, if it’s a good series I’m seeing 10 free movies that month. MoMI has more variety in their programming but they are really bad about scheduling and post their calendar with very little notice.”

### `commentary`

**Definition:**
A personal reaction or opinion about the movie-going experience, supported or not.

**Example 1:**
>“Mouse in Theater  I was at Regal Essex Crossing last night and saw this little guy. It got a lot of love on Twitter so I thought I’d share here.Note: this was at least the third time I’ve seen a mouse in The theater. The other times were at Regal Essex Crossing and AMC Lincoln Square.Edit: the movie was Other Mommy There was another mouse too but I couldn’t get a shot of it. I refer to it as other mousie. Also someone asked me to name it so I call him Greyson.”

**Example 2:**
>“I wouldn't buy popcorn there . That part ain't cute.”

### The hardest boundary

**Which two labels:**
`analysis` and `commentary`

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
If the post offers a specific, checkable fact that the claim actually depends
on — a price, a date, a named policy reason — it's `analysis`, even if the
tone is heated. If it's just how the writer felt about an experience, however
detailed, it's `commentary`.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
 https://www.reddit.com/r/NYCmovies/

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
22 posts cold — read and labelled myself with no AI assistance, before anything
else was labelled. The remaining 186 were pre-labelled by an AI assistant
against the definitions above, then I read every one and corrected the calls I
disagreed with (several of the hardest ones are below).

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `commentary` | 69 | 33.2% |
| `logistics` | 62 | 29.8% |
| `ticket_exchange` | 44 | 21.2% |
| `analysis` | 33 | 15.9% |
| **Total** | 208 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* “Are there empty seats in the 70mm IMAX Odyssey showings bc of
> the scalpers? ... if anyone in here has tickets on that date that wanna sell
> them (not for $2k) please lmk”
>
> *Could have been:* `logistics` (it opens with a genuine question about
> whether scalped seats go empty) or `ticket_exchange`.
>
> *I chose, because:* the post ends with an explicit request to buy a ticket
> from whoever's reading. The question is context for why they're asking, not
> the point of the post — the actionable ask is the transaction.

**2.**
> *The post:* “i will say, i use my MOMI membership the least out of all my
> film memberships, their programming is good but they schedule things
> weirdly and also are really bad about emailing about member only events...”
>
> *Could have been:* `analysis` (it does cite specific complaints — bad
> scheduling, bad emailing) or `commentary`.
>
> *I chose, because:* the specific complaints aren't load-bearing for an
> argument — there's no conclusion being argued for beyond "I probably won't
> renew." It reads as a personal account of frustration, not a case being made.

**3.**
> *The post:* “Club Kid tickets are live on the AMC app randomly checked the
> app and saw tickets are up! get em while they're hot”
>
> *Could have been:* `ticket_exchange` (it's entirely about tickets) or
> `logistics`.
>
> *I chose, because:* nobody is buying, selling, trading, or giving away a
> ticket in the post itself — it's an alert that tickets exist. That's
> practical information about seeing a show, which is what `logistics` is for,
> even though the subject matter overlaps heavily with `ticket_exchange`.

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**
`distilbert-base-uncased`, the notebook's default — no reason to move off it
for a four-label, 208-post dataset this size.

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, seed 42 (the notebook's defaults).

**Anything I changed from the defaults, and why:**
Nothing yet — running with the defaults first so I have a baseline before
tuning anything.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
train 144 · val 32 · test 32 (70/15/15 of 208 posts, seed 42).

| Label | train | val | test |
|---|---|---|---|
| `ticket_exchange` | 30 | 7 | 7 |
| `logistics` | 43 | 10 | 9 |
| `analysis` | 23 | 5 | 5 |
| `commentary` | 48 | 10 | 11 |

`ticket_exchange` (7) and `analysis` (5) both have fewer than 8 examples in
the test split — their F1 will be noisy between seeds, and this run already
shows it: both scored 0.000 F1 on this seed.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* help deciding how many labels my taxonomy should have
  after I described my community (a ticket buy/sell/trade forum crossed with
  general movie discussion).
- *What came back:* a recommendation to use 4 labels instead of 2, with
  `ticket_exchange` split out from general discussion, since collapsing them
  would risk one label dominating over 70% of the data.
- *What I changed:* adopted the 4-label structure (`ticket_exchange`,
  `logistics`, `analysis`, `commentary`) instead of my original 2-label idea.

**Moment 2**

- *What I asked for:* to pre-label the ~186 posts in `labels.csv` I hadn't
  cold-labelled yet, using my taxonomy's definitions.
- *What came back:* every post labelled with one of my 4 labels, plus a note
  on which ones were genuinely ambiguous (ties to scalping, ticket-availability
  announcements, and a couple of analysis/commentary boundary calls).
- *What I changed:* read every pre-labelled post and corrected the ones I
  disagreed with before finalising `labels.csv`.

**Pre-labelling disclosure:**
186 of my 208 posts were pre-labelled by AI against my written definitions,
then read and corrected by me one at a time. 22 were labelled cold, with no AI
involvement, before the pre-labelling pass began.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->
<!--run python baseline.py --trained results.json with labels_definition.txt-->
| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.594 | 0.500 | -0.094 |
| Macro F1 | 0.556 | 0.304 | -0.252 |
| F1 — `analysis` | 0.545 | 0.000 | -0.545 |
| F1 — `commentary` | 0.645 | 0.629 | -0.017 |
| F1 — `logistics` | 0.200 | 0.588 | +0.388 |
| F1 — `ticket_exchange` | 0.833 | 0.000 | -0.833 |

<!--run python baseline.py --trained results.json --out baseline_results_nodefs.json without labels_definition.txt-->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.500 | 0.500 | +0.000 |
| Macro F1 | 0.355 | 0.304 | -0.051 |
| F1 — `analysis` | 0.000 | 0.000 | +0.000 |
| F1 — `commentary` | 0.667 | 0.629 | -0.038 |
| F1 — `logistics` | 0.182 | 0.588 | +0.406 |
| F1 — `ticket_exchange` | 0.571 | 0.000 | -0.571 |


**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
     I predict that my trained model has higher accuracy when classifying the posts.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->


| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread | Verdict |
|---|---|---|---|---|---|
| Overall accuracy | 0.500 | 0.344 | 0.438 | 0.156 | Moved 0.156 across seeds — more than 10 points. The dataset is too small or too lopsided for a stable measure. |
| Macro F1 | 0.304 | 0.190 | 0.261 | 0.114 | |
| F1 — `ticket_exchange` | 0.000 | 0.000 | 0.000 | 0.000 | Not a spread problem — zero on every seed. The model never predicts this label at all, which is collapse, not noise. |
| F1 — `logistics` | 0.588 | 0.474 | 0.471 | 0.118 | |
| F1 — `analysis` | 0.000 | 0.000 | 0.000 | 0.000 | Same pattern as `ticket_exchange` — consistently zero, not swinging. |
| F1 — `commentary` | 0.629 | 0.286 | 0.571 | 0.343 | Largest swing in the table, over double the accuracy spread — the least trustworthy number here. |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | ticket_exchange | logistics | analysis | commentary |
|---|---|---|---|---|
| **ticket_exchange** | 0 | 2 | 0 | 5 |
| **logistics** | 0 | 5 | 0 | 4 |
| **analysis** | 0 | 1 | 0 | 4 |
| **commentary** | 0 | 0 | 0 | 11 |

Biggest confusion: 5 real `ticket_exchange` posts called `commentary`.
The other direction: 0 real `commentary` posts called `ticket_exchange`.

That's lopsided — the model leans toward `commentary` on this pair.
A direction like that usually points at your labelling rather than the
model. Your agreement report is the thing that can tell you which.

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

Compared 30 posts.

  agreed      30
  disagreed   0
  agreement   100%

I matched staff on every post. The set was easy or my rule is genuinely tight.

**Agreement rate:** 30 / 30 = 100%

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

None — I matched staff on all 30, including the 7 staff flagged as `HARD`
(`sp_005`, `sp_010`, `sp_014`, `sp_019`, `sp_020`, `sp_028`, `sp_029`). Three of
those worth showing the reasoning on, since they're where disagreement would
most plausibly have happened:

**1.**
> *The post:* sp_014 — "Times are down about 3% since the shoe rules changed,
> so obviously the shoes are the whole story and the sport has a problem."
>
> *Staff said / I said:* `hot_take` / `hot_take`.
>
> *My call, and why:* The 3% is decoration on a conclusion already reached —
> "so obviously" is doing the argumentative work, not the number. Rule 1 asks
> whether the fact carries the claim; here it doesn't.
>
> *Which pile:* n/a — agreement, not a disagreement. Flagged because it's the
> canonical rule-1 case: the clearest test of whether I'd apply the rule or
> get pulled in by the presence of a number.

**2.**
> *The post:* sp_010 — "Three of us ran the same session... within 10 seconds
> of each other over 5 miles. I'm not saying shoes do nothing, but whatever
> they do was smaller than the noise between three people."
>
> *Staff said / I said:* `analysis` / `analysis`.
>
> *My call, and why:* Hedged conclusion, tiny sample — but the observation is
> checkable and the entire claim rests on it. The hedge doesn't disqualify it
> from `analysis`; the fact is still load-bearing.
>
> *Which pile:* n/a — agreement.

**3.**
> *The post:* sp_029 — "Just realised the shoes I set every PB in this year
> are the ones I bought by accident in the wrong size."
>
> *Staff said / I said:* `reaction` / `reaction`.
>
> *My call, and why:* Reads like it could be a wry `hot_take` about shoe
> discourse, but it's the writer reporting their own realisation, right now —
> rule 2 (a claim about the world vs. a report about the writer) puts it in
> `reaction`.
>
> *Which pile:* n/a — agreement.

**What the pattern in my disagreements tells me:**
There's no disagreement to diagnose, but the near-misses are informative on
their own: every post staff marked `HARD` is exactly where I had to slow down
and apply rules 1-3 explicitly rather than pattern-match on surface features
like numbers or heated tone. That's encouraging for my own taxonomy's hardest
boundary (`analysis` vs. `commentary`), which turns on the same test — whether
a fact is load-bearing, not whether one is merely present. What this 100%
doesn't tell me is whether I'd stay that careful across 200 posts of my own
without an answer key to check against, which is the more realistic failure
mode than misreading the rules themselves.

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
