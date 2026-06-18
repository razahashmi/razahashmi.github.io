# Faithful but not general: does a transformer extrapolate the algorithm it learns?

*A controlled study that started as a question about machine "creativity" and ended as a clean result about length generalization — one strong negative finding, one positive flicker, and a methodological trap worth naming.*

## TL;DR

I trained small decoder-only transformers from scratch to compute a running sum mod 23 using an explicit chain-of-thought, on sequences of length 2–8, and tested whether they could apply the *same* procedure to longer sequences.

- Every model learned the task **perfectly in-distribution** (100% accuracy, zero variance across 5 seeds), and the chain-of-thought is **fully faithful** — it really computes the running sum step by step.
- **None of them generalize the algorithm.** The best case buys *one to two positions* past the training boundary and then falls off a cliff to chance. "Faithfully executes the algorithm" and "learned the algorithm as an abstraction" turn out to be completely different things.
- **The positional encoding is the entire story** for that thin margin: NoPE ≈ 0.78 at L10, Abacus-style ≈ 0.51 (but wildly unstable), ALiBi ≈ 0.16, RoPE at chance. This reproduces Kazemnejad et al.'s finding that NoPE generalizes best.
- **Model size doesn't matter** (4× scaling = identical chance-level extrapolation) and **weight-tied recurrence doesn't matter** (identical to untied).
- A **self-discovery** setup (the model invents its own chain-of-thought, verified and reinforced) stalls at the same point regardless of positional encoding, for an interesting reason: the verifier gets starved.
- A **shuffled-label control** confirms the in-distribution learning is genuine, not leakage.

None of the individual ingredients is novel — this is a clean reproduction plus a tightly controlled negative result. The value, if any, is in the instrument: a task with *provable* structure that lets you separate "executes" from "abstracts" cleanly, and watch exactly where and how generalization fails.

---

## Where this came from: the cheapest rule always wins

This started from a romantic question: can a language model *create* something — discover a rule, a structure, a "law" — that wasn't in its training data? The Gödelian fantasy is that you withhold a truth from the system and the model reaches outside the system to find it.

I spent a while learning, the hard way, that the question is mostly ill-posed, and that the honest version of it keeps colliding with one fact: **a model fits the cheapest hypothesis consistent with its data under its inductive bias.** I watched this happen at every level:

- Train a model on a corpus that's *provably consistent with two rules* — a deep global one and a cheap local one that agree on the training data and disagree out of distribution — and it takes the cheap one, every seed, every regularization setting. Since the data is symmetric between the two rules, that choice is 100% inductive bias.
- Give it a chain-of-thought scratchpad to "show its work," and if the task leaves any room for a shortcut, the shortcut just hides *inside* the scratchpad.
- Force the shortcut out, and the model learns the real rule — but only as a pattern locked to the exact shape of the training data.

This post is the rigorous version of that last step. If a model learns a genuine algorithm (running sum) and that algorithm is length-agnostic by nature, does the model's *implementation* generalize to lengths it never saw? That's the cleanest possible "did it abstract the rule, or just fit the data" question, and length is the cleanest axis on which to ask it.

## The task

The model sees a sequence of integers and must emit a chain-of-thought of running prefix sums, mod 23:

```
input:      x0 x1 ... x_{L-1}  SEP   c0 c1 ... c_{L-1}
where       c_i = (x0 + ... + x_i) mod 23,   c_{L-1} = the answer
```

