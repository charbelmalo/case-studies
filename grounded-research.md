# Grounded Agentic Research: making LLM research trustworthy by construction

> LLMs bridge; deterministic structures ground.

## What it is

A research method, demonstrated end-to-end in a public artifact:
[metamorphosis-research](https://github.com/charbelmalo/metamorphosis-research) — a one-day,
verification-gated sweep of the literature on memory retention through insect metamorphosis,
culminating in a Perspective-format synthesis paper that derives a novel, falsifiable experimental
prediction.

The method exists because the way most people use LLMs for research fails in a specific,
predictable place. Retrieval is fine; summaries are fine. The failure is in the *connections* —
the high-value step where a model joins two things nobody has joined. An unconstrained model
"discovers" patterns that are artifacts of its own fluency, and delivers them in the same
confident voice as retrieved fact. The fix is architectural, not rhetorical: generation and
assertion are separated, and only deterministic verification moves a claim forward.

## The scaffold

Agentic LLM passes run inside discrete structures that decide what may be asserted:

- **Claim decomposition.** The deliverable is a claim base — one claim per line, individually
  checkable — before it is ever prose. Nine topic modules, each with its own facts file.
- **Epistemic status as a type system.** Every claim carries one tier — `[CONFIRMED]`,
  `[SUPPORTED]`, `[OPEN]`, `[REFUTED]`, `[NOTE]` — assigned against primary literature.
  Synthesis may only load-bear on the top tiers. Nothing is silently promoted.
- **A refutation-first pass.** A dedicated module catalogues what the evidence does *not*
  support: failed replications, methodological criticism, and the three viral claims about the
  topic (the "pupal soup," immune suppression of imaginal discs, "memory outside the brain"),
  each refuted against primary sources with the probable origin of the myth traced.
- **Citations as verifiable objects.** Claims carry DOI/journal/year; a pre-publication gate
  re-checks every entry against DOI resolution and CrossRef metadata, run with probes that would
  fail if the gate were vacuous.
- **Bridging under formal constraints.** The synthesis step is written in explicit discrete
  logic (predicates, a syllogism), and its output must be falsifiable. The result: combining
  Blackiston 2008 (behavioral memory retention through metamorphosis) with Truman et al. 2023
  (single-neuron mushroom-body fate map) yields the Compartment-Resolved Retention Hypothesis —
  a prediction, to the archive's knowledge untested, that ships with the split-Gal4 experiment
  that would kill it.

## The result that matters most

The verification gate caught the method's own failure mode before publication — at scale. An
independent audit re-verified all 33 bibliography entries and all 10 paper references against DOI
resolution and CrossRef metadata. It **confirmed** the load-bearing citations (including the
August 2026 *Science* paper at the heart of the synthesis, checked down to its 20-author list) and
caught systematic identifier confabulation in the machine-compiled draft: 15 wrong-or-missing
DOIs — eight of which resolved to *real but unrelated papers*, the failure a link-checker cannot
see — three fabricated titles, and two corrupted author lists, one of which mangled a Nobel
laureate's name. Eighteen entries were corrected against publisher records; every catch is
disclosed in the repo rather than hidden. In every corrected case the intended paper is real and
matches the claim made on it — the confabulation lived in identifier metadata, exactly where
fluency is least trustworthy.

That is the honest version of an anti-hallucination claim. Not "my system doesn't hallucinate" —
no such system exists — but: *the process assumes the model will confabulate, and is built so that
confabulation is a detectable, correctable event instead of a silent one.*

## The same architecture in software

This is one thesis applied across domains — an LLM's output is only as trustworthy as the
deterministic structure it must pass through:

| Structure | Where it runs | Evidence |
|---|---|---|
| Deterministic organization, LLM only names | [nebulai](https://github.com/charbelmalo/nebulai) — UMAP + HDBSCAN structure SAE features; the LLM's role is bounded to naming clusters | 50,000+ feature nodes at 60 FPS |
| Cross-version behavioral diffing | [Semantha](https://huggingface.co/spaces/charbel-malo/Semantha) — SAE feature-set diffs between models | ~2,838 LOC, running |
| Memory maturity ladder | Mind/Iris — retrieval never cites a draft-tier note as fact; one gated promotion pipeline | see [Mind/Iris study](mind-iris.md) |
| Schema-constrained generation | [cox-protocol-package](https://github.com/charbelmalo/cox-protocol-package) — JSON-Schema contracts around agent output | 84% adoption, 35+ engineers; −40% visual-regression bugs |
| Checkpoints inside execution | Human–Agent Experience Layer steering work | 4× more effective than post-hoc review; schema boundaries eliminated 80% of regressions |

## Honest limitations

- **The paper is not peer-reviewed** and says so on its face (provenance banner, no venue, no
  DOI). It demonstrates a method; it does not claim standing in the neuroscience literature.
- **The novel prediction is untested.** Its value claim is precision and falsifiability, not truth.
- **One operator.** The scaffold has been run by its author; no study of whether other operators
  get the same verification quality.
- **Verification depth is citation-level, not experiment-level.** The gate checks that claims
  trace to real, correctly-cited primary sources — it cannot re-run the underlying science.
