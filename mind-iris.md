# Mind / Iris: a local-first agent harness and personal-context system

> A local-first personal context and agent-harness system combining MCP, scored
> retrieval, hierarchical memory, RAG and multiple inspection interfaces.

## What it is

Mind is the knowledge system I run my own work out of — 5,477 tracked files under
self-hosted git, indexed for semantic retrieval, mounted into every coding agent I use.
I depend on it daily, so its failure modes are designed rather than discovered.

- **A custom MCP server** — one Python file (~70 KB) exposing search, read, similarity,
  rating, promotion and export to any MCP client. The agent-facing API.
- **A vector index** on `mxbai-embed-large` at 1024 dimensions — 3,015 sources
  decomposed into 12,177 chunks: notes, extracted text, image captions, video, 3D.
- **A Rust/Ratatui TUI** (`mind-tui`) for keyboard-driven navigation, and a
  **Python/FastAPI review dashboard** for validating what the index extracted.
- **A native macOS preview agent** (`MindPreview.xcodeproj`) rendering any indexed asset
  in a floating Quick Look-style panel — glTF, USD, video, PDF, image.

## Architecture

**Memory is a maturity ladder, not a bucket.** Every note carries
`status: draft | review | verified | core`. Core memory only ever holds `verified` or
`core`; promotion runs through a gated pipeline; drafts never auto-promote, and
*retrieval never cites a draft as fact*. A context store that treats every note as equally
true is a machine for laundering your own half-formed guesses back to you with the
authority of a citation — and an agent reading it inherits the confidence without the
provenance.

**Retrieval fails closed rather than degrading.** The embedding model is pinned and the
fallback path is exact-dimension-gated with no silent substitution, so a model swap that
would poison the index refuses to run. File access takes the same posture: read-only by
default, never delete, back up before modify.

**Inspection is a first-class surface, in three shapes** — because the honest problem
with RAG over a personal corpus is that you cannot tell whether it is working. The
dashboard opens the vector DB strictly read-only and writes triage decisions to a sidecar
SQLite file that is safe to delete; it previews the asset inline beside the extracted
chunks, so a bad extraction is visible rather than inferred. Status promotion is the only
action that edits a vault file, and the UI does not perform it — it shells out to one
gatekeeper, forward-only and logged.

## What I built versus what agents built

I architected the system, specified behavior, directed agentic implementation, evaluated
outputs, resolved failures and owned the final product. Mine as decisions: the memory
maturity model and its promotion gate; the fail-closed safety posture; the sidecar review
database instead of mutating the index; the dimension gate on embedding fallback; the
three-surface inspection strategy; and the rule that the preview agent never takes focus.
Agents wrote much of the implementation under those constraints — the part worth
interviewing me about.

## Design decisions and trade-offs

**Privacy-first local inference over quality.** Embedding, retrieval and preview
generation all run on my own hardware. That costs me the best available models and
managed-service reliability, and buys the only property that makes a *personal* context
system usable: I can put things in it I would never send to a third-party API. I would
not make this trade for a team product.

**Human-in-the-loop triage over an automated quality score.** The dashboard is
keyboard-first (`j`/`k` to move, `a` approve, `f` flag) because first-pass index
validation is a throughput problem, not a judgment problem — so I optimized the human
loop rather than a classifier I would have to trust.

**The preview agent never steals focus.** It is an `LSUIElement` background agent with a
`.nonactivatingPanel`, and it frees SceneKit/AV content on dismiss so the resident process
stays light. A context system that interrupts you to show you your own notes has inverted
its purpose.

## Evaluation and limitations

Genuinely evaluated: extraction correctness, per document, by a human, with approve/flag
state persisted. Failure containment is structural — read-only index handle, allowlisted
paths, refused directory classes, one promotion gatekeeper with an audit log. Latency is
managed by design rather than measured: render-on-demand, cached thumbnails and
conversions, a bounded worker queue.

Not evaluated: **no offline retrieval benchmark** — no nDCG, no recall@k, no held-out
query set, no model-routing or latency numbers. Building that harness is next.

### Honest limitations

- **One user.** Every ergonomics claim is an n-of-1 claim.
- **No retrieval metrics.** Structural safety is proven; ranking quality is asserted from
  daily use.
- **Source is private**, on self-hosted git. Available for a live walkthrough.
- **Two failures I disclosed rather than hid.** Newly installed Quick Look extensions are
  registered but disabled until a user enables them in System Settings — the rendering
  code is complete, the gate cannot be forced. And `.gltf` previews inside those sandboxed
  extensions frequently fail, because the sandbox grants read access to the previewed file
  only while `.gltf` references sibling buffers. Both ship as warnings.

## Why this matters for agent products

Agent harnesses converge on four problems: what context an agent can reach, how it is
ranked, what it may change, and how a human sees any of it. Mind answers all four
end-to-end — MCP as the boundary, a maturity ladder as the trust model, fail-closed
defaults as the safety model, and purpose-built inspection surfaces, because an agent's
context is not debuggable if you cannot look at it. Those are the decisions a team-scale
memory, skills or context product has to make.