Each step `c_i = (c_{i-1} + x_i) mod 23` is a *local* operation; chained, they compute a *global* aggregation. I train on lengths 2–8 (values drawn uniformly, so there's no exploitable shortcut), with cross-entropy on the scratchpad tokens, and I evaluate by free-running greedy generation — the model generates its own chain, and I check whether the final token equals the true sum.

**Metric.** Sum-accuracy at length L = the fraction of fresh length-L inputs for which the greedily-generated final scratchpad token equals `(Σ x) mod 23`. In-range = mean over training lengths; extrapolation = the same metric at lengths beyond 8. Chance ≈ 1/23 ≈ 0.043.

**Rigor.** AdamW with β2 = 0.98, LR warmup + cosine decay, gradient clipping (the loss curves are smooth — no spikes, no best-checkpoint cherry-picking); 5 seeds per condition with 95% confidence intervals; correct, unit-tested implementations of RoPE (its attention scores depend only on relative position), ALiBi (geometric head slopes), NoPE, and an Abacus-style offset-randomized positional table. (The literal Abacus method is digit-aligned and arithmetic-specific; I transfer only the offset-randomization idea and label it as an adaptation.)

## Result 1: it learns the algorithm perfectly — and faithfully

Every condition reaches 100% in-distribution accuracy with zero variance across seeds, and trace faithfulness (does every intermediate `c_i` equal the true prefix sum?) is also perfect. This matters: the model is not getting the answer by some lookup or heuristic. It genuinely runs the recurrence. So whatever fails next is *not* a failure to learn the rule.

## Result 2: the positional encoding is the whole story — and it only buys a sliver

Here is the same model, same training data, varying only the positional encoding, measured at the first out-of-distribution length (L10, two past the max training length of 8):

![PE comparison at L10](files/fig1_pe_bars.png)

NoPE generalizes best (0.78 ± 0.02), then the Abacus-style encoding (0.51, but with a ±0.19 CI — moderately good and wildly unreliable run to run), then ALiBi (0.16, weak but clearly above chance), and RoPE sits at chance. **This reproduces Kazemnejad et al. (2023):** removing positional encodings entirely is the best choice for length generalization. The summation recurrence is position-agnostic, and absolute or rotary positions apparently anchor the computation to specific offsets in a way that doesn't transfer.

But look what happens as you push further out:

![The extrapolation cliff](files/fig2_cliff.png)

Every encoding that extrapolates at all does so *only at L10*, and by L12 everything is back at chance — including NoPE. So the honest framing is not "NoPE solves length generalization." It's: **NoPE widens a boundary margin from zero to about two positions; no encoding produces the abstracted, length-general algorithm.** The cliff is the result. The positional encoding is a knob on its width, not a fix.

(The fine-grained sweep at L9–L13, run on RoPE, confirms RoPE is at chance even one position out — zero margin. NoPE's exact break is somewhere between L10 and L12.)

## Result 3: even the positive case is fragile

The NoPE number deserves an asterisk. Watching a single run over training, in-range accuracy pins to 1.0 early and stays there — but L10 extrapolation **oscillates between chance and ~0.8 across checkpoints**, never settling:

![NoPE instability over training](files/fig3_nope_instability.png)

This is important for two reasons. First, it means the "capability" isn't a stable thing the model acquires and keeps; it flickers. Second, it's a reporting hazard: a single final-checkpoint number (or worse, a best-checkpoint number) can make a fragile, intermittent behavior look like a solid one. The tight ±0.02 seed CI captures variance *at a fixed step* and understates the real instability. If you only take one lesson from this post on the methods side, take this one.

## Result 4: scale and recurrence don't help

Two interventions you might reach for, both null:

- **Capacity**: a 4× larger model (and a 4× smaller one) extrapolate identically to the base — at chance. This isn't underfitting; the bigger model learns in-range just as perfectly and generalizes just as poorly.
- **Weight-tied recurrence** (a Universal-Transformer-style shared block applied repeatedly, the natural "apply the operation N times" inductive bias): identical to the untied model, at chance. Tying the weights gives the right *shape* but not the right *positional* behavior, and the failure here is positional.

## Result 5: self-discovery stalls, and the reason is instructive

The experiments above hand the model the procedure via supervised chain-of-thought. The harder, more interesting question is whether it can *discover* the procedure without being shown it. I set up expert iteration (STaR-style) with a length curriculum: at each length the model samples its own scratchpad traces, a verifier keeps only the **fully correct** ones (every step right, not just the final answer), and the model is fine-tuned on its own verified traces, with a replay buffer to prevent forgetting earlier lengths.

![Expert iteration collapse](files/fig4_expert_iteration.png)

It bootstraps length 1 trivially (the answer just is the input), and then **collapses at the L2→L3 step** — and it collapses at the *same place* whether you use NoPE or RoPE. The mechanism is clean: to get a training signal at the next length, the model has to *sample* a fully-correct trace one step beyond what it has mastered. Once it can't (by L3, fewer than 1% of sampled traces are fully faithful), the verifier has nothing to keep, and the loop starves. The positional encoding's two-position extrapolation margin — which only appears after training on a *range* of lengths — never gets a chance to help, because self-discovery can't climb far enough to build that range. The replay buffer did work as intended (length 1 stays at 100% through the whole curriculum instead of being forgotten), so the failure is specifically the bootstrap, not catastrophic forgetting.

## Control: is the in-distribution learning even real?

A skeptic should ask whether the in-range success is genuine rule-learning or some leakage. The control: shuffle the labels — pair each training input with a *different* input's scratchpad, on a fixed training set — and train to convergence. The model memorizes the (meaningless) training set (train loss → 0), but held-out accuracy stays glued to chance (0.037–0.051 around the 0.043 baseline) the entire time. There's no learnable function, so there's nothing to generalize — exactly as it should be. The real task's in-range generalization is therefore genuine, not an artifact.

## What this means for the original question

The romantic version — "can the model create a rule that isn't in its data?" — dissolves on contact with the no-free-lunch theorem: if a regularity is genuinely underdetermined by the data, no learner can recover it from the data alone; whatever it produces is fixed by its inductive bias. There's no creation from nothing, only fitting; novelty always lives in the gap between the corpus and the bias.

This study makes that concrete and measurable. The model learns the cheapest implementation consistent with its training data — and the cheapest implementation of "running sum on lengths 2–8" turns out to be something length-specific, not the general recurrence. You can *move* the gap by engineering the inductive bias (that's all the positional encoding is doing — and removing positions entirely is the bias most aligned with a position-agnostic rule), but on this task even the best-aligned bias only stretches generalization by a sliver. And the one paradigm that has actually produced verifiable novelty in the literature — search plus a ground-truth verifier plus a learning loop, as in AlphaTensor or FunSearch — is exactly the thing my self-discovery experiment is a stripped-down version of, and it failed for a concrete reason (the verifier starves when the model can't sample correct solutions). Temperature and sampling are not a creativity engine here; they're a lottery on inputs the model can't already do.

## Related work and honest positioning

This sits squarely inside a well-studied area, and nothing here is a novel mechanism:

- **Kazemnejad et al. (2023), *The Impact of Positional Encoding on Length Generalization*** — finds NoPE best for length generalization. My PE ranking reproduces this.
- **Zhou et al. (2023), *What Algorithms Can Transformers Learn?* (RASP-L)** — predicts which tasks length-generalize based on whether a short, position-agnostic program exists. The right lens for *why* summation behaves the way it does.
- **McLeish et al. (2024), *Transformers Can Do Arithmetic with the Right Embeddings*** — the real Abacus embeddings, which I only approximate.
- **Dziri et al. (2023), *Faith and Fate*** and the **grokking** line (Power et al. 2022; Nanda et al. 2023) — on the limits of compositional generalization and on what mechanistic structure these models actually learn.

If there's a contribution, it's instrumental rather than conceptual: a task with *provable* structure (so "which rule did it pick" and "did it abstract or fit" are not matters of interpretation), a clean separation of faithful execution from abstraction, the documented fragility of the one positive case, and the verifier-starvation observation in self-discovery. That's a blog post, not a NeurIPS paper, and I think that's the correct calibration.

## Limitations

One operation (summation), one modulus, one model scale regime, short lengths, and a single positive condition (NoPE) that is itself unstable. The fine-grained cliff sweep was run on RoPE, not NoPE, so NoPE's exact break point is bracketed (L10–L12), not pinned. Self-discovery used a specific curriculum and a strict full-trace verifier; a softer verifier or a denser curriculum might climb further. None of this speaks directly to large pretrained models, where scale, data diversity, and pretraining priors change the picture; it's a controlled toy designed to isolate one mechanism, and it should be read as exactly that.

## Reproducibility

All code, the stabilized training recipe, the unit-tested positional encodings, the multi-seed sweep, the controls, and the expert-iteration loop are available, along with the exact commands used to produce every number and figure here. The figures in this post are generated directly from the run logs.
