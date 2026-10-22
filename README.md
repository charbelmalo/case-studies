# Case studies — Charbel Malo

Senior AI Design Engineer / AI Product Prototyper specializing in human–AI interaction, agentic systems, cognitive interfaces, and spatial creative tools.

I design and build AI-native products that turn opaque model behavior, agent state and large semantic datasets into understandable human experiences.

---

## The studies

| Study | One line | Source availability |
|---|---|---|
| [Grounded Agentic Research](grounded-research.md) | A method for making LLM-driven research trustworthy by construction — claim-level epistemic tiers, refutation-first passes, deterministic citation gates — demonstrated end-to-end in a public artifact | **Public** — [metamorphosis-research](https://github.com/charbelmalo/metamorphosis-research) |
| [Mind / Iris](mind-iris.md) | A local-first personal context and agent-harness system combining MCP, scored retrieval, hierarchical memory, RAG and multiple inspection interfaces | Private (self-hosted git) — available for walkthrough |
| [Weld](weld.md) | A native macOS menu-bar writing surface that watches the system-wide focused text field via the Accessibility APIs, built by directing a multi-agent cluster | Private (self-hosted git) — available for walkthrough |
| [Magmaflow](magmaflow.md) | An AI-native spatial creative environment with depth-aware layers, shaders, particles, 3D assets and agent-readable project state | Private fork; **the live app is the artifact** |

Each study includes an explicit "Honest limitations" section. Where a claim is not
measured, it says so.

---

## Public artifacts you can open right now

| Artifact | What it is | Link |
|---|---|---|
| **Magmaflow (live)** | The spatial creative environment, running | <https://design.charbelmalo.online> |
| **nebulai** | WebGPU atlas of what small language models learned — tokens / SAE features / MLP neurons → UMAP → HDBSCAN → auto-naming → WebGPU map (WebGL2 fallback). A 49,857-point GPT-2 token map stays interactive on a laptop; nine models mapped, Pythia-70m to Muse-Glimmer-30B, read over HTTP range requests; every map ships with its validation numbers. | <https://github.com/charbelmalo/nebulai> |
| **cox-protocol-package** | npm CLI + JSON-Schema spec for agent-readable UI/design contracts. 84% adoption across 35+ engineers and designers at VML Middle East; 40% reduction in visual-regression bugs. | [repo](https://github.com/charbelmalo/cox-protocol-package) · [npm](https://www.npmjs.com/package/cox-protocol-package) |
| **PrismaticShadersPack** | 9 effects authored three times over — WebGL2 GLSL, WGSL and Metal MSL — across 28 files. | <https://github.com/charbelmalo/PrismaticShadersPack> |
| **Semantha** | SAE-diffing client for comparing sparse-autoencoder features between models (~2,838 LOC). | <https://huggingface.co/spaces/charbel-malo/Semantha> |
| **metamorphosis-research** | A one-day, verification-gated research sweep (9 modules, 34-entry audit-verified bibliography, claim-level epistemic tiers) plus a synthesis paper deriving a novel falsifiable prediction — the Grounded Agentic Research demonstration. | <https://github.com/charbelmalo/metamorphosis-research> |

Related background: *The Human–Agent Experience Layer* — steerable interfaces,
deterministic boundaries and trust protocols for enterprise AI adoption (write-up
available on request). Its headline finding: human-in-the-loop steering is **4× more effective at
decision checkpoints during agent execution** than post-hoc review of finished output,
and schema-constrained tool boundaries eliminated **80% of regressions**.

---

## What is *not* claimed anywhere in this repo

- External users or adoption numbers for Mind, Weld or Magmaflow. They have one user.
- Production scale beyond internal tooling at VML Middle East.
- Published research papers.

The metrics that are cited (cox-protocol adoption, nebulai's validation numbers, the 4×
steering result, the 80% regression reduction) come from work with real users or real
measurement. The rest is described as what it is: built, running, unmeasured.

---

## Contact

- **Email** — <charbel.work@outlook.com>
- **LinkedIn** — <https://linkedin.com/in/charbel-malo-b96857206>
- **Portfolio** — <https://charbelmalo.online>
- **GitHub** — <https://github.com/charbelmalo>
- **Hugging Face** — <https://huggingface.co/charbel-malo>
